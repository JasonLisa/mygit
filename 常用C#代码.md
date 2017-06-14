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