---
layout: post
title: "SSL证书问题：不能访问本地发行的证书"
categories: ["技术"]
tags: ["SSL"]
---

* 目录
{:toc .toc}
## Error 描述

cURL error 60: SSL certificate problem: unable to get local issuer certificate (see http://curl.haxx.se/libcurl/c/libcurl-errors.html) [^1] [^2] [^3]



## 环境

| 名称                | 版本                           |
| ------------------- | ------------------------------ |
| 来客PHP在线客服系统 | LK_DIY5.0.8                    |
| PHP                 | PHP-56                         |
| Web服务器           | Apache2.4.48                   |
| DB                  | MySQL5.7.26                    |
| 部署环境            | Windows Server 2019 Datacenter |
| 管理软件            | 宝塔免费版7.3.0                |



## Error 原因

操作系统中的 CA 证书列表过期了。



## 解决方案

1. 下载证书 pem 文件。

2. 修改 宝塔目录/php/php版本/php.ini 文件：

   ```ini
   curl.cainfo = "下载的pem文件路径"
   ```

3. 重新启动 Apache 和 在线客服系统。



在线客服系统中的 CURLOPT_SSL_VERIFYPEER 参数本身就是关闭的。




[^1]: [curl: (60) SSL 证书问题](https://www.ibm.com/mysupport/s/question/0D50z00005q4FheCAE/curl-60-ssl-certificate-problem-unable-to-get-local-issuer-certificate?language=en_US){:target="_blank"}

[^2]: [curl error 60 ssl证书问题，无法获取本地发行的证书](https://laracasts.com/discuss/channels/general-discussion/curl-error-60-ssl-certificate-problem-unable-to-get-local-issuer-certificate?page=2){:target="_blank"}

[^3]: [cURL error 60: SSL证书问题:无法获取本地发行的证书](https://learnku.com/laravel/t/14111/curl-error-60-ssl-certificate-problem-unable-to-get-local-issuer-certificate-see-httpcurlhaxxselibcurlclibcurl-errorshtml){:target="_blank"}