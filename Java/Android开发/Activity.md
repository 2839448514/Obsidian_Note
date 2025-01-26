`Activity` 类是 Android 应用程序中一个重要的组件，涉及用户界面和生命周期管理。下面是 `Activity` 类中的所有方法，以及它们的作用。每个方法与 `Activity` 的生命周期密切相关，通过这些方法，开发者可以控制 `Activity` 的创建、暂停、恢复、销毁等操作。

### 基本生命周期方法

1. **`onCreate(Bundle savedInstanceState)`**
    
    - **作用**：`Activity` 创建时调用。用于初始化 `Activity`，如设置布局、初始化控件等。
    - **调用顺序**：生命周期的开始。
    
    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
    ```
    
2. **`onStart()`**
    
    - **作用**：`Activity` 变为可见时调用。此时 `Activity` 被加载到屏幕上，但还没有与用户交互。
    - **调用顺序**：`onCreate()` 后，`onResume()` 前。
    
    ```java
    @Override
    protected void onStart() {
        super.onStart();
        // 初始化时需要做的操作
    }
    ```
    
3. **`onResume()`**
    
    - **作用**：`Activity` 即将开始与用户交互时调用。此时 `Activity` 位于前台。
    - **调用顺序**：`onStart()` 后，`Activity` 成为前台。
    
    ```java
    @Override
    protected void onResume() {
        super.onResume();
        // 恢复暂停时的状态，如开始动画、注册监听器等
    }
    ```
    
4. **`onPause()`**
    
    - **作用**：当系统准备启动或恢复另一个 `Activity` 时调用。`Activity` 不再处于前台。
    - **调用顺序**：`onResume()` 后，`onStop()` 前。
    
    ```java
    @Override
    protected void onPause() {
        super.onPause();
        // 保存数据、停止动画等
    }
    ```
    
5. **`onStop()`**
    
    - **作用**：当 `Activity` 不再对用户可见时调用。此时 `Activity` 已不再在屏幕上显示。
    - **调用顺序**：`onPause()` 后，`onDestroy()` 前。
    
    ```java
    @Override
    protected void onStop() {
        super.onStop();
        // 停止任何耗时的操作或释放资源
    }
    ```
    
6. **`onRestart()`**
    
    - **作用**：当 `Activity` 从停止状态恢复时调用。此方法在 `onStart()` 之前调用。
    - **调用顺序**：`onStop()` 后，`onStart()` 前。
    
    ```java
    @Override
    protected void onRestart() {
        super.onRestart();
        // 恢复暂停时的数据
    }
    ```
    
7. **`onDestroy()`**
    
    - **作用**：当 `Activity` 即将被销毁时调用。通常用于清理资源、注销广播接收器等。
    - **调用顺序**：`onStop()` 后，生命周期的最后。
    
    ```java
    @Override
    protected void onDestroy() {
        super.onDestroy();
        // 清理资源、注销监听器等
    }
    ```
    

### 状态保存和恢复方法

8. **`onSaveInstanceState(Bundle outState)`**
    
    - **作用**：当 `Activity` 即将被销毁或系统需要回收时调用，用于保存必要的数据（例如输入框中的文本、滚动位置等）。
    - **调用顺序**：在 `onPause()` 后，`onStop()` 前调用。
    
    ```java
    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        outState.putString("key", "value");
    }
    ```
    
9. **`onRestoreInstanceState(Bundle savedInstanceState)`**
    
    - **作用**：当 `Activity` 被恢复时调用，用于恢复之前保存的数据。该方法在 `onStart()` 之前调用。
    - **调用顺序**：`onStart()` 前，`onResume()` 后。
    
    ```java
    @Override
    protected void onRestoreInstanceState(Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);
        String value = savedInstanceState.getString("key");
    }
    ```
    

### 结果返回方法

10. **`onActivityResult(int requestCode, int resultCode, Intent data)`**
    
    - **作用**：当一个 `Activity` 启动另一个 `Activity` 并返回结果时调用。通常用于获取返回的数据。
    - **调用顺序**：当另一个 `Activity` 返回结果时调用。
    
    ```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) {
            // 处理返回的数据
        }
    }
    ```
    

### 菜单方法

11. **`onCreateOptionsMenu(Menu menu)`**
    
    - **作用**：创建 `Activity` 的选项菜单。调用时会根据 `Activity` 的布局生成菜单项。
    - **调用顺序**：在 `Activity` 被创建时调用。
    
    ```java
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }
    ```
    
12. **`onOptionsItemSelected(MenuItem item)`**
    
    - **作用**：处理选项菜单项的点击事件。
    - **调用顺序**：当用户选择菜单项时调用。
    
    ```java
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.menu_item:
                // 处理菜单项点击事件
                return true;
            default:
                return super.onOptionsItemSelected(item);
        }
    }
    ```
    

### 界面和输入方法

13. **`onBackPressed()`**
    
    - **作用**：当用户按下设备的“返回”键时调用。可以重写此方法来实现自定义的返回逻辑。
    - **调用顺序**：按下“返回”键时调用。
    
    ```java
    @Override
    public void onBackPressed() {
        // 自定义返回键操作
        super.onBackPressed();
    }
    ```
    
14. **`onKeyDown(int keyCode, KeyEvent event)`**
    
    - **作用**：当按键被按下时调用。可以用来处理物理键（如音量键）的按键事件。
    - **调用顺序**：按键按下时调用。
    
    ```java
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (keyCode == KeyEvent.KEYCODE_BACK) {
            // 处理返回键事件
            return true;
        }
        return super.onKeyDown(keyCode, event);
    }
    ```
    
15. **`onKeyUp(int keyCode, KeyEvent event)`**
    
    - **作用**：当按键松开时调用。
    - **调用顺序**：按键松开时调用。
    
    ```java
    @Override
    public boolean onKeyUp(int keyCode, KeyEvent event) {
        // 处理按键抬起事件
        return super.onKeyUp(keyCode, event);
    }
    ```
    

### 其他常用方法

16. **`onNewIntent(Intent intent)`**
    
    - **作用**：当 `Activity` 已经存在并且它的启动模式是 `singleTop` 或者 `singleTask` 时，如果该 `Activity` 被重新启动，则会调用此方法。
    - **调用顺序**：当 `Activity` 启动时。
    
    ```java
    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        // 处理新传入的 Intent
    }
    ```
    
17. **`onPrepareOptionsMenu(Menu menu)`**
    
    - **作用**：在选项菜单显示之前调用，可以用来更新菜单项的状态或显示新的菜单项。
    - **调用顺序**：在选项菜单即将显示时调用。
    
    ```java
    @Override
    public boolean onPrepareOptionsMenu(Menu menu) {
        // 更新菜单项
        return super.onPrepareOptionsMenu(menu);
    }
    ```
    

### 权限请求方法

18. **`onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)`**
    
    - **作用**：当请求权限的结果返回时调用。处理用户是否授予权限的结果。
    - **调用顺序**：权限请求结果返回时调用。
    
    ```java
    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        // 处理权限请求的结果
    }
    ```
    

### 其他辅助方法

19. **`finish()`**
    
    - **作用**：结束当前 `Activity`，并返回到前一个 `Activity`。
    - **调用顺序**：结束当前 `Activity` 时调用。
    ```java
    @Override
    public void finish() {
        super.finish();
        // 自定义退出逻辑
    }
