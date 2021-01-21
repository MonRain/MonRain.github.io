---
title: KDD99数据集的特征(Features)介绍
date: 2021-01-17 19:02:19
categories: 机器学习应用于信息安全
tags:
- KDD99
- 信息安全
- 机器学习
---



# KDD99数据集的特征(Features)介绍

KDD99是一个用来从正常连接中监测非正常连接的数据集。产出于1999年Thrid International Knowlegde Discovery and Data Mining Tools Competition，其目的是建立一个稳定的的入侵检测系统。


KDD99包含了置入攻击的军事网络环境中的记录。攻击可以分类为：

+ **DoS攻击**：Denial of Service
+ **R2U**：Remote to User
+ **U2R**：User to Root
+ **探针攻击**：Probing

KDD99数据集是 *DARPA数据集* 的 **特征提取(Feature Extract)** 版本（ *DARPA* 是原始数据集）KDD99对每个连接提取了 **41** 个特征，使用**Bro-IDS**工具对数据贴标签。

其41个特征可以按以下方式分类：

+ 1-9	TCP连接的基本特征
+ 10-22	TCP连接的内容特征
+ 23-31	基于时间的网络流量统计特征，使用2秒的时间窗(Traffic features computed using a two-second time window)
+ 32-41	基于主机的网络流量统计特征，主机特征(Host features)，用来评估持续时间在两秒钟以上的攻击

## TCP连接的基本特征

feature name|description|type
:--:|--|:--:
duration|length (number of seconds) of the connection<br>连接的持续时间，以秒(s)为单位<br>**[0 ~ 58329]**<br>它的定义是从TCP连接以3次握手建立算起，到FIN/ACK连接结束为止的时间；若为UDP协议类型，则将每个UDP数据包作为一条连接。(数据集中出现大量的duration=0 的情况，是因为该条连接的持续时间不足1秒.)|continuous<br>连续
protocol_type|type of the protocol, e.g. tcp, udp, etc.<br>协议类型，此数据集中有三种：<br>**TCP, UDP, ICMP**|discrete<br>离散
service|network service on the destination, e.g., http, telnet, etc.<br>连接目的端的网络服务。有70+种：<br>**aol, auth, bgp, courier, csnet_ns, ctf, daytime, discard, domain, domain_u, echo, eco_i, ecr_i, efs, exec, finger, ftp, ftp_data, gopher, harvest, hostnames, http, http_2784, http_443, http_8001, imap4, IRC, iso_tsap, klogin, kshell, ldap, link, login, mtp, name, netbios_dgm, netbios_ns, netbios_ssn, netstat, nnsp, nntp, ntp_u, other, pm_dump, pop_2, pop_3, printer, private, red_i, remote_job, rje, shell, smtp, sql_net, ssh, sunrpc, supdup, systat, telnet, tftp_u, tim_i, time, urh_i, urp_i, uucp, uucp_path, vmnet, whois, X11, Z39_50**|discrete<br>离散
src_bytes|number of data bytes from source to destination<br>从源主机到目的主机数据的字节数<br>**[0 ~ 1379963888]**|continuous<br>连续
dst_bytes|number of data bytes from destination to source<br>从目的主机到源主机数据的字节数<br>**[0 ~ 1309937401]**|continuous<br>连续
flag|normal or error status of the connection<br>连接状态正常或错误的标志，共11中<br>**OTH, REJ, RSTO, RSTOS0, RSTR, S0, S1, S2, S3, SF, SH**<br>表示该连接是否按照协议要求开始或完成。例如SF表示连接正常建立并终止；S0表示只接到了SYN请求数据包，而没有后面的SYN/ACK。其中SF表示正常，其他10种都是error。<br>11种状态的详细解释，参考文章[4]|discrete<br>离散
land|1 if connection is from/to the same host/port; 0 otherwise<br> **1:** 连接来自/到同一主机/端口<br> **0:** 其它|discrete<br>离散
wrong_fragment|number of ``wrong'' fragments<br>“错误”片段的数量<br>**[0 ~ 3]**|continuous<br>连续
urgent|number of urgent packets<br>urgent加急包数量<br>**[0 ~ 14]**|continuous<br>连续

<center>

Table 1: Basic features of individual TCP connections.

表1：TCP连接的基本特征

