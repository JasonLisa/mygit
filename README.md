# 人资系统
- 合同管理中，员工合同信息，表格行为红色原因。 
- 文档需要细化。
- word文档增加目录。目录保留。
- word中所有流程图修改为规范样式。

# GPS系统
## 联系人按地区分。
> (表格项目名称后增加两列，联系人、联系电话)
```
htwhsj_default.aspx
```
---
## 编码管理加一类(返厂维修缺陷)
> 大类直接在数据库加，小类在程序加
## 统计分析加一模块，对应缺陷，类似于缺陷分类统计
## 安装维修管理中，返厂维修维修申请 
> 流程中第二部按新添加的缺陷编码表添加下拉列表，当选择报废或其他处理时，备注为必填选项。

 ## 处理方式如下
 - 1. 编码表添加一记录
 ``` 
  insert into gps_t_p_type values ('fcgzch','返厂故障处理','1',sysdate)
 ```
 - 2. 编码子表添加30条记录

```
INSERT INTO gps_t_r_code (scode_id,stype_id,scodename,orderid,isshow)
SELECT '001','fcgzch','补CPU引脚',1,'1' from dual union
SELECT '002','fcgzch','补SIM卡座',2,'1' from dual union
SELECT '003','fcgzch','补GPS模块',3,'1' from dual union
SELECT '004','fcgzch','补串口芯片',4,'1' from dual union
SELECT '005','fcgzch','固定IPEX座',5,'1' from dual union
SELECT '006','fcgzch','补GPRS模块',6,'1' from dual union
SELECT '007','fcgzch','更换充电芯片',7,'1' from dual union
SELECT '008','fcgzch','更换SMA母座(GPS)',8,'1' from dual union
SELECT '009','fcgzch','更换SMA母座(GPRS)',9,'1' from dual union
SELECT '010','fcgzch','更换LM2596',10,'1' from dual union
SELECT '011','fcgzch','更换CPU',11,'1' from dual union
SELECT '012','fcgzch','更换SIM卡座',12,'1' from dual union
SELECT '013','fcgzch','更换按键',13,'1' from dual union
SELECT '014','fcgzch','更换拨动开关',14,'1' from dual union
SELECT '015','fcgzch','更换GPS模块',15,'1' from dual union
SELECT '016','fcgzch','更换GPRS模块',16,'1' from dual union
SELECT '017','fcgzch','更换IPEX跳线',17,'1' from dual union
SELECT '018','fcgzch','更换LM393',18,'1' from dual union
SELECT '019','fcgzch','更换SPP4953',19,'1' from dual union
SELECT '020','fcgzch','更换电源插头',20,'1' from dual union
SELECT '021','fcgzch','更换MC34119',21,'1' from dual union
SELECT '022','fcgzch','更换双色灯',22,'1' from dual union
SELECT '023','fcgzch','启动异常,重新下载程序',23,'1' from dual union
SELECT '024','fcgzch','报废',24,'1' from dual union
SELECT '025','fcgzch','其它处理',25,'1' from dual union
SELECT '026','fcgzch','未修改参数',26,'1' from dual union
SELECT '027','fcgzch','更换VET6顶板',27,'1' from dual union
SELECT '028','fcgzch','更换锂电池',28,'1' from dual union
SELECT '029','fcgzch','测试正常,更新版本或初始化参数',29,'1' from dual union
SELECT '030','fcgzch','备用设备反库',30,'1' from dual
```
 - 3. 在权限管理系统中，在 **统计分析模块** 添加加一模块，配置如下，并赋予相应权限。
    - 1. 模块名称：返厂维修处理方式统计
    - 2. 路径：tjfx/fcwxclfstj.aspx

 - 4. 添加tjfx/fcwxclfstj.aspx文件


 - 5. 返厂故障申请
    - 修改Gpsgl\azwxgl\fcwxsq.aspx
    - 添加Gpsgl\public\public_SelectFCGZXXCLFf.aspx

---

## 库存查询  数量对应不上。

---
## 返厂维修申请流程更改、返厂维修申请与出库申请中的完结挂勾。
> 具体如下：在返厂维修中，设备列表增加一列，对应出库单(只是维修的),这样出库单就可以完结了。

---
## 出库样式更改。

----
 ## 合同维护时间管理，已过期过滤，去掉暂停维护。
 ---
 ## 联系人按地区分。(调整联系人顺序)

 ---
 ## 流程监控(流程查询)，只监控开始但是没有完结的流程。

 ---
 ## 所有统计分析中，查询条件中，有时间范围的，都加上默认时间范围。默认时间范围为当月1号至本天。

 --
 ## 工作量统计中，加一列，表示工作效率。