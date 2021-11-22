# id1e

<!-- TOC -->

- [id1e](#id1e)
  - [Web](#web)
    - [同源策略 SOP & 跨域资源共享 CORS](#同源策略-sop--跨域资源共享-cors)
    - [内容安全策略 CSP](#内容安全策略-csp)
    - [SQL 注入](#sql-注入)
      - [关键函数和关键表](#关键函数和关键表)
      - [延时注入](#延时注入)
      - [报错注入](#报错注入)
      - [order by 注入](#order-by-注入)
      - [宽字节注入](#宽字节注入)
      - [二次注入](#二次注入)
      - [堆叠注入](#堆叠注入)
      - [绕过](#绕过)
    - [NoSQL 注入](#nosql-注入)
    - [XSS](#xss)
      - [其他](#其他)
    - [CSRF](#csrf)
    - [SSRF](#ssrf)
    - [XXE](#xxe)
    - [SSTI](#ssti)
    - [文件包含](#文件包含)
    - [文件上传/解析漏洞](#文件上传解析漏洞)
      - [二次渲染](#二次渲染)
    - [文件下载](#文件下载)
    - [PHP](#php)
      - [反序列化](#反序列化)
      - [伪协议](#伪协议)
      - [弱类型](#弱类型)
      - [随机数安全](#随机数安全)
      - [变量覆盖](#变量覆盖)
      - [代码执行](#代码执行)
    - [Java-web](#java-web)
      - [Java 反序列化](#java-反序列化)
    - [python](#python)
    - [前端安全](#前端安全)
    - [其他漏洞](#其他漏洞)
      - [HTTP 走私攻击](#http-走私攻击)
      - [LDAP 注入](#ldap-注入)
      - [CRLF 注入](#crlf-注入)
    - [CVE](#cve)
      - [CVE-2015-4553](#cve-2015-4553)
      - [CVE-2019-0193](#cve-2019-0193)
      - [CVE-2018-12613](#cve-2018-12613)
      - [ThinkPHP 5](#thinkphp-5)
    - [内网渗透](#内网渗透)
  - [Misc](#misc)
  - [CTF](#ctf)
    - [2019 强网杯](#2019-强网杯)
    - [2019 DDCTF](#2019-ddctf)
    - [2019 SCTF](#2019-sctf)
    - [2019 SUCTF](#2019-suctf)
    - [2020 MRCTF](#2020-mrctf)
    - [2020 BJDCTF](#2020-bjdctf)
    - [2020 网鼎杯青龙组](#2020-网鼎杯青龙组)

<!-- /TOC -->

***

## Web

### 同源策略 SOP & 跨域资源共享 CORS

[漫谈同源策略攻防](https://www.anquanke.com/post/id/86078)

[再谈同源策略](https://lightless.me/archives/review-SOP.html)

### 内容安全策略 CSP

[CSP](https://xz.aliyun.com/t/5084)

### SQL 注入

[MySQL 注入攻击与防御](https://www.anquanke.com/post/id/85936)

#### 关键函数和关键表

#### 延时注入

[延时注入新思路](https://xz.aliyun.com/t/2288)

#### 报错注入

[报错注入原理分析](https://www.cnblogs.com/Triomphe/p/9489639.html)

#### order by 注入

#### 宽字节注入

#### 二次注入

#### 堆叠注入

[2019强网杯随便注](#2019-%e5%bc%ba%e7%bd%91%e6%9d%af)

#### 绕过

[逗号绕过](https://www.cnblogs.com/i-honey/p/8203954.html)

[sql注入绕过](https://www.cnblogs.com/Vinson404/p/7253255.html)

[SQL注入的“冷门姿势”](https://www.freebuf.com/articles/web/155876.html)

[sql注入绕过姿势总结](https://www.cnblogs.com/joker-vip/p/12698962.html)

[PDO(预编译参数化查询)和安全问题](https://www.cnblogs.com/wangtanzhi/p/12927199.html)

### NoSQL 注入

[NoSQL注入的分析与缓解](https://www.cnblogs.com/wangyayun/p/6598166.html)

[谈谈我对NoSQL注入的一点研究](https://pockr.org/guest/activity?activity_no=act_761e1e744d8aa16823&speech_no=sp_26a751c506f61078b0)

[SQL和NoSQL注入浅析 (上)](https://xz.aliyun.com/t/2075)

[SQL和NoSQL注入浅析 (下)](http://liehu.tass.com.cn/archives/1294)

### XSS

[再谈同源策略](https://lightless.me/archives/review-SOP.html)

[浏览器解析机制和XSS向量](http://bobao.360.cn/learning/detail/292.html)

[CSP内容安全策略总结及如何抵御 XSS 攻击](https://www.cnblogs.com/goloving/p/11186176.html)

#### 其他

```Javascript
<details open ontoggle=prompt(111111111)>
```

### CSRF

[CSRF攻击与防御](https://www.cnblogs.com/phpstudy2015-6/p/6771239.html)

### SSRF

[SSRF漏洞分析与利用](http://www.4o4notfound.org/index.php/archives/33/)

[Gopher协议在SSRF漏洞中的深入研究](https://zhuanlan.zhihu.com/p/112055947)

[SSRF绕过ip限制](https://www.freebuf.com/articles/web/135342.html)

[SSRF in PHP](https://joychou.org/web/phpssrf.html)

[关于DNS-rebinding的总结](http://www.bendawang.site/2017/05/31/%E5%85%B3%E4%BA%8EDNS-rebinding%E7%9A%84%E6%80%BB%E7%BB%93/)

[通过 DNS Rebinding 获取访客 QQ 号](https://segmentfault.com/p/1210000010014129/read)

[DNS Rebinding技术绕过SSRF/代理IP限制](http://www.mottoin.com/detail/906.html)

### XXE

[一篇文章带你深入理解漏洞之 XXE 漏洞](https://xz.aliyun.com/t/3357#toc-13)

[浅谈XXE漏洞攻击与防御](https://thief.one/2017/06/20/1/)

[LCTF 2018 T4lk 1s ch34p,sh0w m3 the sh31l 详细分析](https://www.k0rz3n.com/2018/11/19/LCTF%202018%20T4lk%201s%20ch34p,sh0w%20m3%20the%20sh31l%20%E8%AF%A6%E7%BB%86%E5%88%86%E6%9E%90/)

[xxe绕过](https://xz.aliyun.com/t/4059)

### SSTI

[模板注入](https://blog.csdn.net/qq_40827990/article/details/82940894)

[Flask/Jinja2 SSTI](https://www.kingkk.com/2018/06/Flask-Jinja2-SSTI-python-%E6%B2%99%E7%AE%B1%E9%80%83%E9%80%B8/)

### 文件包含

[php文件包含漏洞](https://chybeta.github.io/2017/10/08/php%E6%96%87%E4%BB%B6%E5%8C%85%E5%90%AB%E6%BC%8F%E6%B4%9E/)

### 文件上传/解析漏洞

[bypass文件上传](https://paper.seebug.org/219/)

[Web渗透之文件上传漏洞总结](https://www.secpulse.com/archives/95987.html)

[文件上传漏洞解析及简单绕过技巧](https://blog.csdn.net/qq_35569814/article/details/100587817)
 
#### 二次渲染

[2019ddctf](#2019-ddctf)

### 文件下载

[2020网鼎杯青龙组filejava](#2020-%e7%bd%91%e9%bc%8e%e6%9d%af%e9%9d%92%e9%be%99%e7%bb%84)

### PHP

#### 反序列化

[php反序列化](https://www.anquanke.com/post/id/86452)

[PHP反序列化入门之phar](https://www.freebuf.com/column/198945.html)

#### 伪协议

[php伪协议](https://lorexxar.cn/2016/09/14/php-wei/)

[谈一谈php://filter的妙用](https://www.leavesongs.com/PENETRATION/php-filter-magic.html)

[PHP中filter协议详解](https://www.php.cn/php-weizijiaocheng-387051.html)

[利用 Gopher 协议拓展攻击面](https://blog.chaitin.cn/gopher-attack-surfaces/)

[PHP伪协议分析与应用](http://www.4o4notfound.org/index.php/archives/31/)

[php伪协议实现命令执行的七种姿势](https://www.freebuf.com/column/148886.html)

[LFI、RFI、PHP封装协议安全问题学习](https://www.cnblogs.com/LittleHann/p/3665062.html)

#### 弱类型

[php字符串解析特性](https://www.freebuf.com/articles/web/213359.html)

#### 随机数安全

[PHP mt_rand()随机数安全](https://mp.weixin.qq.com/s/3TgBKXHw3MC61qIYELanJg)

#### 变量覆盖

#### 代码执行

[CTF命令执行及绕过技巧](https://blog.csdn.net/JBlock/article/details/88311388)

[PHP Code Injection Analysis](http://www.polaris-lab.com/index.php/archives/254/)

### Java-web

#### Java 反序列化

[Java反序列化学习之Apache Commons Collections](https://p0sec.net/index.php/archives/121/)

[深入理解JAVA反序列化漏洞](https://www.vulbox.com/knowledge/detail/?id=11)

[Java反序列化漏洞-玄铁重剑之CommonsCollection(上)](https://xz.aliyun.com/t/2028)

[Java反序列化漏洞-玄铁重剑之CommonsCollection(下)](https://xz.aliyun.com/t/2029)

[Java反序列化漏洞从入门到深入](https://xz.aliyun.com/t/2041)

[Java反序列化漏洞从无到有](https://www.freebuf.com/column/155381.html)

[DDCTF2019](#2019-ddctf)

[SCTF2019](#2019-sctf)

### python

### 前端安全

[深入理解 JavaScript Prototype 污染攻击](https://www.leavesongs.com/PENETRATION/javascript-prototype-pollution-attack.html#0x01-prototype__proto__)

[从一道 CTF 题看 Node.js 的 prototype pollution attack](https://xz.aliyun.com/t/2802)

[JavaScript原型链污染攻击](https://blog.csdn.net/qq_41107295/article/details/95789944?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

### 其他漏洞

#### HTTP 走私攻击

[HTTP走私攻击](https://paper.seebug.org/1048/)

[chunked编码](https://blog.csdn.net/yankai0219/article/details/8269922)

[2019 blackhat](https://www.blackhat.com/us-19/briefings/schedule/#http-desync-attacks-smashing-into-the-cell-next-door-15153)

#### LDAP 注入

[理解LDAP与LDAP注入](https://www.cnblogs.com/bendawang/p/5156562.html)

[从一次漏洞挖掘入门Ldap注入](https://xz.aliyun.com/t/5689)

#### CRLF 注入

[CRLF注入学习](https://www.cnblogs.com/iceli/p/8595125.html)

### CVE

#### CVE-2015-4553

#### CVE-2019-0193

[Apache Solr远程代码执行漏洞](https://xz.aliyun.com/t/5965)

#### CVE-2018-12613

[HCTF WARMUP](https://www.jianshu.com/p/36eaa95068ca)

#### ThinkPHP 5

[Thinkphp_rce](https://www.vulnspy.com/cn-thinkphp-5.x-rce/)

### 内网渗透

[内网渗透基础知识](https://www.cnblogs.com/-mo-/p/11906772.html)

[后渗透之meterpreter使用攻略](https://xz.aliyun.com/t/2536)

[DNS隧道技术调研](https://blog.csdn.net/Fly_hps/article/details/80628885)

[内网转发工具的使用](https://blog.csdn.net/qq_36119192/article/details/84568266#%E5%86%85%E7%BD%91%E8%BD%AC%E5%8F%91)

[SSH隧道与端口转发及内网穿透](http://blog.creke.net/722.html)

[ew](https://www.cnblogs.com/0nth3way/p/11693188.html)

[socat](https://payloads.online/tools/socat#socat%E7%AE%80%E4%BB%8B)

## Misc

## CTF

### 2019 强网杯

[2019 强网杯](https://www.ctfwp.com/%E5%AE%98%E6%96%B9%E8%B5%9B%E4%BA%8B%E9%A2%98/2019%E5%BC%BA%E7%BD%91%E6%9D%AF)

### 2019 DDCTF

[2019 DDCTF](https://www.anquanke.com/post/id/178434)

### 2019 SCTF

[2019 SCTF](https://www.ctfwp.com/%E5%AE%98%E6%96%B9%E8%B5%9B%E4%BA%8B%E9%A2%98/2019SCTF)

### 2019 SUCTF

[2019 SUCTF](https://www.jianshu.com/p/4ab4b161749f)

### 2020 MRCTF

[2020 MRCTF](https://www.ctfwp.com/%E5%AE%98%E6%96%B9%E8%B5%9B%E4%BA%8B%E9%A2%98/2020MRCTF)

### 2020 BJDCTF

[2020 BJDCTF](https://www.ctfwp.com/%E5%AE%98%E6%96%B9%E8%B5%9B%E4%BA%8B%E9%A2%98/2020%E7%AC%AC%E4%BA%8C%E5%B1%8ABJDCTF)

### 2020 网鼎杯青龙组

[2020 网鼎杯青龙组](https://www.gem-love.com/websecurity/2322.html)
