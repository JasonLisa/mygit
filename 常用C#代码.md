## C# MVC 用户登录状态判断
> 在Filters文件夹下添加一个类AuthenticationAttribute ，代码如下：
```
// 登录认证特性
public class AuthenticationAttribute : ActionFilterAttribute
{
    public override void OnActionExecuting(ActionExecutingContext filterContext)
    {
        if (filterContext.HttpContext.Session["username"] == null)
            filterContext.Result = new RedirectToRouteResult("Login", new RouteValueDictionary { { "from", Request.Url.ToString() } });
            
        base.OnActionExecuting(filterContext);
    }
}
```
> 在Controller中的使用方法如下：
```
public class HomeController : Controller 
{ 
    [Authentication] 
    public ActionResult Index()
    {
        return View();
    }
}
```
> 如果你想针对整个MVC项目的所有Action都使用此过滤器，步骤如下（a,b选择一种就好）：

> a. 确保Global.asax.cs的Application_Start方法中包含如下红色行：
```
public class MvcApplication : System.Web.HttpApplication
{
    protected void Application_Start()
    {
        AreaRegistration.RegisterAllAreas();

        WebApiConfig.Register(GlobalConfiguration.Configuration);
        FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
        RouteConfig.RegisterRoutes(RouteTable.Routes);
    }
}
```

> b. 在FilterConfig.cs文件中注册相应的特性过滤器：

```
public class FilterConfig
{
    public static void RegisterGlobalFilters(GlobalFilterCollection filters)
    {
        filters.Add(new HandleErrorAttribute());
        filters.Add(new AuthenticationAttribute());
    }
}
```

## entity framework 相关
>1. 使用MVC5+Entity Framework6的Code First模式创建数据库并实现增删改查功能
>
> http://www.cnblogs.com/andfly/p/6719444.html

> https://docs.microsoft.com/en-us/aspnet/mvc/overview/getting-started/database-first-development/generating-views


## 最近使用的一种登录判断
> 根目录添加一文件夹Filters

 1.添加3个过滤器类，其实就是普通类
> 第一个类
```  public class AuthenticationAttribute : ActionFilterAttribute {

        public override void OnActionExecuted(ActionExecutedContext filterContext) {
            //排除此过滤器
            object[] actionFilter = filterContext.ActionDescriptor.GetCustomAttributes(typeof(NoAuthenticationAttribute), false);
            object[] controllerFilter = filterContext.ActionDescriptor.ControllerDescriptor.GetCustomAttributes(typeof(NoAuthenticationAttribute), false);
            if(controllerFilter.Length == 1 || actionFilter.Length == 1) {
                return;
            }



            if(filterContext.HttpContext.Session["USERID"] == null)
                filterContext.Result = new RedirectResult("~/Account/Index?BackUrl=" + filterContext.HttpContext.Request.Url.LocalPath);

            base.OnActionExecuted(filterContext);

        }
    }
```
> 第二个类
 ```   // 登录不认证特性
    [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false, Inherited = false)]
    public class NoAuthenticationAttribute : ActionFilterAttribute {
        // public override void OnActionExecuting(ActionExecutingContext filterContext) {
        //if(filterContext.HttpContext.Session["userName"] == null)
        //    filterContext.Result = new RedirectToRouteResult("/Account/Login", new RouteValueDictionary { { "url", filterContext.HttpContext.Request.Url.ToString() } });

        // base.OnActionExecuting(filterContext);
        // }
        public NoAuthenticationAttribute() {

        }
```
> 第三个类
 ```public class CheckLoginAttribute : ActionFilterAttribute {

        public override void OnActionExecuted(ActionExecutedContext filterContext) {

            if(filterContext.HttpContext.Session["username"] == null)
                filterContext.Result = new RedirectResult("~/Account/Index?BackUrl=" + filterContext.HttpContext.Request.Url.LocalPath);

            base.OnActionExecuted(filterContext);

        }
    }
```

    2. App_Start文件夹下天际过滤器配置文件FilterConfig.cs
```      public class FilterConfig
    {
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
            filters.Add(new Filters.AuthenticationAttribute());//添加登录认证特性
        }
    }
```
    3. Globa.cs注册过滤器，前两个方法是在WebApi项目中用到，目的是为了让WebApi项目支持Session
    最后一个访求中第二行是注册登录认证过滤器的
