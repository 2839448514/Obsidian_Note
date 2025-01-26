```c#
using UnityEngine;
#if ENABLE_INPUT_SYSTEM
using UnityEngine.InputSystem;
#endif

// StarterAssetsInputs 脚本处理角色输入，允许用户通过新输入系统（如果启用）进行控制
public class StarterAssetsInputs : MonoBehaviour
{
    // ===============================
    // 输入数据
    // ===============================

    [Header("Character Input Values")]
    // 玩家输入的移动方向（水平和垂直）
    public Vector2 move;

    // 玩家输入的视角方向（鼠标的移动方向）
    public Vector2 look;

    // 玩家是否按下跳跃键
    public bool jump;

    // 玩家是否按下冲刺键
    public bool sprint;

    // ===============================
    // 移动设置
    // ===============================

    [Header("Movement Settings")]
    // 是否启用模拟的模拟移动（模拟输入的方式），例如左摇杆控制
    public bool analogMovement;

    // ===============================
    // 鼠标设置
    // ===============================

    [Header("Mouse Cursor Settings")]
    // 控制鼠标是否被锁定在屏幕中
    public bool cursorLocked = true;

    // 控制鼠标是否用于视角控制
    public bool cursorInputForLook = true;

    // ===============================
    // 输入系统相关的函数
    // ===============================

#if ENABLE_INPUT_SYSTEM
    // 如果启用了新的输入系统，这些函数将与 Unity 的 Input System 交互

    // 处理移动输入
    public void OnMove(InputValue value)
    {
        // 获取并设置移动方向
        MoveInput(value.Get<Vector2>());
    }

    // 处理视角输入
    public void OnLook(InputValue value)
    {
        // 如果启用了鼠标输入控制视角，则更新视角方向
        if (cursorInputForLook)
        {
            LookInput(value.Get<Vector2>());
        }
    }

    // 处理跳跃输入
    public void OnJump(InputValue value)
    {
        // 设置跳跃状态
        JumpInput(value.isPressed);
    }

    // 处理冲刺输入
    public void OnSprint(InputValue value)
    {
        // 设置冲刺状态
        SprintInput(value.isPressed);
    }
#endif

    // ===============================
    // 处理输入的逻辑
    // ===============================

    // 处理移动输入
    public void MoveInput(Vector2 newMoveDirection)
    {
        move = newMoveDirection;
    }

    // 处理视角输入
    public void LookInput(Vector2 newLookDirection)
    {
        look = newLookDirection;
    }

    // 处理跳跃输入
    public void JumpInput(bool newJumpState)
    {
        jump = newJumpState;
    }

    // 处理冲刺输入
    public void SprintInput(bool newSprintState)
    {
        sprint = newSprintState;
    }

    // ===============================
    // 鼠标控制
    // ===============================

    // 处理应用程序焦点变化（例如窗口最小化或恢复时）
    private void OnApplicationFocus(bool hasFocus)
    {
        // 确保在应用程序获得焦点时，鼠标锁定状态正确
        SetCursorState(cursorLocked);
    }

    // 设置鼠标的锁定状态
    private void SetCursorState(bool newState)
    {
        // 根据 cursorLocked 的值锁定或解锁鼠标
        Cursor.lockState = newState ? CursorLockMode.Locked : CursorLockMode.None;
    }
}
```