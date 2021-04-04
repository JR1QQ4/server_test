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
            - map remote：切换测试环境
    - 自动化测试：mitmproxy [地址](https://docs.mitmproxy.org/dev/overview-installation/)
        - pip install pipx
        - pipx install mitmproxy
        - 运行: 默认监听 8080 `mitmdmp`，修改为 8090 `mitmdump -p 8090`
        - 工具组成
            - mitmproxy 交互的UI工具
            - mitmdump 核心工具
            - mitmweb UI界面工具，类型 Chrome
        - 证书下载: [地址](http://mitm.it)
        - 基本使用
            - 保存录制好的二进制文件 mitmdump -p 8070 -w .\Desktop\tmp
            - 不开启录制并打开文件进行回放 mitmdump -p 8070 -nC .\Desktop\tmp
            - 打开本地文件过滤请求 mitmdump -p 8070 -nr .\Desktop\tmp -w .\Desktop\tmp2 "~s douban"
            - `mitmweb` 分析录制到的内容
            - 运行脚本实现录制 mitmdump -p 8070 -s maplocal.py
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
            - -k 不进行证书认证

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

### socks、socket、websocket

- TCP/IP 四层模型：应用层 -> 传输层 -> 网络层 -> 网络接口层
- TCP/IP 五层模型：应用层 -> 传输层 -> 网络层 -> (数据链路层 -> 物理层)
- OSI 七层模型：(应用层 -> 表示层 -> 会话层) -> 传输层 -> 网络层 -> (数据链路层 -> 物理层)

- socks：在TCP/IP五层模型中和http处于应用层同一层，但是在OSI七层默认中会话层，http在应用层
    - chrome -> http -> socks library -> tcp
- socket：应用层 -> socket抽象层 -> 运输层 -> 网络层 => 链路层，实现了五层协议
- websocket：应用层中的协议

## 性能测试

### 前言

- 性能测试：模拟多个用户的操作对服务器硬件性能的影响
    - TPS（Transaction per Second）每秒事务处理能力
    - RT（Response Time）响应时间
- 工具：
    - Apache ab：Apache HTTP 服务器性能基准工具
    - Apache JMeter：支持多种协议，Java 语言开发
    - LoadRunner：支持很多协议，C 语言开发
    - Locust：有 Web 界面，支持多种协议，Python 开发
        - 安装：pip install locustio
        - 运行：locust
    - nGrinder：Naver 公司基于 Grinder 开发的性能测试平台，能运行 jython、groovy 编写的测试脚本，使用 Java 开发
        - 运行：java ngrinder-controller.war
        - 默认账号密码：admin

### 基本功能

- 创建 JMeter 脚本
    - 方式一：录制新建
    - 方式二：手动创建
- 接口压力测试请求的创建：POST、GET、DELETE...
    - HTTP Header Manager 设置请求头信息，比如 token
- 压力测试请求中的数据传递
    - JSON 提取器
    - XPATH 提取器
- 压力测试中的结果断言校验
    - Response Assertion
    - JSON Assertion
- 利用 Beanshell 生成测试数据
    - Beanshell script 逻辑生成数据
    - Java 代码逻辑生成数据
- 全局变量与 CSV 数据导入
    - User Defined Variables
    - CSV Data Set
- 压测结果数据解读
    - 聚合报告
    - 请求/响应结果树
    - Debug Sampler
- 采样器
    - Debug Sampler，运行时开启
- 定时器
    - Constant Timer，思考时间
- HTTP Request
    - JSON Extractor
        - Names of created variables：access_token，定义全局变量
        - JSON Path expressions：$.access_token，需要匹配的 JSON 中的字段
        - Match No. (0 for Random)： 0，匹配的初始位置
    - JSON Assertion
        - Additionally assert value，是否额外验证根据 JSONPath 提取的值
        - Assert JSON Path exists：$.message，返 回用于断言的字段
        - Expected Value：login success，期望结果 
- 静默压测
    - jmeter -n -t $jmx_file -l $jtl_file，jmx JMeter压测程序脚本文件，jtl JMeter压测请求响应数据的原始文件
    - 自动生成 web 版压测报告
        - jmeter -g $jtl_file -o $web_report_folder
        - jmeter -n -t $jmx_file -l $jtl_file -o $web_report_folder

### 使用

- 线程组：TestPlan->Thread Group
- 监听器 Recording Controller：TestPlan->Thread Group->Add->Logic Controller->Recording Controller
- 查看结果树：TestPlan->Thread Group->Add->Listener->View Results Tree
- 录制：TestPlan->Add->No-Test Elements->HTTP(S) Test Script Recorder

#### 模拟用户

- 请求：TestPlan->Thread Group->Sampler->HTTP Request
- 采样器：TestPlan->Thread Group->Add
- 查看结果树：TestPlan->Thread Group->Add->Listener->View Results Tree
- 测试：
    - 运行服务：python -m http.server 80，监听80端口
    - HTTP Request 中 IP 填写为本机 IP

- Listener View Results Tree
- Listener Aggregate Report（聚合报告）
    - 命令行运行：./jmeter.sh -n -t test_http.jmx test_http.jtl，n不开启图形界面，t指定测试计划，l运行结果
- Backend Listener

#### 分布式压测

- 工作节点（Slave）部署
    - 负载机（Slaves），开启端口 tcp 1099
        - jmeter.properties：
            - 关闭 SSL：server.rmi.ssl.disable=true
        - system.properties：
            - 添加主机：java.rmi.server.hostname=192.168.0.106
    - 运行：jmeter-server
- 控制节点（Master）部署
    - 控制端（Master），开启端口 udp 4445
        - jmeter.properties
            - 添加负载机 IP：remote_hosts=192.168.0.106,192.168.0.107
            - 关闭 SSL：server.rmi.ssl.disable=true
    - 运行：jmeter -> run
- 命令运行远程主机：./jmeter.sh -n -t test_http.jmx test_http.jtl -R 192.168.0.106,192.168.0.107

#### 监控系统

- InfluxDB，开源时序数据库
  - 部署：
    1. Docker 部署 -> docker pull influxdb ( 下载InfluxDB镜像 )
    2. 启动 InfluxDB 容器 -> docker run -d -p 8086:8086  -p 8083:8083 --name=jmeterdb influxdb ( 将容器命名为jmeter_influxdb ) -> docker exec -it jmeterdb bash ( 进入容器内部 )
    3. 在容器内部创建 jmeter 数据库 -> 执行 Influx 命令进入命令台 -> create database jmeter; ( 创建 jmeter 数据库 ) -> show databases; ( 验证结果 )
- JMeter，压测工具
  - 添加 Backend Listener 组件，用于收集数据并发送给 influxdb
    - 在 Backend Listener Implementation 中选择 InfluxdbBackendListenerClient ( jmeter 5.0以上 ) -> 在 InfluxURL 中将实际 Influxdb hostname 填进去 ( http://localhost:8086/write?db=jmeter ) -> 在 application 中填写 order -> 在 testTitle 中填写 Order Testing -> 其余配置保持不变
  - 运行 JMeter，然后在 Influxdb 中校验是否已经能够接受到数据 -> 在 Influxdb 命令中使用查询语句，检查是否已经能够收到数据 ( select * from jmeter; )
- Grafana，度量分析与可视化图标展示工具
  - Dcoker 部署：
    - 下载镜像 ( docker pull grafana/grafana ) -> 启动镜像 ( docker run -d -p 3000:3000 --name=jmeterGraf grafana/grafana )
    - 访问 Grafana 的控制台链接 ( http://localhost:3000/login ) -> 默认用户名/密码：admin/admin
    - 在 Grafana 中添加数据源 -> 选择 Add data source -> 找到 InluxDB 并选择
    - 配置influxDB 数据源 (URL 为 http://localhost:8086，Access 为 browser，Database 为 jmeter)
    - 单击 Sava & Test
    - 在 Grafana 内导入 JMeter Dashboard -> 进入 Home Dashboard 页面选择 Import
    - 打开 iJmeter 项目中的 shell/ jmeter_dashboard.json 文件 -> 将 json 文件粘贴到 paste JSON 中 -> 在 DB name 中选择 InfluxDB -> 单击 Import 按钮完成 Dashboard 导入 -> 单击 Load 导入 
      - ps：模板可以从官网下载
    - 打开刚刚导入的 JMeter Dashboard 查看结果

- influxDB 数据库
    - docker 部署
        - 创建容器网络：docker network create grafana
        - 运行容器：docker run -d --name=influxdb --network grafana -p 8086:8086 -v ${PWD}/influxdb/:/var/lib/influxdb/ inluxdb:1.7.10
            - d 后台运行，name 类似域名，network 指定容器网络，p 指定端口映射，inluxdb:1.7.10 镜像版本信息
            - v 把容器内/var/lib/influxdb/挂在到${PWD}/influxdb/当前目录下的influxdb文件下
        - 创建数据库
            - 方式一：curl -i -XPOST http://localhost:8086/query --data-urlencode "q=CREATE DATABASE jmeter"
            - 方式二：登录influxdb容器 docker exec -it influxdb influx，执行语句 create database jmeter;
        - 简单使用
            - show databases;
            - use jmeter;
            - show measurements;  # 显示数据表，类似 show tables
            - select * from jmeter limit 3;

- Grafana
    - 运行容器：docker run -d --name grafana --network grafana -p 3000:3000 grafana/grafana:6.6.2
    - 默认登录账号：admin，密码：admin
    - 配置数据源：
        - Add data source
            - InfluxDB
                - URL：http://influxdb:8086
                    - 如果没有配置 influxdb name，需要进入到容器查看 IP
                        - docker exec -it influxdb sh
                        - cat /etc/hosts -> 例如 172.18.0.2
                        - 此时 URL：http://172.18.0.2:8086
                - Database：jmeter
                - Min time interval：5s，grafana 读取 influxdb 默认 10s

- JMeter
    - 后端监听器：TestPlan->Thread Group->Add->Listener->Backend Listener
        - 选择 influxdb
        - 修改 influxdbUrl，ip 地址为服务器地址 http://192.168.0.106:8086/write?db=jmeter
        - application 用于确保哪个主机 host106
        - measurement 要与 grafana 设置面板时的 Measurement name 一致

#### 压测脚本

```shell script
#!/usr/bin/env bash

# 压测脚本模板中设定的压测时间为60秒，jmx_template_filename 创建的模板文件
export jmx_template="emenu"
export suffix=".jmx"
export jmx_template_filename="${jmx_template}${suffix}"
export os_type=`uname`

# 需要在系统变量中定义 jmeter 根目录的位置，如下
# export jmeter_path="/your jmeter paht/"

exho "自动化压测全部开始"
# 压测并发列表
thread_number_array=(10 20 30 40 50 60 70)
for num in "${thread_number_array[@]}"
do
  # 生成对应压测线程的 jmx 文件
  export jmx_filename="${jmx_template}_${num}${suffix}"
  export jtl_filename="test_${num}.jtl"
  export web_report_path_name="web_${num}"

  rm -f ${jmx_filename} ${jtl_filename}
  rm -rf ${web_report_path_name}

  cp ${jmx_template_filename} ${jmx_filename}
  echo "生成 jmx 压测脚本 ${jmx_filename}"
  
  # thrad_num 是模板文件中表示并发数的变量，即 Number od Threads(users)
  if [[ "${os_type}" == "Darwin" ]]]; then
    sed -i "" "s/thrad_num/${num}/g" ${jmx_filename}
  else
    sed -i "s/thread_num/${num}/g" ${jmx_filename}

  # JMeter 静默压测
  ${jmeter_path}/bin/jmeter -n -t ${jmx_filename} -l ${jtl_filename}
  
  # 生成 web 压测报告
  ${jmeter_path}/bin/jmeter -g ${jmx_filename} -e -o ${web_report_path_name}
done
echo "自动化压测全部结束"
```
