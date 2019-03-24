title: Tomcat9 http/2 configration and client authentication  
date: 2019-03-24 16:13:51 +0800
update: 2019-03-24 17:00:00 +0800
author: me
cover: ""
tags:
  - tomcat
  - http2
  - TLS
preview: Tomcat9下配置HTTP/2和客户端认证
---

> 业务后端管理web端是jsp跑在tomcat上，处于安全需要配置了HTTP/2和客户端认证，以下是配置过程以供参考

## 生成证书

> 系统在windows上，证书生成这里使用了jdk中的keytool工具，linux下可以使用openssl生成

- 生成服务端证书库容器

```bash
keytool -validity 36500 -genkey -v -alias server_test_trust -keyalg EC -keystore server.keystore -dname "CN=192.168.1.10,OU=test,O=test,L=shanghai,ST=shanghai,c=cn" -storepass test -keypass test
```

- 生成客户端证书库容器

```bash
keytool -validity 36500 -genkey -v -alias server_test_trust -keyalg EC -keystore server_trust.keystore -dname "CN=192.168.1.10,OU=test,O=test,L=shanghai,ST=shanghai,c=cn" -storepass test -keypass test
```

- 生成客户端密钥对

```bash
keytool -validity 36500 -genkeypair -v -alias client -keyalg EC -storetype PKCS12 -keystore client.p12 -dname "CN=client,OU=test,O=test,L=shanghai,ST=shanghai,c=cn" -storepass test -keypass test
```

- 导出客户端证书

```bash
keytool -export -v -alias client -keystore client.p12 -storetype PKCS12 -storepass test -rfc -file client.cer
```

- 导入客户端证书到客户端证书库容器

```bash
keytool -import -v -alias client -file client.cer -keystore server_trust.keystore -storepass test
```

## Tomcat9 HTTP/2 配置

- 编辑conf/server.xml文件并加入以下配置项

```xml
    <Connector
           protocol="org.apache.coyote.http11.Http11NioProtocol"
           port="8443" maxThreads="200"
           scheme="https" secure="true" SSLEnabled="true"
           keystoreFile="conf/server.keystore" keystorePass="test" // 服务端证书路径和密码
           truststoreFile="conf/server_trust.keystore" truststorePass="test" // 客户端证书路径和密码
           clientAuth="true" // 开启客户端认证
           sslProtocol="TLS1.2" >
          <UpgradeProtocol className="org.apache.coyote.http2.Http2Protocol" /> // 开启HTTP/2设置
    </Connector>
```

- 编辑conf/web.xml文件末尾加入以下配置可启用强制https

```xml
    <login-config>  
        <!-- Authorization setting for SSL -->  
        <auth-method>CLIENT-CERT</auth-method>  
        <realm-name>Client Cert Users-only Area</realm-name>  
    </login-config>  
    <security-constraint>  
        <!-- Authorization setting for SSL -->  
        <web-resource-collection >  
            <web-resource-name >SSL</web-resource-name>  
            <url-pattern>/*</url-pattern> // 全站TLS
        </web-resource-collection>  
        <user-data-constraint>  
            <transport-guarantee>CONFIDENTIAL</transport-guarantee>  
        </user-data-constraint>  
    </security-constraint>
```

> Note: 客户端访问时先导入第一步生成的client.cer或client.p12，之后用浏览器访问时会要求选择证书.
