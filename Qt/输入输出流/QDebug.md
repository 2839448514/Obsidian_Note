需要将调试或跟踪信息写入设备、文件、字符串或控制台时，就会用到 QDebug。

### 基本用法

在一般情况下，调用qDebug() 函数获得一个默认的 QDebug 对象，用于写入调试信息。

```c++
qDebug() << "Date:" << QDate::currentDate();
qDebug() << "Types:" << QString("String") << QChar('x') << QRect(0, 10, 50, 40);
qDebug() << "Custom coordinate type:" << coordinate;
```

这将使用构造函数构造一个 `QDebug` 对象，该构造函数接受`QtMsgType` 值`QtDebugMsg` 。类似地，**`qWarning()`,`qCritical()` 和`qFatal()`** 函数也返回相应消息类型的 `QDebug` 对象。

该类还为其他情况提供了几个构造函数，包括一个接受`QFile` 或任何其他`QIODevice` 子类的构造函数，该子类用于将调试信息写入文件和其他设备。接受`QString `的构造函数用于写入字符串，以便显示或序列化。

### 格式化选项

`QDebug` 会格式化输出，使其易于阅读。它会自动在参数之间添加空格，并在`QString`,`QByteArray`,`QChar` 参数周围添加引号。

您可以通过`space()`,`nospace() `和`quote()`,`noquote()` 方法调整这些选项。此外，`QTextStream manipulators` 还可以通过管道导入 `QDebug` 流。

`QDebugStateSaver` 限制在当前范围内更改格式。() 将选项重置为默认选项。

### 将自定义类型写入流

```cpp
QDebug operator<<(QDebug debug, const Coordinate &c)
{
    QDebugStateSaver saver(debug);
    debug.nospace() << '(' << c.x() << ", " << c.y() << ')';

    return debug;
}
```