```    
        public override void Init() {
            PostAuthenticateRequest += MvcApplication_PostAuthenticateRequest;
            base.Init();
        }

        void MvcApplication_PostAuthenticateRequest(object sender, EventArgs e) {
            HttpContext.Current.SetSessionStateBehavior(System.Web.SessionState.SessionStateBehavior.Required);
        }


        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            WebApiConfig.Register(GlobalConfiguration.Configuration);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }
```
        3.在登录action页面对应的action中添加登录不认证过滤器，如下
       ```  [WebMVC.Filters.NoAuthenticationAttribute]
        public ActionResult Login(string BackUrl) {
            ViewBag.BackUrl = BackUrl;
            #region 注销Session

            System.Web.HttpContext.Current.Session.Remove("USERID");       //用户主键ID
            System.Web.HttpContext.Current.Session.Remove("NAME");
            System.Web.HttpContext.Current.Session.Remove("USERNAME");
            System.Web.HttpContext.Current.Session.Remove("ISADMIN");
            System.Web.HttpContext.Current.Session.Remove("ORG_NO");

            #endregion
            return View();
        }

        //登录页面提交action
        [WebMVC.Filters.NoAuthenticationAttribute]
        public ActionResult Loginin(string BackUrl, string LOGIN_NAME, string PASSWORD) {
            ExpendAnalyModel.T_SYS_USER R_UserInfo = new ExpendAnalyDAL.T_SYS_USER().Exists(LOGIN_NAME, PASSWORD);
            if(R_UserInfo != null) {
                
                //登录成功，进入主页。并制定session
                System.Web.HttpContext.Current.Session["USERID"] = R_UserInfo.USERID; //用户主键ID
                System.Web.HttpContext.Current.Session["NAME"] = R_UserInfo.NAME;
                System.Web.HttpContext.Current.Session["USERNAME"] = R_UserInfo.USERNAME;
                System.Web.HttpContext.Current.Session["ISADMIN"] = R_UserInfo.ISADMIN;
                System.Web.HttpContext.Current.Session["ORG_NO"] = R_UserInfo.ORG_NO;

                if(!string.IsNullOrEmpty(BackUrl)) {
                    return Json(new { flag = true, msg = BackUrl }, JsonRequestBehavior.AllowGet);
                }
                else {
                    if(R_UserInfo.ISADMIN == "1") {
                        return Json(new { flag = true, msg = Url.Action("Admin", "Account") }, JsonRequestBehavior.AllowGet);
                    }
                    else {
                        return Json(new { flag = true, msg = Url.Action("Index", "Index") }, JsonRequestBehavior.AllowGet);
                    }
                }
            }
            else {
                return Json(new { flag = false, msg = "用户名或密码错误！" }, JsonRequestBehavior.AllowGet);
            }

        }
        ```


        ## 常用后台代码
        ```

        public string addUser(ExpendAnalyModel.T_SYS_USER user) {
            object o = null;
            if(new ExpendAnalyDAL.T_SYS_USER().Exists(user.USERNAME)) {
                o = new {
                    code = 1, msg = "用户已经存在!"
                };
            }
            else {
                string sql = string.Empty;
                int b = new ExpendAnalyDAL.T_SYS_USER().Add(user);
                // sql = "delete FROM t_sys_user where userid='" + id + "'";
                // bool b = DbCommand.DB_Analy.ExecuteSql(sql);
                if(b == 1) {
                    o = new {
                        code = 0, msg = "添加成功!"
                    };
                }
                else {
                    o = new {
                        code = 1, msg = "添加错误!"
                    };
                }

            }


            JsonSerializerSettings jsSettings = new JsonSerializerSettings();
            jsSettings.ReferenceLoopHandling = ReferenceLoopHandling.Ignore;
            jsSettings.DateFormatString = "yyyy-MM-dd HH:mm:ss";
            return JsonConvert.SerializeObject(o, jsSettings);
        }

        ```
    
## 后端反回JSON有2种方式格式
1.简单的dataTable等集合的反回
```
public string GetUsers(){
    DataTable dtOrgInfo = DbCommand.DB_Analy.SearchSql(sql);
    return Json(dtUerInfo, JsonRequestBehavior.AllowGet);
}
```

2.自己组成各种方式的json反回
```
public string GetUsers(){
    DataTable dtOrgInfo = DbCommand.DB_Analy.SearchSql(sql);
            
    object o = new {
        code = 0, msg = "",
        count = dtOrgInfo.Rows.Count, data = lists
    };

    JsonSerializerSettings jsSettings = new JsonSerializerSettings();
    jsSettings.ReferenceLoopHandling = ReferenceLoopHandling.Ignore;
    jsSettings.DateFormatString = "yyyy-MM-dd HH:mm:ss";
    return JsonConvert.SerializeObject(o, jsSettings);
}
```

## 提交表单的方式
```
 //监听提交
            form.on('submit(fsNew)', function (data) {
              
                $.getJSON('AddUser/', $("form").serializeObject(), function (data) {
                    layer.msg(data.msg, { icon: 2, time: 2500 }, function () {
                        if (data.code == 0) {
                            var index = parent.layer.getFrameIndex(window.name); //获取窗口索引
                            parent.layer.close(index);
                        }
                    });

                });
            }).on('submit(fsEdit)', function (data) {

                $.getJSON('UpdateUser/', $("form").serializeObject(), function (data) {
                    layer.msg(data.msg, { icon: 2, time: 2500 }, function () {
                        if (data.code == 0) {
                            var index = parent.layer.getFrameIndex(window.name);
                            parent.layer.close(index);
                        }
                    });

                });
            });
```
> 上面用的是 $("form").serializeObject() ，而不是$("form").serializeArray()
> 如果后端是按照一个Modual对象接收的，，则必须提交的是一个对象，serializeArray()序列化后不是对象，而是一个数组，每个元素是一个对象。
> serializeObject()定义如下：
```

	$.fn.serializeObject = function () {
		var obj = new Object();
		$.each(this.serializeArray(), function (index, param) {
			if (!(param.name in obj)) {
				obj[param.name] = param.value;
			}
		});
		return obj;
	};

```