# UCloud SDK

## 安装
```
python setup.py install
```

## 初始化连接


```
from ucloud_sdk.UCloud import UCloud

ucloud = UCloud('public_key', 'private_key')
```


## 切换zone

默认为GetRegion返回的IsDefault的域
```
ucloud.switch_zone('cn-bj2-03')
```


## 发送短信
```
ucloud.send_sms('1111111,111111,111111,1111', '测试短信')
# 手机号码可以用逗号分隔, 也可以传入list
```


## UHost主机操作

### 获取当前Zone下所有主机实例

```
ucloud.uhost.instances
```


### Uhost 监控概览
```
ucloud.uhost.mon_overview()
```

### 通过UhostId获取实例

```
instance = ucloud.uhost.get(UhostId)
```


### UHost 信息
```
instance.private_ip #私有ip
instance.public_ips #公有ip
instance.eip #返回list, EIP实例
instance.id # UhostId
instance.tag # Uhost 业务组
instance.network_state # Uhost 网络状态
instance.cpu # Uhost CPU数量
instance.gpu # Uhost GPU数量
instance.memory # Uhost 内存大小

instance.name # Uhost 名称 默认：Uhost
instance.remark # Uhost 备注
instance.auto_renew # Uhost 是否自动续费
instance.create_time # Uhost 创建时间
instance.expire_time # Uhost 过期时间
instance.state # Uhost 状态： 初始化: Initializing; 启动中: Starting; 运行中: Running; 关机中: Stopping; 关机: Stopped 安装失败: Install Fail; 重启中: Rebooting
instance.storage_type # Uhost LocalDisk，本地磁盘; UDisk，云硬盘
```

### UHost 操作
```
instance.name = test # 修改Uhost Name字段
instance.tag = test # 修改Uhost 业务组
instance.remark = test # 修改Uhost 备注
instance.reboot() # 重启Uhost
instance.start() # 启动Uhost
instance.stop() # 停止Uhost
instance.make_snapshot() # 创建快照
instance.reload() # 重新加载Uhost信息
instance.mon(cpu=False, root_disk=False, data_disk=False, io_read=False, io_write=False, disk_ops=False,
            net_in=False, net_out=False, net_pack_in=False, net_pack_out=False, memory=False, alive_process=False,
            block_process=False, tcp_connect_num=False) # 获取监控信息，最多不能超过10个选项
```


## EIP 查询与操作


### 获取当前Zone下EIP
```
ucloud.eip.instances # 所有EIP信息
ucloud.eip.unbind_eip # 所有未绑定EIP
ucloud.eip.get(EIPId) # 通过EIPId来获取EIP实例
ucloud.eip.share_bandwidth # 获取共享带宽
ucloud.eip.get_share_bandwidth(ShareBandwidthId) # 通过共享带宽id来获取 带宽实例
```

### 创建EIP
```
ucloud.eip.create_eip(operator_name, bandwidth=2, charge_type='Month', name=None, tag=None, remark=None, share_bandwidth=None)

# operator_name EIP 属性, 一般传入BGP， 海外传International
# bandwidth 带宽值，取值2到100之间
# charge_type 付费类型默认：按月
# share_bandwidth 绑定的共享带宽ID

返回创建好的EIP实例
```

### EIP实例操作

#### EIP 实例信息
```
eip = ucloud.eip.get(EIPId) # 通过EIPId来获取EIP实例
eip.id #EIPId
eip.type # 类型
eip.name # 名称默认 EIP
eip.tag # 业务组
eip.remark # 备注
eip.charge_type #付费类型
eip.status # 状态 used: 已绑定, free: 未绑定, freeze: 已冻结
eip.create_time # 创建时间
eip.expire_time # 过期时间
eip.eip_address # EIP地址信息
eip.expire # 是否过期
eip.resource # 绑定的资源
```

#### EIP 实例操作
```
eip.name = test # 修改 EIP 名称
eip.tag = test # 修改 EIP 业务组
eip.remark = test # 修改 EIP 备注
eip.release() # EIP 释放
eip.unbind(resource) # EIP 解绑 传入Uhost实例或者ULB实例
eip.bind(resource) # EIP 绑定 传入Uhost实例或者ULB实例
```


### 共享带宽

#### 共享带宽信息
```
share = ucloud.eip.get_share_bandwidth(ShareBandwidthId)
share.id # 共享带宽ID
share.width # 带宽大小
share.charge_type # 付费类型
share.create_time #创建时间
share.expire_time # 过期时间
share.address # 带宽包含的IP信息，返回EIP实例
```

#### 共享带宽操作
```
share.add_eip() # 将EIP加入共享带宽 参数类型为list， 内容为EIPID
share.remove_eip() # 将EIP 从共享带宽中移除 参数类型为list， 内容为EIPID
share.new_eip() # 在该共享带宽中创建EIP
```



# 1. 待补充 ULB（代码已经有了，但是并未全部实际测试）
# 2. 基础SDK 为封装成流程式待补充其他SDK， Uredis UDB等