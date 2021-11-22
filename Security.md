# Security

***

- [Security](#security)
  - [Web](#web)
    - [同源策略(SOP) & 跨域资源共享(CORS)](#同源策略sop--跨域资源共享cors)
    - [内容安全策略(CSP)](#内容安全策略csp)
    - [SQL注入](#sql注入)
      - [基础](#基础)
      - [延时注入](#延时注入)
      - [报错注入](#报错注入)
      - [order by 注入](#order-by-注入)
      - [宽字节注入](#宽字节注入)
      - [二次注入](#二次注入)
      - [堆叠注入](#堆叠注入)
      - [绕过](#绕过)

***

## Web

> Web Security

### 同源策略(SOP) & 跨域资源共享(CORS)

[漫谈同源策略攻防](https://www.anquanke.com/post/id/86078)

[再谈同源策略](https://lightless.me/archives/review-SOP.html)

### 内容安全策略(CSP)

[CSP](https://xz.aliyun.com/t/5084)

### SQL注入

#### 基础

|          语句          | 作用                                                                               |
| :--------------------: | :--------------------------------------------------------------------------------- |
| **应用隔离与沙箱机制** | 应用程序隔离将限制被利用的目标对其他进程和系统功能的访问                           |
|      **Exp防护**       | **WAF** 可用于限制应用程序的暴露                                                   |
|      **网络隔离**      | 使用 **DMZ** 或在独立的托管基础设施，将面向外部的服务器和服务与网络的其他部分分离  |
|    **账号权限控制**    | 最小化特权                                                                         |
|      **软件更新**      | 确保应用保持最新                                                                   |
|      **更新软件**      | 定期扫描外部系统的漏洞，并建立程序以在通过扫描和公开披露发现重大漏洞时快速修补系统 |

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