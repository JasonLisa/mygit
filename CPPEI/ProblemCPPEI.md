## 外购网站一览信息表
> **UPDATE** CPPEI\cppei.CodeIntegratedManager.Web\Controllers\GeneralAffairs\GeneralAffairsController.cs

## Asp.Net MVC中Action跳转小结
首先我觉得action的跳转大致可以这样归一下类，跳转到同一控制器内的action和不同控制器内的action、带有参数的action跳转和不带参数的action跳转。

1. RedirectToAction("Index");//一个参数时在本Controller下，不传入参数。

2. RedirectToAction(ActionName,ControllerName) //可以直接跳到别的Controller.

3. RedirectToRoute(new {controller="Home",action="Index"});//可跳到其他controller

4. RedirectToRoute(new {controller="Home",action="Index"， id=param});//可跳到其他controller,带参数。

5. Response.Redirect("Index?id=1");//适用于本controller下的方法名称,可带参数。
6. return Redirect("Index");//适用于本controller下的方法名称。

7. return View("Index"); //直接显示对应的页面 不经过执行Controller的方法。 
8. return View("~/Views/Home/Index.aspx");//这种方法是写全路径,直接显示页面,不经过Controller方法
九、return View();//直接显示页面,不经过Controller方法

> ### http://www.cnblogs.com/CreateMyself/p/5384138.html
> ### http://www.cnblogs.com/caoyc/p/5661633.html
> ### http://www.cnblogs.com/ljl0513/p/4691166.html
> ### https://msdn.microsoft.com/zh-cn/library/system.web.mvc.actionresult.aspx
> ### http://www.cnblogs.com/lidabo/archive/2012/03/28/2421898.html
> ### http://www.cnblogs.com/dreamdota/p/5768475.html、 http://www.w3school.com.cn/aspnet/mvc_reference.asp
> ### http://www.cnblogs.com/xielong/p/5940535.html
> ### http://www.cnblogs.com/Cwj-XFH/p/5877204.html


### 一此参数
1. ActionName
2. NonAction、ChildActionOnly
3. HttpPost、HttpGet



## 分部视图知识
> http://www.tuicool.com/articles/ZnAzyqI
> http://www.cnblogs.com/kissdodog/archive/2013/01/07/2848881.html
> http://www.cnblogs.com/SharpL/p/4641886.html


```
//获取参数
    function request(paras) {
        var url = location.href;
        var paraString = url.substring(url.indexOf("?") + 1, url.length).split("&");
        var paraObj = {}
        for (i = 0; j = paraString[i]; i++) {
            paraObj[j.substring(0, j.indexOf("=")).toLowerCase()] = j.substring(j.indexOf("=") + 1, j.length);
        }
        var returnValue = paraObj[paras.toLowerCase()];
        if (typeof (returnValue) == "undefined") {
            return "";
        } else {
            return returnValue;
        }
    }

     //字符串类扩充
    String.format = function () {
        if (arguments.length == 0)
            return null;
        var str = arguments[0];
        for (var i = 1; i < arguments.length; i++) {
            var re = new RegExp('\\{' + (i - 1) + '\\}', 'gm');
            str = str.replace(re, arguments[i]);
        }
        return str;
    };

```
```
 var PaperID = request('paperid');
 
 // {0}题号 {1}题型 {2}题干 {3}(子题干+)选项 {4}参考答案，你的答案 {5}是否标记 {6}是否正确
                    var temp = String.format($('#tpl-question').html(),
                        j,
                        item.TypeName,
                        item.Title,
                        tempOption[0],
                        tempOption[1],
                        item.Marked ? '<i class="fa fa-heart" aria-hidden="true">  </i> 标记' : '<i class="fa fa-heart-o"> </i>  标记 ',
                        tempOption[3] == 0x00 ? ' box-warning' : tempOption[3] == 0x03 ? ' box-success' : 'box-default'
                    );
                    $('.questionViewLeft').append(temp);
```