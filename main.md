# 服务端测试

## 接口测试

### 协议分析工具

- 网络监听：TcpDump + WireShark
    - 例如，抓取访问百度的数据包
        - sudo tcpdump host www.baidu.com -w /tmp/tcpdump.log
        - curl http://www.baidu.com
        - 停止 tcpdump
        - 使用 wireshark 打开 /tmp/tcpdump.log 分析
- 代理 Proxy
    - 推荐工具：手工测试charles（全平台）、安全测试burpsuite（全平台 java）
        - charles
            - rewrite：简单mock
            - map local：复杂mock
            - map remote：整体测试环境
    - 自动化测试：mitmproxy
    - 其他代理：fiddler（仅Windows）、AnProxy（全平台）
    - 高性能代理服务器：squid、dante
    - 反响代理：nginx
    - 流量转发与复制：em-proxy、gor、iptable、nginx
    - socks5代理：ssh -d参数
- 协议客户端工具：curl、postman
    - curl
        - url=http://www.baidu.com
        - get请求 curl $url
        - post请求 curl -d 'xxx' $url
        - proxy使用 curl -x "http://127.0.0.1:8080" $url
        - 重要参数
            - -H "Content-Type: application/json" 消息头设置
            - -u username:password 用户认证
            - -d 要发送的post数据 @file 表示来自文件
            - --data-urlencode 'page_size=50' 对内容进行url编码
            - -G 把data数据当成get请求的参数发送，常与--data-urlencode结合使用
            - -o 写文件
            - -x 代理 http代理 socks5代理
            - -v verbose 打印更详细日志 -s 关闭一些提示输出

```shell script
url=http://www.baidu.com

## get请求加json解析 
curl -s 'https://xueqiu.com/stock/search.json?code=sogo&size=3&page=1' -H 'Connection: keep-alive' -H 'Pragma: no-cache'
 -H 'Cache-Control: no-cache' -H 'Accept: application/json, text/plain, */*' -H 'Sec-Fetch-Dest: empty' 
-H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36' 
-H 'elastic-apm-traceparent: 00-760301b0a132e9a4c0f5ac7448a3419e-8823be75504fc61f-00' -H 'Sec-Fetch-Site: same-origin' 
-H 'Sec-Fetch-Mode: cors' -H 'Referer: https://xueqiu.com/k?q=sogo' -H 'Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7' 
-H 'Cookie: device_id=24700f9f1986800ab4fcc880530dd0ed; cookiesu=841584103115161; aliyungf_tc=AQAAAIPytE8aVQoAXhjf3cw3R+j5DD/s; acw_tc=2760824b15851452106833674e25941ad47588d5d7ded79b38a04dad8f9444; xq_a_token=2ee68b782d6ac072e2a24d81406dd950aacaebe3; xqat=2ee68b782d6ac072e2a24d81406dd950aacaebe3; xq_r_token=f9a2c4e43ce1340d624c8b28e3634941c48f1052; xq_id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJ1aWQiOi0xLCJpc3MiOiJ1YyIsImV4cCI6MTU4NzUyMjY2MSwiY3RtIjoxNTg1MTQ1MTYxMDIwLCJjaWQiOiJkOWQwbjRBWnVwIn0.TPrw6_M2Th9QTVz5spwUybqN1790nJANu9kxXl4GfNb1eQ2p2zD43CStgogOGQ8yRXYmSCfURp0343wgjnnCdnQX5698Jl-brdP94wiYKwv11q8QjBYMXFWJGRj0g69C2nxVrRF8K-ETGEked3KjYfk8Xy2wPuZtyGUhORWeCvMhmBdcRKIlWj4d7wp-w_LjMbSLigJAT29F03wBZIxR0r3eMNUhUsXh8dCsWNb6wzhtg8dT4gcd91mQmR5ToR_SFrzQfOopY4vQGcaOHWaAwUMPLUopZwD4ajWzm1kpoBZnf_n_9uBfT4j0nGk95E8J8EmTfBlq-1p019xkhgp87w; u=431585145210698; Hm_lvt_1db88642e346389874251b5a1eded6e3=1583285031,1584102200,1585145180; Hm_lpvt_1db88642e346389874251b5a1eded6e3=1585145192' 
--compressed | jq
{
  "q": "sogo",
  "page": 1,
  "size": 3,
  "stocks": [
    {
      "code": "SOGO",
      "name": "搜狗",
      "enName": "",
      "hasexist": "false",
      "flag": null,
      "type": 0,
      "stock_id": 1029472,
      "ind_id": 0,
      "ind_name": "通讯业务",
      "ind_color": null,
      "_source": "sc_1:1:sogo"
    }
  ]
}

#post请求
curl 'http://sonarqube.testing-studio.com:9000/api/authentication/login' -H 'Connection: keep-alive' -H 'Pragma: no-cache' -H 'Cache-Control: no-cache' -H 'Accept: application/json' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36' -H 'Content-Type: application/x-www-form-urlencoded' -H 'Origin: http://sonarqube.testing-studio.com:9000' -H 'Referer: http://sonarqube.testing-studio.com:9000/sessions/new' -H 'Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7' -H 'Cookie: _ga=GA1.2.232181868.1566982077; experimentation_subject_id=IjNlYzgxODQ1LTU2MDAtNGIyNy1iNTgzLTE1MzRkY2IwMDI0ZSI%3D--b1f29d33f6a2c85a81be66e4774d437f710c102f; _gid=GA1.2.482544306.1585051015' --data 'login=admin&password=1234' --compressed --insecure

#百度的一个url提交脚本
curl -H 'Content-Type:text/plain' --data-binary @urls.txt "http://data.zz.baidu.com/urls"

#对参数编码并发送get请求
    curl -G $url  \
        --data-urlencode "current=$current" \
        --data-urlencode "pageSize=$pageSize" 

#认证与put上传都ElasticSearch里
    curl -X PUT "$ES_HOST/$index/_doc/$id?pretty" \
        --user username:password \
        -H 'Content-Type: application/json' \
        -d "$content"

#查看邮箱
curl -s --user $mail_username:$mail_password "imaps://imap.exmail.qq.com/inbox?all"
```

### get与post的区别

1. http的method字段不同
2. post可以附加body，可以支持form、json、xml、binary等各种数据格式
    - Content-Length 与 Content-Type
- 行业通用的规范
    - 无状态变化的建议使用get请求
    - 数据的写入与状态修改建议用post

### Session、Cookie、Token

- Cookie：
    - Cookie 保存在客户端
    - 每次请求都会携带 Cookie 发送给服务器，存储大量数据会浪费带宽
    - 浏览器接受服务器的Set-Cookie指令，并把cookie保存到电脑上，每个网站保存的cookie只作用于自己的网站
- Session：
    - 数据存储到服务端，只把关联数据的一个加密串放到 Cookie 中标记
    - 可以基于 cookie，也可以基于 query，用于关联用户相关数据
- Token
    - 应用场景：OAuth认证、企业微信、github等相关认证
        - 凭借认证信息获取token，或者通过后台配置好token
        - 在相关请求中使用token，多数是以query参数的形式提供
    - 一个用户请求时附带的请求字段，用于验证身份与权限
- 跨端应用的时候，比如 android 原生系统不支持 cookie
    - 需要用 token 识别用户
    - 需要用把 sessionId 保存到 http 请求中的 header 或者 query 字段中










