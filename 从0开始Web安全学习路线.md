> 网络安全涉及的内容非常广，其主要的学习路线如下图所示。可以简单归纳为：

* 基础知识：Web发展简史、计算机网络、域名系统、HTTP标准、代码审计、WAF

* 信息收集：域名信息、 站点信息、端口信息、其它

* 内网渗透：Windows信息收集、持久化、Linux信息收集、痕迹清理、其他

* 常见漏洞：SQL注入、XSS、CSRF、SSRF、命令注入、文件读取、文件上传、文件包含、XXE、模版注入、Xpath注入、 逻辑漏洞 、业务漏洞、配置安全、中间件、Web Cache欺骗攻击

* 语言与框架：PHP、Python、Java、JavaScript、Ruby、C\C++、C#

* 防御技术：总体思路、 团队建设、威胁情报、风险控制、加固检查、蜜罐技术、入侵检测、应急响应、溯源分析

* 工具与资源：工具列表、推荐资源、爆破工具、下载工具、流量相关、嗅探工具、SQLMap使用

* 其他：认证方式、拒绝服务攻击、DNS劫持、Docker

### 学习基础 时间：1 周 ~ 2 周：

① 我们用这段时间了解基本的概念：（SQL 注入、XSS、上传、CSRF、一句话木马、常见的后台等：可以通过 Google 搜索获取资料）为之后的 WEB 渗透测试打下基础。  
② 查看一些论坛的一些 Web 渗透资料，学一学案例的思路，每一个站点都不一样，所以思路是主要的。  
③ 学会提问的艺术，如果遇到不懂得要善于提问。

### 配置渗透环境 时间：3 周 ~ 4 周：

① 了解渗透测试常用的工具，例如（AWVS、SQLMAP、NMAP、APPSCAN、BURP、中国菜刀等）。  
② 下载这些工具无后门版本并且安装到计算机上，并做一个工具包，推荐 Rolan。  
③ 了解这些工具的使用场景，懂得基本的使用，推荐在 Secwiki 或者 Google 上查找资料。  

### 渗透实战操作 时间：约 6 周：

① 在网上搜索渗透实战案例，深入了解 SQL 注入、文件上传、解析漏洞等在实战中的使用。  
② 自己搭建漏洞环境测试，推荐 DWVA，SQLi-labs，Upload-labs，bWAPP。  
③ 懂得渗透测试的阶段，每一个阶段需要做那些动作：例如 PTES 渗透测试执行标准。  
④ 深入研究手工 SQL 注入，寻找绕过 waf 的方法，制作自己的脚本。  
⑤ 研究文件上传的原理，如何进行截断、双重后缀欺骗 (IIS、PHP)、解析漏洞利用（IIS、Nignix、Apache）等，参照：上传攻击框架。  
⑥ 了解 XSS 形成原理和种类，在 DWVA 中进行实践，使用一个含有 XSS 漏洞的 cms，安装安全狗等进行测试。  
⑦ 了解一句话木马，并尝试编写过狗一句话。  
⑧ 研究在 Windows 和 Linux 下的提升权限，Google 关键词：提权

### 经常逛网络安全有关的网站 时间：∞

① 例如：Freebuf、i 春秋、安全客、看雪、91Ri.org、Sec-wiki、安全脉搏、Sec 圈子社区、T00ls 论坛等。  
② 遇到有意义的文章可以转载到自己博客

### 熟悉 Windows & Kali Linux 系统 时间：2 周 ~ 4 周

①了解 Windows 系统下的常用命令，如：ipconfig,nslookup,tracert,net,tasklist,taskkill 等。  
② 熟悉 Linux 系统的常用命令，如：wget、mv、cd、rm、mkdir 等。  
③ 熟悉 Kali Linux 系统下的常用工具，可以看看安全牛课堂上苑房弘老师的 Kali 课程 [推荐 1.5 倍速播放]，可以参考 SecWiki,《Web Penetration Testing with Kali Linux》、《Hacking with Kali》等。

### 学习服务器的安全配置 时间：4 周左右

① 了解 03、08、12 系统下 iis 的基本配置，了解 Win 下的目录权限（例如 iis 写权限），建立一个简单的站点。  
② 了解 Linux 的运行权限、跨目录、文件夹权限，学会配置 Linux Web 服务器，并建立一个简单的站点。  
③ 使用自动化工具扫描已经建立好的站点，并利用 Google 学会修补漏洞。  
④ 学会打补丁、iptables 限制端口、添加规则等。  
⑤ 下载一款 waf 软件，熟悉它的使用。

### 学习一些编程知识 时间：约 8 周

① 在 w3cschool 上学习 html、php、数据库的基础，建议每一种学到第 8 节就可以了。  
② 学习 Python（也可以是其他语言，但是强烈建议使用 python）。要求学习：爬虫（基础）、多线程、文件操作、正则表达式（基础）还有一些常用的第三方库，可能需要安装 pip。  
③ 利用 python 写一个简单的 poc 或者 exp。  
④ 开发一些渗透时会用到的程序，例如：端口扫描等。  
⑤ 选择一个 php 框架进行学习，不要太深入。

### 学习代码审计 时间：4 周 ~ 6 周

① 了解代码审计的静态和动态方法，懂得分析程序。  
② 在乌云镜像里找到开源的漏洞程序，跟着学习分析方法，尝试自己分析 3~5 次代码。  
③ 了解 web 漏洞形成的原因，熟悉常见漏洞函数。  

### 安全体系开发 时间：∞

① 开发一些安全工具，并将其开源，可以托管到码云或者 github 上，展示个人实力。  
② 建立自己的一套安全体系，拥有独立的思路方法。

**PTES 执行标准**

http://www.doc88.com/p-7784047461299.html  
