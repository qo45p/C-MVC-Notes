[![hackmd-github-sync-badge](https://hackmd.io/tIu6M2NhSWWtCq2ieHBEOA/badge)](https://hackmd.io/tIu6M2NhSWWtCq2ieHBEOA)
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

bootstrap
```html=
@using (Html.BeginForm("Index", "Home", FormMethod.Post))
{
    <!-- Modal -->
    <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
                    <h4 class="modal-title" id="myModalLabel">Modal title</h4>
                </div>
                <div class="modal-body">
                    <div class="form-group">
                        <label for="recipient-name" class="control-label">Recipient:</label>
                        <input type="text" class="form-control" id="recipient-name" name="Name">
                    </div>
                    <div class="form-group">
                        <label for="message-text" class="control-label">Message:</label>
                        <textarea class="form-control" id="message-text" name="Message"></textarea>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                    <button type="Submit" class="btn btn-primary">Save changes</button>
                </div>
            </div><!-- /.modal-content -->
        </div><!-- /.modal-dialog -->
    </div><!-- /.modal -->
}
```

```html=
[HttpPost]
public ActionResult Index(string Name, string Message)
{
    return View();
}
```

### 範例4
```html=
<form id="form1" runat="server">
        <input type="button" value="另開視窗" id="btnParent" />
        <!--父視窗的DOM元素-->
        <div id="divParent">divParent&ensp; </div>
        <div id="divParent2">divParent2&ensp; </div>
        <div id="divParent3">Try Try</div>

        <!--引用jQuery-->
        <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <script type="text/javascript">
            

            $(function () {                
                $("#btnParent").on("click", function () {
                    //另開popup window
                    var child = window.open('@Url.Action("B","Home")', "_blank", "width=500px,height=500px");
                    var timer = setInterval(checkChild, 500);

                    function checkChild() {
                        if (child.closed) {
                            alert("Child window closed");
                            clearInterval(timer);
                            document.getElementById("divParent3").innerHTML = "New text!";
                        }
                    }
                });            

            });

            //注意父視窗要宣告成global function
            function windowOpenReturnFunc(ret) {
                //ret為子視窗回傳的值
                alert("子視窗回傳的值:" + ret);
                window.opener.document.getElementById("divParent2").innerHTML = ret;
                
            }
        </script>
    </form>

```

```html=
<form id="form1" runat="server">
    <select id="pets" name="pets">
        <option value="dog">Dog</option>
        <option value="cat" selected>Cat</option>
        <option value="hamster">Hamster</option>
        <option value="parrot">Parrot</option>
        <option value="goldfish">Goldfish</option>
    </select>

    <input type="button" value="關閉子視窗" id="btnSub" />
    <!--引用jQuery-->
    <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script type="text/javascript">
        $(function () {
            $("#btnSub").on("click", function () {
                let subParam = "兒子的值";             
                window.opener.document.getElementById("divParent").innerHTML = "兒子給的值";
                window.opener.document.getElementById("divParent2").innerHTML = "選的動物: " + $('#pets').val();
                window.opener.windowOpenReturnFunc(subParam);//執行父視窗的function
            });
        });
    </script>
</form>
```

### 範例五
```
//Index.cshtml
    <form id="form1" runat="server">
        <input type="button" value="另開視窗" id="btnParent" />
        <!--父視窗的DOM元素-->
        <div id="divParent">divParent&ensp; </div>
        <div id="divParent2">divParent2&ensp; </div>
        <div id="divParent3">Try Try</div>
        <h3 id="h3">H3</h3>


        <!--引用jQuery-->
        <!--[if !IE]> -->
        <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <!-- <![endif]-->
        <!--[if IE]-->
        <script src="https://code.jquery.com/jquery-1.11.0.min.js"
                integrity="sha256-spTpc4lvj4dOkKjrGokIrHkJgNA0xMS98Pw9N7ir9oI="
                crossorigin="anonymous"></script>
        <!--[endif]-->

        <script type="text/javascript">
            $(function () {
                $("#btnParent").on("click", function () {

                    var child = window.open('@Url.Action("B","Home")?funcName=windowOpenReturnFunc', "_blank", "width=500px,height=500px");
                    //window.popup = window.open('@Url.Action("B","Home")?fncName=windowOpenReturnFunc', "_blank", "width=500px,height=500px");

                    //解決IE版本addEventListener問題 u
                    //child[child.addEventListener ? 'addEventListener' : 'attachEvent'](  (child.attachEvent ? 'on' : '') + 'load', B, false);
                    //child.addEventListener('load', B, false);

                    //IE new Function    
                    child.onload = new function () {
                        child[child.addEventListener ? 'addEventListener' : 'attachEvent'](  (child.attachEvent ? 'on' : '') + 'unload', B, true);
                    }

                    ////Chrome
                    //child.onload = function () {
                    //    //child[child.addEventListener ? 'addEventListener' : 'attachEvent'](  (child.attachEvent ? 'on' : '') + 'unload', B, true);
                    //    child.onbeforeunload = function(){
                    //        B();                         
                    //     }
                    //}
                });
            });

            function B() {
                var x = 1;
                window.alert(x + "大好");
                document.getElementById("h3").innerHTML = x + "大好";
            }

            //注意父視窗要宣告成global function
            function windowOpenReturnFunc(ret) {
                //ret為子視窗回傳的值
                //alert("子視窗回傳的值:" + ret);
                document.getElementById("divParent2").innerHTML = ret;

            }
        </script>
    </form>
        </script>
```
```
//B.cshtml
<form id="form1" runat="server">
    <select id="pets" name="pets">
        <option value="dog">Dog</option>
        <option value="cat" selected>Cat</option>
        <option value="hamster">Hamster</option>
        <option value="parrot">Parrot</option>
        <option value="goldfish">Goldfish</option>
    </select>

    <p>@ViewBag.funcName</p>

    <input type="button" value="關閉子視窗" id="btnSub" />
    <!--引用jQuery-->

    <script type="text/javascript" src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script type="text/javascript">
        $(function () {
            $("#btnSub").on("click", function () {
                let subParam = "TTTTT";             
                window.opener.document.getElementById("divParent").innerHTML = "兒子給的值";
                //window.opener.document.getElementById("divParent2").innerHTML = "選的動物: " + $('#pets').val();
                //window.opener.windowOpenReturnFunc(subParam);//執行父視窗的function
                //window.opener["@ViewBag.funcName"](subParam);
                window.opener.window.windowOpenReturnFunc(subParam);
                window.close();
            });
        });
    </script>
</form>

<a href="#" onclick='
    window.close(); return false;'>
    關閉視窗
</a>

```
