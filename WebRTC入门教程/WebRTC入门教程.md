## 谁在使用WebRTC技术

 - QQ 
 - YY 
 - VoIP电话：KC网络电话；
 - 在线教育：猿题库
 
## WebRTC相关API介绍

功能划分

 1. 获取音频和视频数据
 2. 传输音频和视频数据
 3. 传输任意二进制数据
 
 API划分:三个JS接口 

 1. MediaStream (getUserMedia)
 2. RTCPeerConnection (C++)
 3. RTCDataChannel
 

### MediaStream (getUserMedia)

 - 抽象表示一个音频或者视频流
 - 可包含多个音视频记录
 - 通过 `navigator.getUserMedia()` 获取
 
![](http://ww3.sinaimg.cn/large/006y8mN6jw1f8xfosnoasj30k50a3jse.jpg)

`getUserMedia`:
  
![](http://ww1.sinaimg.cn/large/006y8mN6jw1f8xfucza4cj30oe0cxadp.jpg)

其中的 `constraints` 介绍下：

控制MediaStream的内容：媒体类型、分辨率、帧率；

### RTCPeerConnection 

 1. 信令处理
 2. 编解码协商
 3. 点对点传输
 4. 通讯安全保护
 5. 带宽管理（手机可以调得质量差点、PC可以质量高）
 6. 。。。
  
 编码采用的不是h264，而是VP8， 
 
![](http://ww1.sinaimg.cn/large/006y8mN6jw1f8u8lor8pbj30p70h8gpu.jpg)

#### TURN 做中转的
 
比如：如果两个人私有路由器都是192.168开头 ，会被认为是同一个网络下。。就需要这个

#### 信令服务

 - 双方需要交换 'Session Description' 对象：
    - 某一方支持什么格式以及将要发什么格式
    - 某一方建立点对点通讯的网络信息
 - 可以用任何消息机制和消息协议来进行交换

信令服务原理图：

![](http://ww1.sinaimg.cn/large/006y8mN6jw1f8u8wczeehj30of0f9dhf.jpg)

#### 打洞服务器（防火墙穿越服务器）

 - 在有防火墙和地址转换时P2P需要UDP打洞：
    - NAT后不能直接向广域网那样IP直接连接
    - 采用UDP进行防火墙和NAT进行穿越
 - STUN/TURN/ICE服务

只有 UDP 能打洞，TCP无法打洞，TCP需要建立连接，会容错。应该避免容错。

Client --UDP--》 Server （获知外网 IP 地址，端口号）

不是所有 NAT 网络都能打洞成功，连接就会建立失败，只能服务器中转。

为什么要有打洞服务：

 - IPv4用完
 - 私有地址一致，用了同一网段，不能用内网地址，需要用外网地址。

理想中的

![](http://ww2.sinaimg.cn/large/006y8mN6jw1f8ul107kkqj30m40dgjry.jpg)

几个服务辨析：
 
 - STUN (Session Traversal Utilities for NAT) 只能UDP，告诉我暴露在广域网的地址IP port ，我通过映射的广域网地址进行P2P数据通信。
 - TURN( Traversal Using Relays around for NAT)UDP或TCP， 打洞失败后，提供服务器中转数据，通话双方数据都通过服务器，占服务器带宽较大 - 为了确保通话在绝大多数环境下可以正常工作。跨网只能用服务器中转（测试发现的） ，使用TURN这种情况在视频通话中占10%
 - ICE 网络连接服务

WebRTC的时序图：

![](http://ww3.sinaimg.cn/large/006y8mN6jw1f8ul2nljofj30hn0f93zl.jpg)


部署STUN和TURN

一般是一个APP提供

 1. WebRTC stunserver, turnserver
 2. rfc5766-turn-server
 3. restund
 
#### STUN (Session Traversal Utilities for NAT)

NAT路由器

只能UDP，告诉我暴露在广域网的地址IP port ，我通过映射的广域网地址进行P2P数据通信。

 网络拓扑结构：
 
![](http://ww2.sinaimg.cn/large/006y8mN6jw1f8ul1s13qwj30kx0dkq3v.jpg)

#### TURN( Traversal Using Relays around for NAT)

UDP或TCP， 打洞失败后，提供服务器中转数据，通话双方数据都通过服务器，占服务器带宽较大 - 为了确保通话在绝大多数环境下可以正常工作。跨网只能用服务器中转（测试发现的） ，使用TURN这种情况在视频通话中占10%

![](http://ww4.sinaimg.cn/large/006y8mN6jw1f8xf801vnkj30jy0d1myb.jpg)

##### ICE 网络连接服务

ICE（Interactive Connectivity Establishment）

 - 是一个用来建立P2P连接的编程框架
 - 尝试去找出建立视频通话的最佳路径

![](http://ww1.sinaimg.cn/large/006y8mN6jw1f8ul2blxg0j30el07jq37.jpg)

## WebRTC for iOS 框架

 - Apple的Safari浏览器目前不支持WebRTC标准
 - 我们需要移植WebRTC的C/C++代码实现
 - 用Objective-C封装成iOS的WebRTC开发框架

 - http://www.webrtc.org
 - http://www.webrtc.org/web-apis
 - http://www.webrtc.org/native-code
 - github.com/webrtc/apprtc
 
Get the code:
 
 http://www.webrtc.org/native-code/development
 
Browse code

  https://chromium.googlesource.com/external/webrtc/+/master

Changes Log:

  https://chromium.googlesource.com/external/webrtc
  
  华为调查，WebRTC市场与前景

   -  Enterprise communications
   -  Telecom service providers
   -  Consumer web & mobile apps
   -  M2M & loT
   -  WebRTC PaaS Providers & APIs
   
![](http://ww1.sinaimg.cn/large/006y8mN6jw1f8ul37yjuaj30jz0hjjt7.jpg)

## 成熟的产品

市场上也有比较成熟的产品，比如：
https://talky.io