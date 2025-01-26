```c#
using UnityEngine;

public class BasicRigidBodyPush : MonoBehaviour
{
    // 设置可推物体的层级
    public LayerMask pushLayers;

    // 控制是否可以推物体
    public bool canPush;

    // 推动物体的力度（范围从 0.5 到 5，默认是 1.1）
    [Range(0.5f, 5f)] public float strength = 1.1f;

    // 当角色控制器与其他物体发生碰撞时被调用
    private void OnControllerColliderHit(ControllerColliderHit hit)
    {
        // 如果可以推物体，则调用推物体的方法
        if (canPush) PushRigidBodies(hit);
    }

    // 处理推动刚体的逻辑
    private void PushRigidBodies(ControllerColliderHit hit)
    {
        // Unity 官方文档解释：当角色控制器碰撞到其他物体时，此方法会触发
        // https://docs.unity3d.com/ScriptReference/CharacterController.OnControllerColliderHit.html

        // 获取碰撞物体的刚体（如果有的话）
        Rigidbody body = hit.collider.attachedRigidbody;

        // 如果没有刚体或者该刚体是运动学的（不能通过物理力推动），则跳过
        if (body == null || body.isKinematic) return;

        // 确保我们只推动指定层级的物体
        var bodyLayerMask = 1 << body.gameObject.layer;
        if ((bodyLayerMask & pushLayers.value) == 0) return;

        // 防止推动位于我们下面的物体（例如地面或其他低于角色的物体）
        if (hit.moveDirection.y < -0.3f) return;

        // 计算推动的方向，仅考虑水平运动（忽略垂直方向）
        Vector3 pushDir = new Vector3(hit.moveDirection.x, 0.0f, hit.moveDirection.z);

        // 根据设定的推力（strength）将力应用到刚体
        body.AddForce(pushDir * strength, ForceMode.Impulse);
    }
}

```