## 项目管理软件地址
> http://10.122.100.165:8091/redmine

> 用户名：ykw 初始密码：12345678

## 统一信息平台
> http://10.122.100.165:8081/

> 用户名：admin 初始密码：1

## 测试接口
> http://localhost:10244/BussinessService.asmx

## 流程配置地址
> http://10.122.100.165:8082/WFDefine/Login.aspx

## 权限地址
> http://10.122.100.165:8081/ admin/1
> http://10.122.100.168:8081/ admin/123 *除了数据库用165 ，其他168为最新的*

## Demo
> http://10.122.100.168:8086/ 

> 用户名：luo_ling 初始密码：123456

## 流程相关
> 业务表
```
select t.* from Com_BuyWebsite t where t.WebSID='2E4713D0-BCC0-4D4F-8DD1-1A6F34D371F6'
```
>系统中有哪些流程
```
SELECT * FROM [MIS_WorkFlow].[dbo].[WFD_FlowInfo] where WorkFlowName like '%外%'
```

>流程主表
```
SELECT * FROM [MIS_WorkFlow].[dbo].[WF_WorkFlowInstance] where WFMouldID='BW220201' and BusinessID='2E4713D0-BCC0-4D4F-8DD1-1A6F34D371F6' order by CreateDate desc
```

>流程总共有多少个Node
```
SELECT * FROM [MIS_WorkFlow].[dbo].[WF_NodeInstance] where WFID='716'
```
>各节点下面有哪些人可以审批
```
SELECT * FROM [MIS_WorkFlow].[dbo].[WF_SubNodeInstance] where wfid='716' 
```

>流程已经审批过的Node
>
```
SELECT * FROM [MIS_WorkFlow].[dbo].[WF_Note] where BusinessID='2E4713D0-BCC0-4D4F-8DD1-1A6F34D371F6'
```

