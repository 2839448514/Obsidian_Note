`Intent` 是 Android 中用于启动组件（如 `Activity`、`Service`、`BroadcastReceiver`）以及传递数据的重要工具。它用于不同组件之间的通信。以下是 `Intent` 类的常用方法及其作用：

### 创建与初始化

1. **`Intent()`**

	- **作用**：默认构造方法，创建一个空的 `Intent` 对象，之后可以通过 `setComponent` 或 `setAction` 来设置具体的目标。
	- **调用顺序**：一般用于空的 `Intent` 初始化。

	```java
    Intent intent = new Intent();
    ```

2. **`Intent(Context packageContext, Class<?> cls)`**

	- **作用**：根据上下文和目标 `Activity` 类来创建一个新的 `Intent`。
	- **调用顺序**：启动 `Activity` 时使用。

	```java
    Intent intent = new Intent(this, TargetActivity.class);
    ```

3. **`Intent(String action)`**

	- **作用**：通过指定 `action` 创建 `Intent`。`action` 是表示要执行的操作的字符串。
	- **调用顺序**：用于创建一个仅包含 `action` 的 `Intent`。

	```java
    Intent intent = new Intent(Intent.ACTION_VIEW);
    ```

4. **`Intent(String action, Uri uri)`**

	- **作用**：通过指定 `action` 和 `uri` 创建 `Intent`。`uri` 通常用来指定一个资源的位置（如文件路径、网页地址等）。
	- **调用顺序**：在需要传递 `URI` 信息时使用。

	```java
    Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://www.example.com"));
    ```

### 设置目标组件

5. **`setComponent(ComponentName component)`**

	- **作用**：设置 `Intent` 的目标组件，通过 `ComponentName` 指定。
	- **调用顺序**：用于设置目标组件的精确名称（`Activity`、`Service`等）。

	```java
    Intent intent = new Intent();
    ComponentName componentName = new ComponentName("com.example", "com.example.TargetActivity");
    intent.setComponent(componentName);
    ```

6. **`setClass(Context packageContext, Class<?> cls)`**

	- **作用**：直接设置目标 `Activity` 类。可以在 `Intent` 中指定目标组件的 `Activity`。
	- **调用顺序**：用于启动指定类的 `Activity`。

	```java
    Intent intent = new Intent(this, TargetActivity.class);
    ```

7. **`setClassName(String packageName, String className)`**

	- **作用**：根据包名和类名设置目标组件。
	- **调用顺序**：用于指定包名和类名来启动特定的 `Activity`。

	```java
    Intent intent = new Intent();
    intent.setClassName("com.example", "com.example.TargetActivity");
    ```

### 设置动作与数据

8. **`setAction(String action)`**

	- **作用**：设置 `Intent` 的动作（例如 `ACTION_VIEW`、`ACTION_SEND` 等），描述 `Intent` 想要执行的操作。
	- **调用顺序**：在 `Intent` 创建时设置操作。

	```java
    Intent intent = new Intent(Intent.ACTION_SEND);
    ```

9. **`setData(Uri data)`**

	- **作用**：设置 `Intent` 的数据部分，通常是一个 `Uri`，用于指定要操作的资源。
	- **调用顺序**：在创建 `Intent` 时设置数据。

	```java
    Intent intent = new Intent(Intent.ACTION_VIEW);
    intent.setData(Uri.parse("https://www.example.com"));
    ```

10. **`setType(String type)`**

	- **作用**：设置 `Intent` 的数据类型，例如 `text/plain` 或 `image/jpeg` 等。
	- **调用顺序**：设置数据类型。

	```java
    Intent intent = new Intent(Intent.ACTION_SEND);
    intent.setType("text/plain");
    ```

11. **`setDataAndType(Uri data, String type)`**

	- **作用**：同时设置 `Intent` 的数据和类型。
	- **调用顺序**：设置数据和类型时使用。

	```java
    Intent intent = new Intent(Intent.ACTION_VIEW);
    intent.setDataAndType(Uri.parse("file:///path/to/file"), "image/jpeg");
    ```

