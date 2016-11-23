# PHP调用认知服务证书认证问题 #

**问题：**

使用 PHP 的 http 客户端工具(如 Guzzle)调用认知服务时出现证书认证问题

**现象：**

    Fatal error: Uncaught exception 'GuzzleHttp\Exception\RequestException' with message 'cURL error 60: SSL certificate problem: unable to get local issuer certificate

**解决方法：**

1.	下载证书保存到本地，下载地址：[https://curl.haxx.se/ca/cacert.pem](https://curl.haxx.se/ca/cacert.pem)
2.	配置 php.ini 文件：`curl.cainfo =<filepath>/cacert.pem`
3.	重启 Apache 服务器，问题解决。