```

20. **`startActivity(Intent intent)`**
    
    - **作用**：启动一个新的 `Activity`，并可以通过 `Intent` 传递数据。
    - **调用顺序**：启动新 `Activity` 时调用。
    
    ```java
    Intent intent = new Intent(this, NewActivity.class);
    startActivity(intent);
    ```
    
21. **`startActivityForResult(Intent intent, int requestCode)`**
    
    - **作用**：启动一个新的 `Activity` 并等待结果返回，可以传递数据。
    - **调用顺序**：启动新 `Activity`，等待结果时调用。
    
    ```java
    Intent intent = new Intent(this, NewActivity.class);
    startActivityForResult(intent, REQUEST_CODE);
    ```
    
22. **`getIntent()`**
    
    - **作用**：获取启动当前 `Activity` 的 `Intent` 对象。可以通过它获取传递给当前 `Activity` 的数据。
    - **调用顺序**：`Activity` 创建时调用。
    
    ```java
    Intent intent = getIntent();
    String data = intent.getStringExtra("key");
    ```
    
23. **`setResult(int resultCode, Intent data)`**
    
    - **作用**：设置当前 `Activity` 的返回结果，用于 `startActivityForResult` 返回数据。
    - **调用顺序**：设置结果并返回时调用。
    
    ```java
    Intent data = new Intent();
    data.putExtra("result", "some data");
    setResult(RESULT_OK, data);
    finish();
    ```
    

### 触摸事件处理方法

24. **`onTouchEvent(MotionEvent event)`**
    
    - **作用**：处理触摸事件，用于处理屏幕的点击、滑动等操作。
    - **调用顺序**：触摸屏幕时调用。
    
    ```java
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                // 处理触摸按下事件
                break;
            case MotionEvent.ACTION_UP:
                // 处理触摸抬起事件
                break;
        }
        return super.onTouchEvent(event);
    }
    ```
    
25. **`onGenericMotionEvent(MotionEvent event)`**
    
    - **作用**：处理其他运动事件，常用于处理来自鼠标、触摸板等输入设备的事件。
    - **调用顺序**：接收到运动事件时调用。
    
    ```java
    @Override
    public boolean onGenericMotionEvent(MotionEvent event) {
        // 处理非触摸屏事件
        return super.onGenericMotionEvent(event);
    }
    ```
    

### 窗口焦点方法

26. **`onWindowFocusChanged(boolean hasFocus)`**
    
    - **作用**：当 `Activity` 的窗口获得或失去焦点时调用。可以在这里执行一些特定的操作，如启动动画等。
    - **调用顺序**：窗口焦点状态发生变化时调用。
    
    ```java
    @Override
    public void onWindowFocusChanged(boolean hasFocus) {
        super.onWindowFocusChanged(hasFocus);
        if (hasFocus) {
            // 执行某些需要焦点的操作
        }
    }
    ```
    

### 网络状态变化监听方法

27. **`onConnectivityChanged(NetworkInfo networkInfo)`**
    
    - **作用**：监听网络状态变化，可以在此方法内执行网络连接的判断和处理。
    - **调用顺序**：当网络状态变化时调用。
    
    ```java
    @Override
    public void onConnectivityChanged(NetworkInfo networkInfo) {
        if (networkInfo.isConnected()) {
            // 网络连接
        } else {
            // 网络断开
        }
    }
    ```
    

### 权限请求方法

28. **`requestPermissions(String[] permissions, int requestCode)`**
    
    - **作用**：请求权限。可以在运行时动态请求权限。
    - **调用顺序**：请求权限时调用。
    
    ```java
    requestPermissions(new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, PERMISSION_REQUEST_CODE);
    ```
    

### 多任务处理方法

29. **`moveTaskToBack(boolean nonRoot)`**
    
    - **作用**：将当前任务移动到后台。通常在 `Activity` 的 `onBackPressed` 方法中使用，按下返回键时将应用转到后台。
    - **调用顺序**：调用时当前任务移到后台。
    
    ```java
    moveTaskToBack(true);  // 将任务移至后台
    ```
    
30. **`setRequestedOrientation(int requestedOrientation)`**
    
    - **作用**：设置 `Activity` 的屏幕方向，如横屏或竖屏。
    - **调用顺序**：调用时设置屏幕方向。
    
    ```java
    setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);  // 横屏
    ```
    

### 状态栏控制方法

31. **`setStatusBarColor(int color)`**
    
    - **作用**：设置状态栏的颜色。需要 API 21 及以上版本。
    - **调用顺序**：设置状态栏的颜色时调用。
    
    ```java
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
        getWindow().setStatusBarColor(Color.RED);
    }
    ```
    

---

以上是 `Activity` 类的主要方法及其作用，涵盖了 `Activity` 的生命周期管理、事件处理、菜单操作、权限请求等常用操作。了解这些方法的作用，可以帮助开发者更好地管理 `Activity` 的生命周期并处理不同的交互场景。