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
            - map remote��������Ի���
    - �Զ������ԣ�mitmproxy
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










