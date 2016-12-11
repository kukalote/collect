

字段名 | 字段类型 | 字段解释
----|---|---
id | int(11) | 主键
payment_order_id | int(11) | 结佣单id
payment_order_info_id | int(11) | 结佣单详情id
lx_id | int(11) | 立项id
uid | int(11) | 审核人员id
contact | varchar(14) | 收款方联系方式
rcv_id | int(11) | 付款服务方id 
amount | float(9.2) | 打款金额
code | int(30) | 支付指令码
send_id | int(11) | 汇款方id 
req_uid | int(11) | 申请人id
rcv_name | varchar(120) | 经济公司名称
attach_addr | varchar(240) | 附件地址
note | varchar(255) | 推送返回备注
type | int(1) | 类型:1.扣减，2.返还
create_time | int(10) | 创建时间
update_time | int(10) | 更新时间