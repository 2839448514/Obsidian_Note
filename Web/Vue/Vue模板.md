## Vue2最基本的模板

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script type=text/javascript src=vue.js></script>
</head>
<body>
    <div id="app">
        <div v-html="message"></div>
        <button type="button" title="按钮" v-on:click="click1()">点击我哦</button>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                message: "Hello world",
            },
            methods: {
                click1: function () {
                    alert('Hello world')
                }
            }
        })
    </script>
</body>
</html>
```
