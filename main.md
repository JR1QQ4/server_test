# ����˲���

## �ӿڲ���

### Э���������

- ���������TcpDump + WireShark
    - ���磬ץȡ���ʰٶȵ����ݰ�
        - sudo tcpdump host www.baidu.com -w /tmp/tcpdump.log
        - curl http://www.baidu.com
        - ֹͣ tcpdump
        - ʹ�� wireshark �� /tmp/tcpdump.log ����
- ���� Proxy
    - �Ƽ����ߣ��ֹ�����charles��ȫƽ̨������ȫ����burpsuite��ȫƽ̨ java��
        - charles
            - rewrite����mock
            - map local������mock
            - map remote���л����Ի���
    - �Զ������ԣ�mitmproxy [��ַ](https://docs.mitmproxy.org/dev/overview-installation/)
        - pip install pipx
        - pipx install mitmproxy
        - ����: Ĭ�ϼ��� 8080 `mitmdmp`���޸�Ϊ 8090 `mitmdump -p 8090`
        - �������
            - mitmproxy ������UI����
            - mitmdump ���Ĺ���
            - mitmweb UI���湤�ߣ����� Chrome
        - ֤������: [��ַ](http://mitm.it)
        - ����ʹ��
            - ����¼�ƺõĶ������ļ� mitmdump -p 8070 -w .\Desktop\tmp
            - ������¼�Ʋ����ļ����лط� mitmdump -p 8070 -nC .\Desktop\tmp
            - �򿪱����ļ��������� mitmdump -p 8070 -nr .\Desktop\tmp -w .\Desktop\tmp2 "~s douban"
            - `mitmweb` ����¼�Ƶ�������
            - ���нű�ʵ��¼�� mitmdump -p 8070 -s maplocal.py
    - ��������fiddler����Windows����AnProxy��ȫƽ̨��
    - �����ܴ����������squid��dante
    - �������nginx
    - ����ת���븴�ƣ�em-proxy��gor��iptable��nginx
    - socks5����ssh -d����
- Э��ͻ��˹��ߣ�curl��postman
    - curl
        - url=http://www.baidu.com
        - get���� curl $url
        - post���� curl -d 'xxx' $url
        - proxyʹ�� curl -x "http://127.0.0.1:8080" $url
        - ��Ҫ����
            - -H "Content-Type: application/json" ��Ϣͷ����
            - -u username:password �û���֤
            - -d Ҫ���͵�post���� @file ��ʾ�����ļ�
            - --data-urlencode 'page_size=50' �����ݽ���url����
            - -G ��data���ݵ���get����Ĳ������ͣ�����--data-urlencode���ʹ��
            - -o д�ļ�
            - -x ���� http���� socks5����
            - -v verbose ��ӡ����ϸ��־ -s �ر�һЩ��ʾ���
            - -k ������֤����֤

```shell script
url=http://www.baidu.com

## get�����json���� 
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
      "name": "�ѹ�",
      "enName": "",
      "hasexist": "false",
      "flag": null,
      "type": 0,
      "stock_id": 1029472,
      "ind_id": 0,
      "ind_name": "ͨѶҵ��",
      "ind_color": null,
      "_source": "sc_1:1:sogo"
    }
  ]
}

#post����
curl 'http://sonarqube.testing-studio.com:9000/api/authentication/login' -H 'Connection: keep-alive' -H 'Pragma: no-cache' -H 'Cache-Control: no-cache' -H 'Accept: application/json' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36' -H 'Content-Type: application/x-www-form-urlencoded' -H 'Origin: http://sonarqube.testing-studio.com:9000' -H 'Referer: http://sonarqube.testing-studio.com:9000/sessions/new' -H 'Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7' -H 'Cookie: _ga=GA1.2.232181868.1566982077; experimentation_subject_id=IjNlYzgxODQ1LTU2MDAtNGIyNy1iNTgzLTE1MzRkY2IwMDI0ZSI%3D--b1f29d33f6a2c85a81be66e4774d437f710c102f; _gid=GA1.2.482544306.1585051015' --data 'login=admin&password=1234' --compressed --insecure

#�ٶȵ�һ��url�ύ�ű�
curl -H 'Content-Type:text/plain' --data-binary @urls.txt "http://data.zz.baidu.com/urls"

#�Բ������벢����get����
    curl -G $url  \
        --data-urlencode "current=$current" \
        --data-urlencode "pageSize=$pageSize" 

#��֤��put�ϴ���ElasticSearch��
    curl -X PUT "$ES_HOST/$index/_doc/$id?pretty" \
        --user username:password \
        -H 'Content-Type: application/json' \
        -d "$content"

#�鿴����
curl -s --user $mail_username:$mail_password "imaps://imap.exmail.qq.com/inbox?all"
```

### get��post������

1. http��method�ֶβ�ͬ
2. post���Ը���body������֧��form��json��xml��binary�ȸ������ݸ�ʽ
    - Content-Length �� Content-Type
- ��ҵͨ�õĹ淶
    - ��״̬�仯�Ľ���ʹ��get����
    - ���ݵ�д����״̬�޸Ľ�����post

### Session��Cookie��Token

- Cookie��
    - Cookie �����ڿͻ���
    - ÿ�����󶼻�Я�� Cookie ���͸����������洢�������ݻ��˷Ѵ���
    - ��������ܷ�������Set-Cookieָ�����cookie���浽�����ϣ�ÿ����վ�����cookieֻ�������Լ�����վ
- Session��
    - ���ݴ洢������ˣ�ֻ�ѹ������ݵ�һ�����ܴ��ŵ� Cookie �б��
    - ���Ի��� cookie��Ҳ���Ի��� query�����ڹ����û��������
- Token
    - Ӧ�ó�����OAuth��֤����ҵ΢�š�github�������֤
        - ƾ����֤��Ϣ��ȡtoken������ͨ����̨���ú�token
        - �����������ʹ��token����������query��������ʽ�ṩ
    - һ���û�����ʱ�����������ֶΣ�������֤�����Ȩ��
- ���Ӧ�õ�ʱ�򣬱��� android ԭ��ϵͳ��֧�� cookie
    - ��Ҫ�� token ʶ���û�
    - ��Ҫ�ð� sessionId ���浽 http �����е� header ���� query �ֶ���

### socks��socket��websocket

- TCP/IP �Ĳ�ģ�ͣ�Ӧ�ò� -> ����� -> ����� -> ����ӿڲ�
- TCP/IP ���ģ�ͣ�Ӧ�ò� -> ����� -> ����� -> (������·�� -> �����)
- OSI �߲�ģ�ͣ�(Ӧ�ò� -> ��ʾ�� -> �Ự��) -> ����� -> ����� -> (������·�� -> �����)

- socks����TCP/IP���ģ���к�http����Ӧ�ò�ͬһ�㣬������OSI�߲�Ĭ���лỰ�㣬http��Ӧ�ò�
    - chrome -> http -> socks library -> tcp
- socket��Ӧ�ò� -> socket����� -> ����� -> ����� => ��·�㣬ʵ�������Э��
- websocket��Ӧ�ò��е�Э��

## ���ܲ���

### ǰ��

- ���ܲ��ԣ�ģ�����û��Ĳ����Է�����Ӳ�����ܵ�Ӱ��
    - TPS��Transaction per Second��ÿ������������
    - RT��Response Time����Ӧʱ��
- ���ߣ�
    - Apache ab��Apache HTTP ���������ܻ�׼����
    - Apache JMeter��֧�ֶ���Э�飬Java ���Կ���
    - LoadRunner��֧�ֺܶ�Э�飬C ���Կ���
    - Locust���� Web ���棬֧�ֶ���Э�飬Python ����
        - ��װ��pip install locustio
        - ���У�locust
    - nGrinder��Naver ��˾���� Grinder ���������ܲ���ƽ̨�������� jython��groovy ��д�Ĳ��Խű���ʹ�� Java ����
        - ���У�java ngrinder-controller.war
        - Ĭ���˺����룺admin

### ��������

- ���� JMeter �ű�
    - ��ʽһ��¼���½�
    - ��ʽ�����ֶ�����
- �ӿ�ѹ����������Ĵ�����POST��GET��DELETE...
    - HTTP Header Manager ��������ͷ��Ϣ������ token
- ѹ�����������е����ݴ���
    - JSON ��ȡ��
    - XPATH ��ȡ��
- ѹ�������еĽ������У��
    - Response Assertion
    - JSON Assertion
- ���� Beanshell ���ɲ�������
    - Beanshell script �߼���������
    - Java �����߼���������
- ȫ�ֱ����� CSV ���ݵ���
    - User Defined Variables
    - CSV Data Set
- ѹ�������ݽ��
    - �ۺϱ���
    - ����/��Ӧ�����
    - Debug Sampler
- ������
    - Debug Sampler������ʱ����
- ��ʱ��
    - Constant Timer��˼��ʱ��
- HTTP Request
    - JSON Extractor
        - Names of created variables��access_token������ȫ�ֱ���
        - JSON Path expressions��$.access_token����Ҫƥ��� JSON �е��ֶ�
        - Match No. (0 for Random)�� 0��ƥ��ĳ�ʼλ��
    - JSON Assertion
        - Additionally assert value���Ƿ������֤���� JSONPath ��ȡ��ֵ
        - Assert JSON Path exists��$.message���� �����ڶ��Ե��ֶ�
        - Expected Value��login success��������� 
- ��Ĭѹ��
    - jmeter -n -t $jmx_file -l $jtl_file��jmx JMeterѹ�����ű��ļ���jtl JMeterѹ��������Ӧ���ݵ�ԭʼ�ļ�
    - �Զ����� web ��ѹ�ⱨ��
        - jmeter -g $jtl_file -o $web_report_folder
        - jmeter -n -t $jmx_file -l $jtl_file -o $web_report_folder

### ʹ��

- �߳��飺TestPlan->Thread Group
- ������ Recording Controller��TestPlan->Thread Group->Add->Logic Controller->Recording Controller
- �鿴�������TestPlan->Thread Group->Add->Listener->View Results Tree
- ¼�ƣ�TestPlan->Add->No-Test Elements->HTTP(S) Test Script Recorder

#### ģ���û�

- ����TestPlan->Thread Group->Sampler->HTTP Request
- ��������TestPlan->Thread Group->Add
- �鿴�������TestPlan->Thread Group->Add->Listener->View Results Tree
- ���ԣ�
    - ���з���python -m http.server 80������80�˿�
    - HTTP Request �� IP ��дΪ���� IP

- Listener View Results Tree
- Listener Aggregate Report���ۺϱ��棩
    - ���������У�./jmeter.sh -n -t test_http.jmx test_http.jtl��n������ͼ�ν��棬tָ�����Լƻ���l���н��
- Backend Listener

#### �ֲ�ʽѹ��

- �����ڵ㣨Slave������
    - ���ػ���Slaves���������˿� tcp 1099
        - jmeter.properties��
            - �ر� SSL��server.rmi.ssl.disable=true
        - system.properties��
            - ���������java.rmi.server.hostname=192.168.0.106
    - ���У�jmeter-server
- ���ƽڵ㣨Master������
    - ���ƶˣ�Master���������˿� udp 4445
        - jmeter.properties
            - ��Ӹ��ػ� IP��remote_hosts=192.168.0.106,192.168.0.107
            - �ر� SSL��server.rmi.ssl.disable=true
    - ���У�jmeter -> run
- ��������Զ��������./jmeter.sh -n -t test_http.jmx test_http.jtl -R 192.168.0.106,192.168.0.107

#### ���ϵͳ

- InfluxDB����Դʱ�����ݿ�
  - ����
    1. Docker ���� -> docker pull influxdb ( ����InfluxDB���� )
    2. ���� InfluxDB ���� -> docker run -d -p 8086:8086  -p 8083:8083 --name=jmeterdb influxdb ( ����������Ϊjmeter_influxdb ) -> docker exec -it jmeterdb bash ( ���������ڲ� )
    3. �������ڲ����� jmeter ���ݿ� -> ִ�� Influx �����������̨ -> create database jmeter; ( ���� jmeter ���ݿ� ) -> show databases; ( ��֤��� )
- JMeter��ѹ�⹤��
  - ��� Backend Listener ����������ռ����ݲ����͸� influxdb
    - �� Backend Listener Implementation ��ѡ�� InfluxdbBackendListenerClient ( jmeter 5.0���� ) -> �� InfluxURL �н�ʵ�� Influxdb hostname ���ȥ ( http://localhost:8086/write?db=jmeter ) -> �� application ����д order -> �� testTitle ����д Order Testing -> �������ñ��ֲ���
  - ���� JMeter��Ȼ���� Influxdb ��У���Ƿ��Ѿ��ܹ����ܵ����� -> �� Influxdb ������ʹ�ò�ѯ��䣬����Ƿ��Ѿ��ܹ��յ����� ( select * from jmeter; )
- Grafana��������������ӻ�ͼ��չʾ����
  - Dcoker ����
    - ���ؾ��� ( docker pull grafana/grafana ) -> �������� ( docker run -d -p 3000:3000 --name=jmeterGraf grafana/grafana )
    - ���� Grafana �Ŀ���̨���� ( http://localhost:3000/login ) -> Ĭ���û���/���룺admin/admin
    - �� Grafana ���������Դ -> ѡ�� Add data source -> �ҵ� InluxDB ��ѡ��
    - ����influxDB ����Դ (URL Ϊ http://localhost:8086��Access Ϊ browser��Database Ϊ jmeter)
    - ���� Sava & Test
    - �� Grafana �ڵ��� JMeter Dashboard -> ���� Home Dashboard ҳ��ѡ�� Import
    - �� iJmeter ��Ŀ�е� shell/ jmeter_dashboard.json �ļ� -> �� json �ļ�ճ���� paste JSON �� -> �� DB name ��ѡ�� InfluxDB -> ���� Import ��ť��� Dashboard ���� -> ���� Load ���� 
      - ps��ģ����Դӹ�������
    - �򿪸ոյ���� JMeter Dashboard �鿴���

- influxDB ���ݿ�
    - docker ����
        - �����������磺docker network create grafana
        - ����������docker run -d --name=influxdb --network grafana -p 8086:8086 -v ${PWD}/influxdb/:/var/lib/influxdb/ inluxdb:1.7.10
            - d ��̨���У�name ����������network ָ���������磬p ָ���˿�ӳ�䣬inluxdb:1.7.10 ����汾��Ϣ
            - v ��������/var/lib/influxdb/���ڵ�${PWD}/influxdb/��ǰĿ¼�µ�influxdb�ļ���
        - �������ݿ�
            - ��ʽһ��curl -i -XPOST http://localhost:8086/query --data-urlencode "q=CREATE DATABASE jmeter"
            - ��ʽ������¼influxdb���� docker exec -it influxdb influx��ִ����� create database jmeter;
        - ��ʹ��
            - show databases;
            - use jmeter;
            - show measurements;  # ��ʾ���ݱ����� show tables
            - select * from jmeter limit 3;

- Grafana
    - ����������docker run -d --name grafana --network grafana -p 3000:3000 grafana/grafana:6.6.2
    - Ĭ�ϵ�¼�˺ţ�admin�����룺admin
    - ��������Դ��
        - Add data source
            - InfluxDB
                - URL��http://influxdb:8086
                    - ���û������ influxdb name����Ҫ���뵽�����鿴 IP
                        - docker exec -it influxdb sh
                        - cat /etc/hosts -> ���� 172.18.0.2
                        - ��ʱ URL��http://172.18.0.2:8086
                - Database��jmeter
                - Min time interval��5s��grafana ��ȡ influxdb Ĭ�� 10s

- JMeter
    - ��˼�������TestPlan->Thread Group->Add->Listener->Backend Listener
        - ѡ�� influxdb
        - �޸� influxdbUrl��ip ��ַΪ��������ַ http://192.168.0.106:8086/write?db=jmeter
        - application ����ȷ���ĸ����� host106
        - measurement Ҫ�� grafana �������ʱ�� Measurement name һ��

#### ѹ��ű�

```shell script
#!/usr/bin/env bash

# ѹ��ű�ģ�����趨��ѹ��ʱ��Ϊ60�룬jmx_template_filename ������ģ���ļ�
export jmx_template="emenu"
export suffix=".jmx"
export jmx_template_filename="${jmx_template}${suffix}"
export os_type=`uname`

# ��Ҫ��ϵͳ�����ж��� jmeter ��Ŀ¼��λ�ã�����
# export jmeter_path="/your jmeter paht/"

exho "�Զ���ѹ��ȫ����ʼ"
# ѹ�Ⲣ���б�
thread_number_array=(10 20 30 40 50 60 70)
for num in "${thread_number_array[@]}"
do
  # ���ɶ�Ӧѹ���̵߳� jmx �ļ�
  export jmx_filename="${jmx_template}_${num}${suffix}"
  export jtl_filename="test_${num}.jtl"
  export web_report_path_name="web_${num}"

  rm -f ${jmx_filename} ${jtl_filename}
  rm -rf ${web_report_path_name}

  cp ${jmx_template_filename} ${jmx_filename}
  echo "���� jmx ѹ��ű� ${jmx_filename}"
  
  # thrad_num ��ģ���ļ��б�ʾ�������ı������� Number od Threads(users)
  if [[ "${os_type}" == "Darwin" ]]]; then
    sed -i "" "s/thrad_num/${num}/g" ${jmx_filename}
  else
    sed -i "s/thread_num/${num}/g" ${jmx_filename}

  # JMeter ��Ĭѹ��
  ${jmeter_path}/bin/jmeter -n -t ${jmx_filename} -l ${jtl_filename}
  
  # ���� web ѹ�ⱨ��
  ${jmeter_path}/bin/jmeter -g ${jmx_filename} -e -o ${web_report_path_name}
done
echo "�Զ���ѹ��ȫ������"
```
