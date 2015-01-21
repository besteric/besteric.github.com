---
Layout: Post
Title: "我是如何改善PS4的下载速度+联机质量的"
Description: 微软Xbox One真是酷炫啊，老板，给我来台Ps4压压惊...
Categories:
- 偶尔惊喜
Tags:
- Ps4


---


一直在Xbox One和Ps4之间摇摆不定，但是看到X1国行的锁区政策以及性价比略低（其实就是没钱），加上看了顽皮狗的「The Last Of Us」演示顿时觉得这游戏就是我想要的...理所当然的归入了索尼大法阵营。

本来以为玩正版能省心不少，实际情况是国内的网络环境实在恶劣，逼着玩家想着各种方法解决，以下主要介绍几种我自己使用的网络配置

---

**改善下载速度**


**Dns**

最简单易操作的方式就是直接在Ps4上选择自定义网络，修改Dns为如下几个：

* 韩国Dns: 168.126.63.1/168.126.63.2
* 阿里Dns: 223.5.5.5/223.6.6.6
* 谷歌Dns: 8.8.8.8/8.8.4.4

最近A9出了一个[公益性Dns节点](Http://Bbs.A9Vg.Com/Thread-4423384-1-1.Html)：106.185.46.149，使用的是Dnsmasq加速技术，也是我目前使用的


**Dnsmasq**

说明：

Ps4国行还未上市，所以国内的Psn全部都是解析至海外Cdn节点，导致数字版游戏、更新游戏的速度都是龟速，这个时候就需要手动将Ps4指引到国内的隐藏Cdn节点

配置：

Dnsmasq比较推荐配置在路由器上，所以Openwrt或者Routeros这类路由器可以直接配置，国内的极路由貌似也可以很简单的配置。我没有这些高级货，只有一台自己组件的[黑群晖](http://besteric.com/2014/05/17/make-my-custom-nas/)，所幸也是Linux系统，所以直接在它上面搭建

过程：
    
1. Ssh Root@Nasip
2. Cd /Volume1/@Tmp
3. Wget "Http://Ipkg.Nslu2-Linux.Org/Feeds/Optware/Syno-I686/Cross/Unstable/Syno-I686-Bootstrap_1.2-7_I686.Xsh”
    不同型号的Nas下载的版本不一致，[详细查阅](Http://Forum.Synology.Com/Wiki/Index.Php/Overview_On_Modifying_The_Synology_Server,_Bootstrap,_Ipkg_Etc#Bootstrap)
4. Sh Syno-I686-Bootstrap_1.2-7_I686.Xsh
5. Ipkg Update
6. Ipkg Install Dnsmasq
7. Vi /Opt/Etc/Dnsmasq.Conf
      * 关闭 Dhcp：No-Dhcp-Interface=
      * 增加 默认 Dns：Server=114.114.114.114
      * 增加以下语句
        Address=/Zeus.Dl.Playstation.Net/58.218.214.10
        Address=/Poseidon.Dl.Playstation.Net/58.218.214.10
        Address=/Gs2.Ww.Prod.Dl.Playstation.Net/58.218.214.10
        Address=/Gs.Ww.Prod.Dl.Playstation.Net/58.218.214.10
        Address=/Ares.Dl.Playstation.Net/58.218.214.10 
    
8. 上述的Ip请参考[这个帖子](Http://Bbs.A9Vg.Com/Thread-4255770-1-1.Html)提供的地理位置做判断
9. /Opt/Etc/Init.D/S56Dnsmasq Restart
10. Ps4的网络设置选择自定，Dns第一栏填写Nas在局域网的Ip

成果：

电信100Mb光纤宽带，裸连或者使用外部Dns下载速度最高为2Mb/S，改用Dnsmasq后最高速可以达到10Mb/S，下载6Gb数据大概在15-20分钟左右，效果比较明显。

参考资料：

* [重新整理一下Dnsmasq所需步骤和知识](Http://Bbs.A9Vg.Com/Thread-3476870-1-1.Html)
* [Synology Nas 党安装 Dnsmasq 加速 Psn](Http://Bbs.A9Vg.Com/Forum.Php?Mod=Viewthread&Tid=4139049)
* [Dnsmasq用 中国节点 收集](Http://Bbs.A9Vg.Com/Thread-4255770-1-1.Html)
 



---

**改善联机质量**

联机质量主要依据你连接游戏服务器的网络状况而定，从目前来看Ea的游戏大都使用自己的服务器，典型代表就是「战地4」和「植物大战僵尸:花园战争」，顽皮狗的「The Last Of Us」也是使用的自己服务器，这些服务器大多数都在境外，考虑到电信糟糕的出口带宽，基本上直连这些服务器的速度非常慢，所以就需要使用代理了。

主要的代理方式比较流行的就是VPN、SSH、Shadowsocks，易用性来看VPN>SSH>Shadowsocks，稳定性是Shadowsocks>SSH>VPN

**VPN**

如果是老款Macbook Pro，最简单的办法就是插入有线后拨VPN，然后通过系统自动的共享功能，将VPN通过Wifi发射出去，PS4直连这个Wifi即可，不用做任何改动

对于新款Retina Macbook Pro来说，推荐使用[squidman](http://squidman.net/squidman/)在本地架设http代理

下载后只需要在Preferences的Clients添加一个「all」网段即可提供http代理给局域网所有设备，默认端口是8080，比如电脑在局域网的IP为10.2.158.118，那么就在PS4的网络设定中http代理填写10.2.158.118:8080

**Shadowsocks**

实际使用下来我发现VPN不是很稳定，经常受到GFW的干扰较严重，在晚上高峰时期（20:00-24:00）经常会丢包，看视频可能影响不大，但是对于联机的影响是致命的...所以研究了下如何将Shadowsocks代理给PS4使用。由于PS4只支持http代理，所以我们需要将Shadowsocks的socks代理先在本地转成http代理，这里推荐使用[Privoxy](http://www.privoxy.org/)

1. 下载安装后，在终端输入： sudo vi /usr/local/etc/privoxy/config
2. 主要修改两行代码（主要是将#注释去掉）：
     * listen-address 0.0.0.0:8118 (0.0.0.0代表监听局域网所有网段的8118端口)
     * forward-socks5 / 127.0.0.1:8087（8087是本地shadowsocks client端口）
3. 重启下Privoxy（应用程序有Privoxy的脚本，直接脱到终端执行）

4. PS4上http代理填写电脑IP与8118端口即可完成代理

以上两种方式VPN+Shadowsocks貌似都可以在路由器层面完成，由于我没有类似的设备就没做相应尝试了，后期倒是考虑将代理配置移入局域网的NAS，这样每次就不用开电脑了...

参考资料:

* [用Mac为PS3 配置一个本地的http代理服务器](http://www.douban.com/note/330572002/?type=rec)
* [shadowsocks Privoxy实现socks5代理转http代理](http://www.wllog.net/2014/10/16/1042.html)

---

**延伸阅读**

* [为什么有些使用联通、网通、移动的用户联机速度比电信快不少](http://www.v2ex.com/t/144697)？
    * 这是由于国内大部分电信用户走的骨干网路线路为老的163，中国电信是严格控制出口带宽...部分企业用户和高端个人用户（上海的国际精品网）走的CN2网络相对好很多
    * 联通、网通、移动的出口带宽貌似是有机房直连香港的机房，所以出口速度相对较快（不确定）
    
* [如何规避电信对于出口的限制？](http://www.v2ex.com/go/shadowsocks)
    * 在国内寻找出口不受限制的跳板机，这样每次上午的流量就是出口流量->国内跳板机->本机，应该是可以突破这个限制
    
---


    It was the best of times, it was the worst of times, it was the age of wisdom, it was the age of foolishness, it was the epoch of belief, it was the epoch of incredulity, it was the season of Light, it was the season of Darkness, it was the spring of hope, it was the winter of despair. We had everything before us, we had nothing before us, we were all going direct to Heaven, we were all going direct the other way--in short.
        
    这是最好的时代，这是最坏的时代；

    这是智慧的年代，这是愚蠢的年代；

    这是信仰的时期，这是怀疑的时期；

    这是光明的季节，这是黑暗的季节；

    这是希望之春，这是绝望之冬；

    我们拥有一切，我们一无所有；

    我们正走向天堂，我们都在奔向与其相反的地方； 
    
    --「双城记」
    


---



-EOF-