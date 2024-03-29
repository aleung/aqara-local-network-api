# 设备发现与查询

## 1. 网关设备发现（设备发现不加密）

设备发现用来在局域网中发现网关，使用组播 \(ip: 224.0.0.50, peer\_port: 4321\) ，所有网关收到Whois命令都要应答、回复自己的IP信息：

* PC   **组播**方式 -&gt;网关: `{"cmd":"whois"}`   
* 网关 **单播**方式-&gt;PC：`{"cmd":"iam","ip" : "192.168.0.42","port" : "9898","model" : "gateway",.....}`

## 2. 加密机制

局域网通信采用key加密方式，需要在米家智能家庭APP上对网关设置KEY（使用AES-CBC 128加密，APP下发随机的16个字节长度的字符串KEY）。必须拥有该网关的KEY，才能与该网关进行局域网通信。

> 注： AES-CBC 128 初始向量定义为：  
>      unsigned char const AES\_KEY\_IV\[16\] = {0x17, 0x99, 0x6d, 0x09, 0x3d, 0x28, 0xdd, 0xb3, 0xba, 0x69, 0x5a, 0x2e, 0x6f, 0x58, 0x56, 0x2e};

在米家智能家庭APP中设置KEY的步骤如下：

![](/assets/111.png)                     ![](/assets/112.png)
![](/assets/113.png)                       ![](/assets/114.png)



## 3. 查询子设备id列表

命令以单播方式发送给网关的udp 9898端口，网关以单播方式回复，用来获取网关中有哪些设备（网关返回子设备的设备id）。

* PC-&gt;网关：  `{"cmd" : "get_id_list"}`
* 网关-&gt;PC:   `{"cmd" : "get_id_list_ack","sid":"1022780","data":"[\"sid1\",\"sid2\",\"sid3\"]"}`

> 其中的“sid”为网关did

## 4. 子设备状态上报

以组播方式发送给 \(ip: 224.0.0.50, port: 9898\) 。当子设备状态发生变化时，子设备会上报状态。例如窗磁上报open/close信息。用户可以拿这个状态去做联动。例如：开窗报警，开窗关空调。

**网关->PC**：`{"cmd":"report","model":"magnet","sid":"89234324","short_id":4343,"data":"{\"status\":\"open\"}"}`
