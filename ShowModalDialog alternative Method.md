https://hackmd.io/@Pi4yvNIzRAe1z54_pwjpQA/ryVL4yqwP
```C# MVC```
# window.showModalDialog 替代方法

## 為什麼要改寫window.showModalDialog()?

## 方法：window.open()
### 範例1 使用父視窗元素 
- 父視窗
``` html=
<!DOCTYPE html>

<html>
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>父視窗 </title>
</head>
<body>
    <form id="form1" runat="server"> 
        <input type="button" value="另開視窗" id="btnParent" />
         <!--父視窗的DOM元素-->
        <div id="divParent"></div>

        <!--引用jQuery-->
        <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <script type="text/javascript">
            $(function () {
                $("#btnParent").on("click", function () {
                    //另開popup window
                    window.open('@Action.Url("B","Home")', "_blank", "width=500px,height=500px");
                   
                });
            });
        </script>
    </form>
</body>
</html>
```
- 子視窗

```html=
<!DOCTYPE html>

<html>
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>子視窗</title>
</head>
<body>
    <form id="form1" runat="server">

        <input type="button" value="關閉子視窗" id="btnSub" />
        <!--引用jQuery-->
        <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <script type="text/javascript">
            $(function () { 
                $("#btnSub").on("click", function () {  
                    //操作父視窗的DOM元素 
                    window.opener.document.getElementById("divParent").innerHTML = "兒子給的值";
                    window.close();//關閉子視窗
                });
            });
        </script>
    </form>
</body>
</html>
```
### 範例2 子視窗傳值給父視窗
- 父視窗
```html=
<!DOCTYPE html>

<html>
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>父視窗 </title>
</head>
<body>
    <form id="form1" runat="server"> 
        <input type="button" value="另開視窗" id="btnParent" />
         
        <!--引用jQuery-->
        <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <script type="text/javascript">
            $(function () {
                $("#btnParent").on("click", function () {
                    //另開popup window
                    window.open('@Action.Url("B","Home")', "_blank", "width=500px,height=500px");
                   
                });
            });

            //注意父視窗要宣告成global function 接子視窗值
            function windowOpenReturnFunc(ret) {
                //ret為子視窗回傳的值
                alert("子視窗回傳的值:"+ret);
            }
        </script>
    </form>
</body>
</html>

```
- 子視窗
```html= 
<!DOCTYPE html>

<html>
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>子視窗</title>
</head>
<body>
    <form id="form1" runat="server">

        <input type="button" value="關閉子視窗" id="btnSub" />
        <!--引用jQuery-->
        <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <script type="text/javascript">
            $(function () { 
                $("#btnSub").on("click", function () {   
                    let subParam = "兒子的值";
                    window.opener.windowOpenReturnFunc(subParam);//執行父視窗的function
                    window.close();//關閉子視窗
                });
            });
        </script>
    </form>
</body>
</html>
```
### 範例3 父視窗值傳給子視窗
- 父視窗
```html=
<!DOCTYPE html>

<html>
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>父視窗 </title>
</head>
<body>
    <form id="form1" runat="server"> 
        <input type="button" value="另開視窗" id="btnParent" />
         
        <!--引用jQuery-->
        <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>

        <script type="text/javascript">
            //注意要用var宣告變數並且放在global區域，才會變成window的屬性
            var parentParam = "我是老爸";
            $(function () {
                $("#btnParent").on("click", function () {
                    //另開popup window
                    window.open('@Action.Url("B","Home")', "_blank", "width=500px,height=500px");                   
                });
            });
        </script>
    </form>
</body>
</html>
```
- 子視窗
```html= 
<!DOCTYPE html>

<html>
<head runat="server">
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>子視窗</title>
</head>
<body>
    <form id="form1" runat="server">

        <input type="button" value="關閉子視窗" id="btnSub" />
        <!--引用jQuery-->
        <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <script type="text/javascript">
            $(function () {
                //取得父視窗傳遞的值
                alert(window.opener.parentParam);
                $("#btnSub").on("click", function () {  
                    window.close();//關閉子視窗
                });
            });
        </script>
    </form>
</body>
</html>
```
