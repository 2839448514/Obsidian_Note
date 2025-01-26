调用带有致命消息的消息处理程序_message_ 。如果未安装消息处理程序，则将消息打印到 stderr。在 Windows 下，该消息将发送到调试器。在 QNX 系统中，消息将发送到 slogger2。

如果使用的是**默认消息处理程序**，该函数将终止以创建核心转储。在 Windows 系统中，对于调试程序，该函数将报告 _CRT_ERROR，以便将调试器连接到应用程序。

该函数接收格式字符串和参数列表，类似于 C 语言的 printf() 函数。

```cpp
int divide(int a, int b)
{
    if (b == 0)                                // program error
        qFatal("divide: cannot divide by zero");
    return a / b;
}
```
