---
title: Let's Encrypt：用免费的 SSL 证书，让网站支持 HTTPS
date: 2017-12-31 18:23:46
tags: [HTTPS, Encrypt, SSL证书]
---


申请 Let's Encrypt 提供的免费 SSL 证书，配置网站使用 HTTPS，让网站应用更安全。
直接在运行网站的服务器上去申请 SSL 证书更方便一些，你也可以在一台服务器上去为在
其它服务器上运行的网站申请 SSL 证书，这需要做额外的验证。操作都非常简单，不用紧张：）
<!-- more -->
我在一台 CentOS  7 服务器上申请了 Let's Encrypt 签发的 SSL 证书，我的网站的 Web 服务器用的是 Nginx。

## 安装EPEL仓库

在 CentOS 的 EPEL 仓库里可以找到签发 Let's Encrypt 证书用的工具。所以先安装一下这个仓库，执行：

```
sudo yum install epel-release -y
```

公司内网不能用，可以在本地虚拟机安装，然后通过下边的『为其他服务器申请证书的流程走』

## 安装签发证书工具

用 Certbot 工具申请 Let's Encrypt 证书，先安装一下：

```
sudo yum install certbot-nginx
```

运行 certbot 工具时报错：

```
ImportError: No module named 'requests.packages.urllib3'
```

执行下面的命令解决了上面的问题：

```
pip install --upgrade --force-reinstall 'requests==2.6.0' urllib3
```

## 申请证书
### 网站运行在申请 SSL 证书用的服务器上

你要申请 SSL 证书的主机名已经指向了申请用的服务器的 IP 地址，
在服务器上用的是 Nginx Web 服务器。保证 Nginx 正在运行，并
且已经配置好了 Nginx ，绑定上了要申请 SSL 证书的主机名。就是
你应该在 Nginx 的配置里面，有一个配置是为要申请 SSL 证书的主
机名准备的。

一开始我们在服务器上安装的 Certbot 可以自动修改指定主机名的
 Nginx 配置，为主机名申请证书，下载证书，并且会自动修改 Nginx
 的配置，让它支持 HTTPS。

执行：

```
sudo certbot --nginx
```

Certbot 会自动查找当前服务器上的 Nginx 里面的虚拟主机。提示：

```
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Starting new HTTPS connection (1): acme-v01.api.letsencrypt.org

Which names would you like to activate HTTPS for?
-------------------------------------------------------------------------------
1: houjiyi.com
2: galary.houjiyi.com
3: www.houjiyi.com
-------------------------------------------------------------------------------
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel):
```

比如我的服务器上有两个 Nginx 虚拟主机，上面会提示想给哪个主机名申请 SSL 证书，输入序号选择，
或留为空白直接回车选择所有。验证成功，Let's Encrypt 就会立即给你签发 SSL 证书，并且会自动
修改主机的 Nginx 配置，让它支持 HTTPS。过程中会提示：

```
Obtaining a new certificate
Performing the following challenges:
tls-sni-01 challenge for houjiyi.com
Waiting for verification...
Cleaning up challenges
Deployed Certificate to VirtualHost /etc/nginx/conf.d/www.conf for set(['houjiyi.com'])

Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
-------------------------------------------------------------------------------
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```

意思是，想不想把 HTTP 的请求重定向到 HTTPS ，如果选择 Redirect，Certbot 会自动在主机的 Nginx 配置里添加重定向用的配置。成功以后会提示：

```
The appropriate server block is already redirecting traffic. To enable redirect anyway, uncomment the redirect lines in /etc/nginx/conf.d/www.conf.

-------------------------------------------------------------------------------
Congratulations! You have successfully enabled https://houjiyi.com

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=houjiyi.com
-------------------------------------------------------------------------------

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/houjiyi.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/houjiyi.com/privkey.pem
   Your cert will expire on 2018-04-02. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

上面会告诉你申请的 Let's Encrypt 证书保存的位置，一般是在：

```
/etc/letsencrypt/live/主机名
```

再打开网站的 Nginx 配置检查一下，配置里会有一些东西是 Certbot 自动添加的，配置的后面都会有注释 managed by Certbot：

```
server {
    listen        80;
    server_name   houjiyi.com;

    listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live/houjiyi.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/houjiyi.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;


    if ($scheme = "https") {
        return 301 https://www.$host$request_uri;
    }
}
```

如果你只想为跟 Nginx 绑定的主机名申请证书，不想让 Certbot 修改 Nginx 的配置，可以执行：

```
sudo certbot --nginx certonly
```

### 网站运行在申请 SSL 证书以外的服务器上

我们在一个 CentOS 服务器上，也可以为运行在其它服务器上的网站申请 Let's Encrypt 证书，只需要多做一步验证。

执行：

```
sudo certbot certonly --manual
```

提示让你输入想要申请证书的主机名：

```
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator manual, Installer None
Starting new HTTPS connection (1): acme-v01.api.letsencrypt.org
Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c'
to cancel): t2.m.le.com
```

继续又会提示要记录服务器的 IP 地址，是不是同意这样：

```
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for t2.m.le.com

-------------------------------------------------------------------------------
NOTE: The IP of this machine will be publicly logged as having requested this
certificate. If you're running certbot in manual mode on a machine that is not
your server, please ensure you're okay with that.

Are you OK with your IP being logged?
-------------------------------------------------------------------------------
(Y)es/(N)o: Y
```

下面要做一步验证，把一串字符放在指定的文件里，而且要用指定的路径可以访问到这个文件，这样就可以完成验证了。

```
Create a file containing just this data:

KIad2acPdKiF5pLpjlv4fK19PJDchhO7ClJZI_P3zbw.dfvuyqod4KGCve8xM-wotdjimoIXlfb6a4dkaSFN7LI

And make it available on your web server at this URL:

http://t2.m.le.com/.well-known/acme-challenge/KIad2acPdKiF5pLpjlv4fK19PJDchhO7ClJZI_P3zbw

-------------------------------------------------------------------------------
Press Enter to Continue
```

一切正常，验证通过就会签发证书：

```
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/t2.m.le.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/t2.m.le.com/privkey.pem
   Your cert will expire on 2018-03-12. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

## 结语

阿里云申请的免费的赛门铁克的证书有效期是一年，同时赛门铁克证书在以后的chrome版本可能不被信任，
因此换一个证书。这个证书有效期为3个月，到期后操作一遍即可。