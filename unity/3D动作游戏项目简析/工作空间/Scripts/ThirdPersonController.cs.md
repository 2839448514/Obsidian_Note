```C#
using UnityEngine;
#if ENABLE_INPUT_SYSTEM 
using UnityEngine.InputSystem;
#endif

/* 注意：动画通过控制器调用，包括角色和胶囊体的动画，使用了 Animator 的空检查
 */

[RequireComponent(typeof(CharacterController))]
#if ENABLE_INPUT_SYSTEM 
[RequireComponent(typeof(PlayerInput))]
#endif
public class ThirdPersonController : MonoBehaviour
{
    [Header("玩家设置")]
    [Tooltip("角色的移动速度（单位：米/秒）")]
    public float MoveSpeed = 2.0f;

    [Tooltip("角色的奔跑速度（单位：米/秒）")]
    public float SprintSpeed = 5.335f;

    [Tooltip("角色转向运动方向的速度")]
    [Range(0.0f, 0.3f)]
    public float RotationSmoothTime = 0.12f;

    [Tooltip("加速和减速的速率")]
    public float SpeedChangeRate = 10.0f;

    // 音效设置
    public AudioClip LandingAudioClip;  // 跳跃落地音效
    public AudioClip[] FootstepAudioClips;  // 步伐音效
    [Range(0, 1)] public float FootstepAudioVolume = 0.5f;  // 步伐音效的音量

    [Space(10)]
    [Tooltip("角色可以跳跃的高度")]
    public float JumpHeight = 1.2f;

    [Tooltip("角色使用自己的重力值，默认是 -9.81f")]
    public float Gravity = -15.0f;

    [Space(10)]
    [Tooltip("重新跳跃前的等待时间。设为 0f 时可以立即再次跳跃")]
    public float JumpTimeout = 0.50f;

    [Tooltip("进入下落状态前的等待时间。适用于下楼梯时")]
    public float FallTimeout = 0.15f;

    [Header("玩家是否着陆")]
    [Tooltip("角色是否在地面上。不是 CharacterController 内建的着陆检测")]
    public bool Grounded = true;

    [Tooltip("用于粗糙地面")]
    public float GroundedOffset = -0.14f;

    [Tooltip("着陆检测的半径，应该与 CharacterController 的半径匹配")]
    public float GroundedRadius = 0.28f;

    [Tooltip("角色所用的地面层")]
    public LayerMask GroundLayers;

    [Header("Cinemachine 摄像机设置")]
    [Tooltip("Cinemachine 虚拟摄像机中设置的跟随目标，摄像机将跟随这个目标")]
    public GameObject CinemachineCameraTarget;

    [Tooltip("摄像机可以向上移动的最大角度")]
    public float TopClamp = 70.0f;

    [Tooltip("摄像机可以向下移动的最小角度")]
    public float BottomClamp = -30.0f;

    [Tooltip("用于覆盖摄像机的额外角度。对于锁定摄像机位置时的微调很有用")]
    public float CameraAngleOverride = 0.0f;

    [Tooltip("是否锁定摄像机的位置")]
    public bool LockCameraPosition = false;

    // Cinemachine
    private float _cinemachineTargetYaw;  // 摄像机目标的水平角度
    private float _cinemachineTargetPitch;  // 摄像机目标的垂直角度

    // 玩家控制相关
    private float _speed;  // 当前速度
    private float _animationBlend;  // 动画混合
    private float _targetRotation = 0.0f;  // 目标旋转角度
    private float _rotationVelocity;  // 旋转速度
    private float _verticalVelocity;  // 垂直速度
    private float _terminalVelocity = 53.0f;  // 终极速度（防止角色掉得太快）

    // 超时计时器
    private float _jumpTimeoutDelta;  // 跳跃超时计时器
    private float _fallTimeoutDelta;  // 下落超时计时器

    // 动画 ID
    private int _animIDSpeed;
    private int _animIDGrounded;
    private int _animIDJump;
    private int _animIDFreeFall;
    private int _animIDMotionSpeed;

#if ENABLE_INPUT_SYSTEM 
    private PlayerInput _playerInput;  // 输入管理
#endif
    private Animator _animator;  // 动画控制器
    private CharacterController _controller;  // 角色控制器
    private StarterAssetsInputs _input;  // 输入系统
    private GameObject _mainCamera;  // 主摄像机引用

    private const float _threshold = 0.01f;  // 输入阈值

    private bool _hasAnimator;  // 判断是否有动画控制器

    // 判断当前设备是否是鼠标
    private bool IsCurrentDeviceMouse
    {
        get
        {
#if ENABLE_INPUT_SYSTEM
            return _playerInput.currentControlScheme == "KeyboardMouse";
#else
			return false;
#endif
        }
    }

    private void Awake()
    {
        // 获取主摄像机的引用
        if (_mainCamera == null)
        {
            _mainCamera = GameObject.FindGameObjectWithTag("MainCamera");
        }
    }

    private void Start()
    {
        _cinemachineTargetYaw = CinemachineCameraTarget.transform.rotation.eulerAngles.y;  // 设置摄像机的初始角度

        _hasAnimator = TryGetComponent(out _animator);  // 检查是否有 Animator 组件
        _controller = GetComponent<CharacterController>();  // 获取角色控制器
        _input = GetComponent<StarterAssetsInputs>();  // 获取输入组件
#if ENABLE_INPUT_SYSTEM 
        _playerInput = GetComponent<PlayerInput>();  // 获取 PlayerInput 组件
#else
		Debug.LogError("Starter Assets 包缺少依赖项。请使用 Tools/Starter Assets/Reinstall Dependencies 来修复");
#endif

        AssignAnimationIDs();  // 分配动画ID

        // 在开始时重置超时计时器
        _jumpTimeoutDelta = JumpTimeout;
        _fallTimeoutDelta = FallTimeout;
    }

    private void Update()
    {
        _hasAnimator = TryGetComponent(out _animator);  // 确保动画控制器存在

        JumpAndGravity();  // 处理跳跃和重力
        GroundedCheck();  // 检查是否在地面
        Move();  // 控制角色移动
    }

    private void LateUpdate()
    {
        CameraRotation();  // 摄像机旋转
    }

    private void AssignAnimationIDs()
    {
        // 通过 Animator 动画控制器的名称来生成 ID
        _animIDSpeed = Animator.StringToHash("Speed");
        _animIDGrounded = Animator.StringToHash("Grounded");
        _animIDJump = Animator.StringToHash("Jump");
        _animIDFreeFall = Animator.StringToHash("FreeFall");
        _animIDMotionSpeed = Animator.StringToHash("MotionSpeed");
    }

    private void GroundedCheck()
    {
        // 设置球形位置，带有偏移量
        Vector3 spherePosition = new Vector3(transform.position.x, transform.position.y - GroundedOffset,
            transform.position.z);
        Grounded = Physics.CheckSphere(spherePosition, GroundedRadius, GroundLayers,
            QueryTriggerInteraction.Ignore);

        // 如果使用动画控制器，更新动画状态
        if (_hasAnimator)
        {
            _animator.SetBool(_animIDGrounded, Grounded);
        }
    }

    private void CameraRotation()
    {
        // 如果有输入且摄像机位置没有被锁定
        if (_input.look.sqrMagnitude >= _threshold && !LockCameraPosition)
        {
            // 不要将鼠标输入乘以 Time.deltaTime；
            float deltaTimeMultiplier = IsCurrentDeviceMouse ? 1.0f : Time.deltaTime;

            _cinemachineTargetYaw += _input.look.x * deltaTimeMultiplier;  // 更新水平角度
            _cinemachineTargetPitch += _input.look.y * deltaTimeMultiplier;  // 更新垂直角度
        }

        // 限制旋转角度，使其在 360 度内
        _cinemachineTargetYaw = ClampAngle(_cinemachineTargetYaw, float.MinValue, float.MaxValue);
        _cinemachineTargetPitch = ClampAngle(_cinemachineTargetPitch, BottomClamp, TopClamp);

        // Cinemachine 将跟随这个目标
        CinemachineCameraTarget.transform.rotation = Quaternion.Euler(_cinemachineTargetPitch + CameraAngleOverride,
            _cinemachineTargetYaw, 0.0f);
    }

    private void Move()
    {
        // 根据移动速度、奔跑速度以及是否按下奔跑键来设置目标速度
        float targetSpeed = _input.sprint ? SprintSpeed : MoveSpeed;

        // 一个简单的加速和减速设计，易于修改或替换
        if (_input.move == Vector2.zero) targetSpeed = 0.0f;  // 如果没有输入，速度设为 0

        // 获取玩家当前的水平速度
        float currentHorizontalSpeed = new Vector3(_controller.velocity.x, 0.0f, _controller.velocity.z).magnitude;

        float speedOffset = 0.1f;
        float inputMagnitude = _input.analogMovement ? _input.move.magnitude : 1f;  // 获取输入方向的幅度

        // 加速或减速至目标速度
        if (currentHorizontalSpeed < targetSpeed - speedOffset ||
            currentHorizontalSpeed > targetSpeed + speedOffset)
        {
            // 创建一个曲线结果，而不是线性变化，从而使速度变化更自然
            _speed = Mathf.Lerp(currentHorizontalSpeed, targetSpeed * inputMagnitude,
                Time.deltaTime * SpeedChangeRate);

            // 将速度四舍五入到小数点后三位
            _speed = Mathf.Round(_speed * 1000f) / 1000f;
        }
        else
        {
            _speed = targetSpeed;  // 如果速度在范围内，直接使用目标速度
        }

        _animationBlend = Mathf.Lerp(_animationBlend, targetSpeed, Time.deltaTime * SpeedChangeRate);
        if (_animationBlend < 0.01f) _animationBlend = 0f;  // 保证动画的平滑

        // 标准化输入方向
        Vector3 inputDirection = new Vector3(_input.move.x, 0.0f, _input.move.y).normalized;

        // 如果有移动输入，且角色在移动时旋转
        if (_input.move != Vector2.zero)
        {
            _targetRotation = Mathf.Atan2(inputDirection.x, inputDirection.z) * Mathf.Rad2Deg +
                              _mainCamera.transform.eulerAngles.y;
            float rotation = Mathf.SmoothDampAngle(transform.eulerAngles.y, _targetRotation, ref _rotationVelocity,
                RotationSmoothTime);

            // 根据摄像机的角度旋转角色
            transform.rotation = Quaternion.Euler(0.0f, rotation, 0.0f);
        }

        Vector3 targetDirection = Quaternion.Euler(0.0f, _targetRotation, 0.0f) * Vector3.forward;

        // 移动玩家
        _controller.Move(targetDirection.normalized * (_speed * Time.deltaTime) +
                         new Vector3(0.0f, _verticalVelocity, 0.0f) * Time.deltaTime);

        // 如果使用动画控制器，更新动画状态
        if (_hasAnimator)
        {
            _animator.SetFloat(_animIDSpeed, _animationBlend);
            _animator.SetFloat(_animIDMotionSpeed, inputMagnitude);
        }
    }

    private void JumpAndGravity()
    {
        if (Grounded)
        {
            // 重置下落超时计时器
            _fallTimeoutDelta = FallTimeout;

            // 如果使用动画控制器，更新动画状态
            if (_hasAnimator)
            {
                _animator.SetBool(_animIDJump, false);
                _animator.SetBool(_animIDFreeFall, false);
            }

            // 停止角色在地面时无限下落
            if (_verticalVelocity < 0.0f)
            {
                _verticalVelocity = -2f;
            }

            // 跳跃
            if (_input.jump && _jumpTimeoutDelta <= 0.0f)
            {
                // 使用平方根公式计算跳跃需要的速度
                _verticalVelocity = Mathf.Sqrt(JumpHeight * -2f * Gravity);

                // 如果使用动画控制器，更新动画状态
                if (_hasAnimator)
                {
                    _animator.SetBool(_animIDJump, true);
                }
            }

            // 跳跃超时
            if (_jumpTimeoutDelta >= 0.0f)
            {
                _jumpTimeoutDelta -= Time.deltaTime;
            }
        }
        else
        {
            // 重置跳跃超时计时器
            _jumpTimeoutDelta = JumpTimeout;

            // 下落超时
            if (_fallTimeoutDelta >= 0.0f)
            {
                _fallTimeoutDelta -= Time.deltaTime;
            }
            else
            {
                // 如果使用动画控制器，更新动画状态
                if (_hasAnimator)
                {
                    _animator.SetBool(_animIDFreeFall, true);
                }
            }

            // 如果没有着陆，不允许跳跃
            _input.jump = false;
        }

        // 如果垂直速度小于终极速度，则应用重力
        if (_verticalVelocity < _terminalVelocity)
        {
            _verticalVelocity += Gravity * Time.deltaTime;
        }
    }

    // 限制角度在一个范围内
    private static float ClampAngle(float lfAngle, float lfMin, float lfMax)
    {
        if (lfAngle < -360f) lfAngle += 360f;
        if (lfAngle > 360f) lfAngle -= 360f;
        return Mathf.Clamp(lfAngle, lfMin, lfMax);
    }

    // 在选中物体时绘制 Gizmo，显示地面检测半径
    private void OnDrawGizmosSelected()
    {
        Color transparentGreen = new Color(0.0f, 1.0f, 0.0f, 0.35f);
        Color transparentRed = new Color(1.0f, 0.0f, 0.0f, 0.35f);

        if (Grounded) Gizmos.color = transparentGreen;
        else Gizmos.color = transparentRed;

        // 选中时，在角色位置画出一个与地面半径匹配的 Gizmo
        Gizmos.DrawSphere(
            new Vector3(transform.position.x, transform.position.y - GroundedOffset, transform.position.z),
            GroundedRadius);
    }

    // 步伐音效事件
    private void OnFootstep(AnimationEvent animationEvent)
    {
        if (animationEvent.animatorClipInfo.weight > 0.5f)
        {
            if (FootstepAudioClips.Length > 0)
            {
                var index = Random.Range(0, FootstepAudioClips.Length);
                AudioSource.PlayClipAtPoint(FootstepAudioClips[index], transform.TransformPoint(_controller.center), FootstepAudioVolume);
            }
        }
    }

    // 跳跃落地音效
    private void OnLand(AnimationEvent animationEvent)
    {
        if (animationEvent.animatorClipInfo.weight > 0.5f)
        {
            AudioSource.PlayClipAtPoint(LandingAudioClip, transform.TransformPoint(_controller.center), FootstepAudioVolume);
        }
    }
}

```