调用包含关键信息_message_ 的消息处理程序。如果未安装消息处理程序，则将消息打印到 stderr。在 Windows 下，该信息将发送到调试器。在 QNX 系统中，消息将发送到 slogger2。

该函数接收格式字符串和参数列表，类似于 C 语言的 printf()函数。格式应为拉丁字母 1 字符串。

```cpp
void load(const QString &fileName)
{
    QFile file(fileName);
    if (!file.exists())
        qCritical("File '%s' does not exist!", qUtf8Printable(fileName));
}
```

如果包含 `<QtDebug>`，还可以使用更方便的语法：

```cpp
qCritical() << "Brush:" << myQBrush << "Other value:" << i;
```

在项目之间插入空格，并在末尾添加换行符。

要在运行时抑制输出，可以定义logging rules 或注册一个自定义的filter 。

出于调试目的，有时让程序终止处理关键信息也很方便。这样就可以检查核心转储，或附加调试器--另请参阅qFatal() 。要启用此功能，可将环境变量`QT_FATAL_CRITICALS` 设为一个数字`n` 。这样，程序就会在出现第 n 条关键信息时终止。也就是说，如果环境变量设置为 1，程序将在第一次调用时终止；如果环境变量的值为 10，程序将在第 10 次调用时退出。环境变量中的任何非数值都等同于 1。
