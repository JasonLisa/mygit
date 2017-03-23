## 项目管理软件地址
> http://10.122.100.165:8091/redmine

> 用户名：ykw 初始密码：12345678

## 流程配置地址
> http://10.122.100.165:8082/WFDefine/Login.aspx

## 权限地址
> http://10.122.100.165:8081/

## Demo
> http://10.122.100.168:8086/ 

> 用户名：luo_ling 初始密码：123456

## 流程相关
> 业务表
```
select t.* from Com_BuyWebsite t where t.WebSID='2E4713D0-BCC0-4D4F-8DD1-1A6F34D371F6'
```

>流程主表
```
SELECT * FROM [MIS_WorkFlow].[dbo].[WF_WorkFlowInstance] where WFMouldID='BW220201' and BusinessID='2E4713D0-BCC0-4D4F-8DD1-1A6F34D371F6' order by CreateDate desc
```

>流程总共有多少个Node
```
SELECT * FROM [MIS_WorkFlow].[dbo].[WF_NodeInstance] where WFID='716'
```

>流程已经审批过的Node
>
```
SELECT * FROM [MIS_WorkFlow].[dbo].[WF_Note] where BusinessID='2E4713D0-BCC0-4D4F-8DD1-1A6F34D371F6'
```

