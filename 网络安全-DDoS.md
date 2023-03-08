# DDoS攻击
## 什么是 DDoS 攻击？
分布式拒绝服务（DDoS）攻击是通过大规模互联网流量淹没目标服务器或其周边基础设施，以破坏目标服务器、服务或网络正常流量的恶意行为。

DDoS 攻击利用多台受损计算机系统作为攻击流量来源以达到攻击效果。利用的机器可以包括计算机，也可以包括其他联网资源（如 IoT 设备）。

总体而言，DDoS 攻击好比高速公路发生交通堵塞，妨碍常规车辆抵达预定目的地。

## DDoS 攻击的工作原理
DDoS 攻击是通过连接互联网的计算机网络进行的。

这些网络由计算机和其他设备（例如 IoT 设备）组成，它们感染了[恶意软件](https://www.cloudflare-cn.com/learning/ddos/glossary/malware/)，从而被攻击者远程控制。这些个体设备称为
[机器人](https://www.cloudflare-cn.com/learning/bots/what-is-a-bot/)（或僵尸），一组机器人则称为[僵尸网络](https://www.cloudflare-cn.com/learning/ddos/what-is-a-ddos-botnet/)。

一旦建立了僵尸网络，攻击者就可通过向每个机器人发送远程指令来发动攻击。

当僵尸网络将受害者的服务器或网络作为目标时，每个机器人会将请求发送到目标的 [IP 地址](https://www.cloudflare-cn.com/learning/dns/glossary/what-is-my-ip-address/)，这可能
导致服务器或网络不堪重负，从而造成对正常流量的[拒绝服务](https://www.cloudflare-cn.com/learning/ddos/glossary/denial-of-service/)。

由于每个机器人都是合法的互联网设备，因而可能很难区分攻击流量与正常流量。

## 如何识别 DDoS 攻击
DDoS 攻击最明显的症状是网站或服务突然变慢或不可用。但是，造成类似性能问题的原因有多种（如合法流量激增），因此通常需要进一步调查。流量分析工具可以帮助您发现 DDoS 攻击的一些明显迹象：
- 来自单个 IP 地址或 IP 范围的可疑流量
- 来自共享单个行为特征（例如设备类型、地理位置或 Web 浏览器版本）的用户的大量流量
- 对单个页面或端点的请求数量出现不明原因的激增
- 奇怪的流量模式，例如一天中非常规时间段的激增或看似不自然的模式（例如，每 10 分钟出现一次激增）

DDoS 攻击还有其他更具体的迹象，具体取决于攻击的类型。

## 常见的 DDoS 攻击有哪几类？
不同类型的 DDoS 攻击针对不同的网络连接组件。为了解不同的 DDoS 攻击如何运作，有必要知道网络连接是如何建立的。

互联网上的网络连接由许多不同的组件或“层”构成。就像打地基盖房子一样，模型中的每一步都有不同的用途。

[OSI 模型](https://www.cloudflare-cn.com/learning/ddos/glossary/open-systems-interconnection-model-osi/)（如下图所示）是一个概念框架，用于描述 7 个不同层级的网络连接。
![img](https://user-images.githubusercontent.com/42236890/223639006-acbab59a-1729-48c3-a422-5f3c79e8b469.png)

虽然几乎所有 DDoS 攻击都涉及用流量淹没目标设备或网络，但攻击可以分为三类。攻击者可能利用一种或多种不同的攻击手段，也可能根据目标采取的防范措施循环使用多种攻击手段。

### 应用程序层攻击
**攻击目标：**

此类攻击有时称为第 7 层 DDoS 攻击（指 OSI 模型第 7 层），其目标是耗尽目标资源。

攻击目标是生成网页并传输网页响应 HTTP 请求的服务器层。在客户端执行一项 HTTP 请求的计算成本比较低，但目标服务器做出响应却可能非常昂贵，因为服务器通常必须加载多个文件并运行数据库查询才能创建网页。

第 7 层攻击很难防御，因为难以区分恶意流量和合法流量。

**应用程序层攻击示例：**

![img](https://user-images.githubusercontent.com/42236890/223639831-47fc9985-c3a4-4750-b071-3a7f5be777aa.png)

**HTTP 洪水**

HTTP 洪水攻击类似于同时在大量不同计算机的 Web 浏览器中一次又一次地按下刷新 ——大量 HTTP 请求涌向服务器，导致拒绝服务。

这种类型的攻击有简单的，也有复杂的。

较简单的实现可以使用相同范围的攻击 IP 地址、referrer 和用户代理访问一个 URL。复杂版本可能使用大量攻击性 IP 地址，并使用随机 referrer 和用户代理来针对随机网址。

### 协议攻击
**攻击目标：**

协议攻击也称为状态耗尽攻击，这类攻击会过度消耗服务器资源和/或[防火墙](https://www.cloudflare-cn.com/learning/security/what-is-a-firewall/)和负载平衡器之类的网络设备资源，
从而导致服务中断。

协议攻击利用协议堆栈第 3 层和第 4 层的弱点致使目标无法访问。

**协议攻击示例：**

![img](https://user-images.githubusercontent.com/42236890/223640756-2e99dc04-4d77-41c1-b807-a834135a1de8.png)

**SYN 洪水**

[SYN 洪水](https://www.cloudflare-cn.com/learning/ddos/syn-flood-ddos-attack/)就好比补给室中的工作人员从商店的柜台接收请求。

工作人员收到请求，前去取包裹，再等待确认，然后将包裹送到柜台。工作人员收到太多包裹请求，但得不到确认，直到无法处理更多包裹，实在不堪重负，致使无人能对请求做出回应。

此类攻击利用 [TCP](https://www.cloudflare-cn.com/learning/ddos/glossary/tcp-ip/) 握手（两台计算机发起网络连接时要经过的一系列通信），通过向目标发送大量带有
[伪造](https://www.cloudflare-cn.com/learning/ddos/glossary/ip-spoofing/)源 IP 地址的 TCP“初始连接请求”SYN 数据包来实现。

目标计算机响应每个连接请求，然后等待握手中的最后一步，但这一步确永远不会发生，因此在此过程中耗尽目标的资源。

### 容量耗尽攻击
**攻击目标：**

此类攻击试图通过消耗目标与较大的互联网之间的所有可用带宽来造成拥塞。攻击运用某种放大攻击或其他生成大量流量的手段（如僵尸网络请求），向目标发送大量数据。

**放大示例：**

![img](https://user-images.githubusercontent.com/42236890/223641610-9f7a34e8-3245-43d8-8f66-38673fc76a67.png)

**DNS 放大**

[DNS 放大](https://www.cloudflare-cn.com/learning/ddos/dns-amplification-ddos-attack/)就好比有人打电话给餐馆说“每道菜都订一份，请给我回电话复述整个订单”，而提供的
回电号码实际上属于受害者。几乎不费吹灰之力，就能产生很长的响应并发送给受害者。

利用伪造的 IP 地址（受害者的 IP 地址）向开放式 [DNS 服务器](https://www.cloudflare-cn.com/learning/dns/what-is-dns/)发出请求后，目标 IP 地址将收到服务器发回的响应。

## 如何防护 DDoS 攻击？
若要缓解 DDoS 攻击，关键在于区分攻击流量与正常流量。

例如，如果因发布某款产品导致公司网站涌现大批热情客户，那么全面切断流量是错误之举。如果公司从已知恶意用户处收到的流量突然激增，或许需要努力缓解攻击。

难点在于区分真实客户流量与攻击流量。

在现代互联网中，DDoS 流量以多种形式出现。流量设计可能有所不同，从非欺骗性单源攻击到复杂的自适应多方位攻击无所不有。

多方位 DDoS 攻击采用多种攻击手段，以期通过不同的方式击垮目标，很可能分散各个层级的缓解工作注意力。

同时针对协议堆栈的多个层级（如 DNS 放大（针对第 3/4 层）外加 HTTP 洪水（针对第 7 层））发动攻击就是多方位 DDoS 攻击的一个典型例子。

为防护多方位 DDoS 攻击，需要部署多项不同策略，从而缓解不同层级的攻击。

一般而言，攻击越复杂，越难以区分攻击流量与正常流量 —— 攻击者的目标是尽可能混入正常流量，从而尽量减弱缓解成效。

如果缓解措施不加选择地丢弃或限制流量，很可能将正常流量与攻击流量一起丢弃，同时攻击还可能进行修改调整以规避缓解措施。为克服复杂的破坏手段，采用分层解决方案效果最理想。

### 黑洞路由
有一种解决方案几乎适用于所有网络管理员：创建[黑洞路由](https://www.cloudflare-cn.com/learning/ddos/glossary/ddos-blackhole-routing/)，并将流量汇入该路由。在最简单
的形式下，当在没有特定限制条件的情况下实施黑洞过滤时，合法网络流量和恶意网络流量都将路由到空路由或黑洞，并从网络中丢弃。

如果互联网设备遭受 DDoS 攻击，则该设备的互联网服务提供商（ISP）可能会将站点的所有流量发送到黑洞中作为防御。这不是理想的解决方案，因为它相当于让攻击者达成预期的目标：使网络无法访问。

### 速率限制
限制服务器在某个时间段接收的请求数量也是防护拒绝服务攻击的一种方法。

虽然速率限制对于减缓 Web 爬虫窃取内容及防护[暴力破解](https://www.cloudflare-cn.com/learning/bots/brute-force-attack/)攻击很有帮助，但仅靠速率限制可能不足以有效应对
复杂的 DDoS 攻击。

然而，在高效 DDoS 防护策略中，速率限制不失为一种有效手段。

### Web 应用程序防火墙
[Web 应用程序防火墙（WAF）](https://www.cloudflare-cn.com/learning/ddos/glossary/web-application-firewall-waf/) 是一种有效工具，有助于缓解第 7 层 DDoS 攻击。在
互联网和源站之间部署 WAF 后，WAF 可以充当[反向代理](https://www.cloudflare-cn.com/learning/cdn/glossary/reverse-proxy/)，保护目标服务器，防止其遭受特定类型的恶意流量
入侵。

通过基于一系列用于识别 DDoS 工具的规则过滤请求，可以阻止第 7 层攻击。有效的 WAF 的一个关键价值是能够快速实施自定义规则以应对攻击。

### Anycast 网络扩散
此类缓解方法使用 Anycast 网络，将攻击流量分散至分布式服务器网络，直到网络吸收流量为止。

这种方法就好比将湍急的河流引入若干独立的小水渠，将分布式攻击流量的影响分散到可以管理的程度，从而分散破坏力。

[Anycast 网络](https://www.cloudflare-cn.com/learning/cdn/glossary/anycast-network/)在缓解 DDoS 攻击方面的可靠性取决于攻击规模及网络规模和效率。

# 其他文章
[蚂蚁集团](https://antchain.antgroup.com/docs/2/28401)