12. **`putExtra(String name, String value)`**

	- **作用**：在 `Intent` 中加入一个字符串类型的额外数据。
	- **调用顺序**：向 `Intent` 中添加数据。

	```java
    Intent intent = new Intent(this, TargetActivity.class);
    intent.putExtra("key", "value");
    ```

13. **`putExtra(String name, int value)`**

	- **作用**：向 `Intent` 中加入一个整型额外数据。
	- **调用顺序**：向 `Intent` 中添加数据。

	```java
    Intent intent = new Intent(this, TargetActivity.class);
    intent.putExtra("key", 123);
    ```

14. **`getStringExtra(String name)`**

	- **作用**：获取通过 `putExtra` 方法传递的字符串数据。
	- **调用顺序**：在接收 `Intent` 时获取数据。

	```java
    String value = intent.getStringExtra("key");
    ```

15. **`getIntExtra(String name, int defaultValue)`**

	- **作用**：获取通过 `putExtra` 方法传递的整型数据。如果没有该数据，返回默认值。
	- **调用顺序**：在接收 `Intent` 时获取数据。

	```java
    int value = intent.getIntExtra("key", 0);
    ```

16. **`getExtras()`**

	- **作用**：获取所有通过 `putExtra` 方法传递的数据。
	- **调用顺序**：在接收 `Intent` 时获取所有附加数据。

	```java
    Bundle extras = intent.getExtras();
    ```

### 启动与返回

17. **`startActivity(Intent intent)`**

	- **作用**：启动一个 `Activity`。
	- **调用顺序**：启动目标 `Activity` 时使用。

	```java
    startActivity(intent);
    ```

18. **`startActivityForResult(Intent intent, int requestCode)`**

	- **作用**：启动一个 `Activity` 并期待返回结果。通过 `requestCode` 标识请求。
	- **调用顺序**：启动 `Activity` 并期待结果时使用。

	```java
    startActivityForResult(intent, REQUEST_CODE);
    ```

19. **`setResult(int resultCode)`**

	- **作用**：设置结果代码。通常与 `startActivityForResult` 配合使用。
	- **调用顺序**：设置 `Activity` 的结果。

	```java
    setResult(RESULT_OK);
    finish();
    ```

20. **`setResult(int resultCode, Intent data)`**

	- **作用**：设置带有数据的结果。
	- **调用顺序**：设置结果并附带返回数据。

	```java
    Intent resultIntent = new Intent();
    resultIntent.putExtra("key", "value");
    setResult(RESULT_OK, resultIntent);
    finish();
    ```

### 其他辅助方法

21. **`getAction()`**

	- **作用**：获取 `Intent` 的操作（action）。
	- **调用顺序**：在使用 `Intent` 时可以获取设置的动作。

	```java
    String action = intent.getAction();
    ```

22. **`getData()`**

	- **作用**：获取 `Intent` 的数据部分。
	- **调用顺序**：在接收 `Intent` 时获取数据。

	```java
    Uri data = intent.getData();
    ```

23. **`getType()`**

	- **作用**：获取 `Intent` 的类型。
	- **调用顺序**：在接收 `Intent` 时获取类型。

	```java
    String type = intent.getType();
    ```

24. **`getPackage()`**

	- **作用**：获取 `Intent` 的包名。
	- **调用顺序**：在接收 `Intent` 时获取包名。

	```java
    String packageName = intent.getPackage();
    ```

### 清单标记与其他设置

25. **`addFlags(int flags)`**

	- **作用**：为 `Intent` 添加标志位。标志位用于指定启动模式，如 `Intent.FLAG_ACTIVITY_NEW_TASK` 等。
	- **调用顺序**：设置启动模式或其他特殊行为。

	```java
    intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    ```

26. **`addCategory(String category)`**

	- **作用**：为 `Intent` 添加类别，用于筛选 `Intent`。
	- **调用顺序**：用于为 `Intent` 添加类别标记。

	```java
    intent.addCategory(Intent.CATEGORY_LAUNCHER);
    ```

以上是 `Intent` 类的常用方法及其作用，涵盖了 `Intent` 的创建、目标设置、数据传递、启动与返回等功能。通过这些方法，开发者可以在应用的不同组件之间进行交互和通信。