</center>

## TCP连接的内容特征

feature name|description|type
:--:|--|:--:
hot|number of ``hot'' indicators<br>访问系统敏感文件和目录的次数<br>**[0 ~ 101]**<br>例如访问系统目录，建立或执行程序等|continuous<br>连续
num_failed_logins|number of failed login attempts<br>登录尝试失败的次数。<br>**[0 ~ 5]**|continuous<br>连续
logged_in|1 if successfully logged in<br>0 otherwise<br>**1**：成功登录<br>**0**：其它|discrete<br>离散
num_compromised|number of ``compromised'' conditions<br>'compromised'条件出现的次数<br>**[0 ~ 7479]**|continuous<br>连续
root_shell|1 if root shell is obtained; 0 otherwise<br>**1**：获得root shell<br>**0**：其它|discrete<br>离散
su_attempted|1 if ``su root'' command attempted; 0 otherwise<br>**1**：出现'su root'<br>**0**：其它|discrete<br>离散
num_root|number of ``root'' accesses<br>root用户访问次数<br>**[0 ~ 7468]**|continuous<br>连续
num_file_creations|number of file creation operations<br>文件创建操作的次数<br>**[0 ~ 100]**|continuous<br>连续
num_shells|number of shell prompts<br>使用shell命令的次数<br>**[0 ~ 5]**|continuous<br>连续
num_access_files|number of operations on access control files<br>访问控制文件的次数<br>**[0 ~ 9]**|continuous<br>连续
num_outbound_cmds|number of outbound commands in an ftp session<br>一个FTP会话种出现连接的次数<br>**数据集种这一特征出现次数为0**|continuous<br>连续
is_hot_login|1 if the login belongs to the ``hot'' list; 0 otherwise<br>**1**：登录属于'hot'列表<br>**0**：其它<br>如超级用户或管理员登录|discrete<br>离散
is_guest_login|1 if the login is a ``guest''login; 0 otherwise<br>**1**：guest登录<br>**0**：其它|discrete<br>离散

<center>

Table 2: Content features within a connection suggested by domain knowledge.

表2：TCP连接的内容特征

</center>

## 基于时间的网络流量统计特征

feature name|description|type
:--:|--|:--:
count|number of connections to the same host as the current connection in the past two seconds<br>**Note:** The following  features refer to these same-host connections.<br>过去两秒内，与当前连接具有相同的目标主机的连接数。<br>**[0 ~ 511]**<br> **注意：** 以下特征连接到相同主机|continuous<br>连续
srv_count|number of connections to the same service as the current connection in the past two seconds<br>**Note:** The following features refer to these same-service connections.<br>过去两秒内，与当前连接具有相同服务的连接数<br>**[0 ~ 511]**<br> **注意：** 以下特征连接到相同服务|continuous<br>连续
serror_rate|% of connections that have ``SYN'' errors<br>过去两秒内，在与当前连接具有相同目标主机的连接中，出现“SYN” 错误的连接的百分比<br>**[0.00 ~ 1.00]**|continuous<br>连续
rerror_rate|% of connections that have ``REJ'' errors<br>过去两秒内，在与当前连接具有相同目标主机的连接中，出现“REJ” 错误的连接的百分比<br>**[0.00 ~ 1.00]**|continuous<br>连续
same_srv_rate|% of connections to the same service<br>过去两秒内，在与当前连接具有相同目标主机的连接中，与当前连接具有相同服务的连接的百分比<br>**[0.00 ~ 1.00]**|continuous<br>连续
diff_srv_rate|% of connections to different services<br>过去两秒内，在与当前连接具有相同目标主机的连接中，与当前连接具有不同服务的连接的百分比<br>**[0.00 ~ 1.00]**|continuous<br>连续
srv_serror_rate|% of connections that have ``SYN'' errors<br>过去两秒内，在与当前连接具有相同服务的连接中，出现“SYN” 错误的连接的百分比<br>**[0.00 ~ 1.00]**|continuous<br>连续
srv_rerror_rate|% of connections that have ``REJ'' errors<br>过去两秒内，在与当前连接具有相同服务的连接中，出现“REJ” 错误的连接的百分比<br>**[0.00 ~ 1.00]**|continuous<br>连续
srv_diff_host_rate|% of connections to different hosts<br>过去两秒内，在与当前连接具有相同服务的连接中，与当前连接具有不同目标主机的连接的百分比<br>**[0.00 ~ 1.00]**|continuous<br>连续 
<font size=2>

+ count、serror_rate、rerror_rate、same_srv_rate、diff_srv_rate这5个特征是 same host特征，前提都是与当前连接具有相同目标主机的连接；

+ srv_count、srv_serror_rate、srv_rerror_rate、srv_diff_host_rate这4个特征是same service特征，前提都是与当前连接具有相同服务的连接。

</font>

<center>
Table 3: Traffic features computed using a two-second time window.

表 3：基于时间的网络流量统计特征
</center>

## 基于主机的网络流量统计特征

feature name|description|type
:--:|--|:--:
dst_host_count|前100个连接中，与当前连接具有相同目标主机的连接数<br>**[0 ~ 255]**|连续
dst_host_srv_count|前100个连接中，与当前连接具有相同目标主机相同服务的连接数<br>**[0 ~ 255]**|连续
dst_host_same_srv_rate|前100个连接中，与当前连接具有相同目标主机相同服务的连接所占的百分比<br>**[0.00 ~ 1.00]**|连续
dst_host_diff_srv_rate|前100个连接中，与当前连接具有相同目标主机不同服务的连接所占的百分比<br>**[0.00 ~ 1.00]**|连续
dst_host_same_src_port_rate|前100个连接中，与当前连接具有相同目标主机相同源端口的连接所占的百分比<br>**[0.00 ~ 1.00]**|连续
dst_host_srv_diff_host_rate|前100个连接中，与当前连接具有相同目标主机相同服务的连接中，与当前连接具有不同源主机的连接所占的百分比<br>**[0.00 ~ 1.00]**|连续
dst_host_serror_rate|前100个连接中，与当前连接具有相同目标主机的连接中，出现SYN错误的连接所占的百分比<br>**[0.00 ~ 1.00]**|连续
dst_host_srv_serror_rate|前100个连接中，与当前连接具有相同目标主机相同服务的连接中，出现SYN错误的连接所占的百分比<br>**[0.00 ~ 1.00]**|连续
dst_host_rerror_rate|前100个连接中，与当前连接具有相同目标主机的连接中，出现REJ错误的连接所占的百分比<br>**[0.00 ~ 1.00]**|连续
dst_host_srv_rerror_rate|前100个连接中，与当前连接具有相同目标主机相同服务的连接中，出现REJ错误的连接所占的百分比<br>**[0.00 ~ 1.00]**|连续

<center>

表 4:基于主机的网络流量统计特征（[KDD99官网的task部分](http://kdd.ics.uci.edu/databases/kddcup99/task.html)没找到对应表格）

</center>

## 其它
KDD99在研究者当中十分流行，研究者也对其本身做了很多工作：
+ [\*]减少特征数量，从最初的41个特征中选择最有用的特征
+ [\*]指出了KDD99的不足之处
	> 1. KDD99面临不平衡的分类方法问题。测试集和训练集的概率分布是不同的，由于在训练集中加入新的攻击，攻击和正常流量的类别的平衡会被打破。[?]
	> 2. 数据集太老了，可能存在过时的问题
	> 3. 有研究表明，该数据集存在导致对异常检测性能的过高估计的可能性

## 参考资料
[1]. CHAABOUNI N, MOSBAH M, ZEMMARI A, et al. Network Intrusion Detection for IoT Security Based on Learning Techniques [J]. Ieee Communications Surveys and Tutorials, 2019, 21(3): 2671-701.
[2]. [KDD Cup 1999 Data](http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html)
[3]. [KDD99数据集与NSL-KDD数据集介绍](https://mathpretty.com/10244.html) *BTW： 这篇博客对 KDD99 和 NSL-KDD 写的很详细*
[4]. Song J, Takakura H, Okabe Y. Description of kyoto university benchmark data[J]. Available at link: http://www.takakura.com/Kyoto_data/BenchmarkData-Description-v5.pdf [Accessed on 15 March 2016], 2006.
[5]. Özgür A, Erdem H. A review of KDD99 dataset usage in intrusion detection and machine learning between 2010 and 2015[J]. PeerJ Preprints, 2016, 4: e1954v1.