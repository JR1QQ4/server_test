# WEB安全

## 渗透测试

渗透测试步骤：
- 获取纸面授权
- 信息收集
- 挖掘利用
- 整理报告

编码：
- URL 编码：一种浏览器用来打包表单输入的格式，格式为 `%号+十六进制`
- Base64 编码：网络上常见的用于传输 8Bit 字节代码的编码方式之一，格式为 `大小写字母数字/大小写字母+=号`
- HTML 实体化编码：在 HTML 中使用小于号、大于号等会被误认为是标签，格式为 `&开头/&#开头，;结尾`

加密：
- MD5 加密（消息摘要算法第五版）：默认长度为 16 和 32 位，可以包含 abcdef 和数字 0-9；字典撞密

工具：
- namp
- sqlmap
- BurpSuite

常见漏洞：
- 弱口令：BurpSuite
- 类别一
    - HTML 注入
    - XSS 跨站
    - CSRF 客户端请求伪造
    - SSRF 服务端请求伪造
- 类别二
    - SQL 注入
        - 数据类型之数字型
            - And 测试： `And 1=1` `And 1=2`
            - Or 测试： `Or 1=1` `Or 1=2`
            - Xor 测试： `Xor 1=1` `Xor 1=2`
            - Like 测试： `Like 1=1` `Like 1=2`
            - 符号测试： 单引号、减号
        - 数据类型之字符型
        - 返回结果-显错注入
        - 返回结果（无返回结果）-盲注
    - 逻辑漏洞
        - 两个重点：业务流程和抓包改包
        - 密码找回、支付漏洞、越权、Cookies和session验证问题、顺序执行缺陷
        - 创建逻辑漏洞类型：密码重置、任意使用、越权操作
            - 各种回写
            - 各种可逆：撞库危害、Cookie 可逆
            - 逻辑顺序
    - 越权漏洞
    - 未授权访问
- 类别三
    - 文件读取
    - 文件上传
    - 文件包含
    - 文件下载
- 类别三
    - 代码注入
    - 命令执行
    - 威胁情报

绕过查杀规则：
- 常见查杀规则：静态文本查杀、动态执行查杀
- 1.文本查杀绕过：大小写转换、文本颠倒、文本分割、干扰函数、加密、语言特征
- 2.动态执行查杀逃过：加密传输、改变传输特征...
- 方式：WebShell原理 + ByPass安全狗 + Cknife

NTFS文件流：
- 1.打开cmd，`$ echo matlab>>abc.txt:test.txt`，会生成一个 abc.txt 的文件，但文件的大小为 0 字节，点击打开后无任何内容
- 2.进入cmd，`$ notepad abc.txt:test.txt` 打开可以查看编辑里面的内容
    - 这里的 `abc.txt` 可以不存在，也可以是某个已存在的文件，文件格式无所谓，无论是.txt还是.jpg|.exe|.asp都行
    - 这里的 `test.txt` 代表一个文件流
- 3.包含隐藏信息的文件任然可以继续隐藏其他的内容
    - `echo matlab>>abc.txt:test.txt` 会给 abc.txt 建立一个隐藏信息的流文件，cmd 中使用 notepad 可查看
    - `echo matlab1>>abc.txt:test1.txt` 会给 abc.txt 建立新的隐藏信息的流文件，notepad 打开显示 matlab1
- 其中，abc.txt 称为 `宿主`，test.txt 称为 `寄生`，可以使用 FileStream 等工具查看

分析第三方危害：
- 外部 JS \ 外部 CSS \ 域名商 \ 服务器商
- 同源策略：不同协议、域名（子域名、顶级域名）、端口的客户端脚本在未经授权的情况下不能读写对方的资源

条件竞争漏洞：
- 一种服务端的漏洞，由于服务器端在处理不同用户的请求时是并发进行的，因此，如果并发处理不当或相关操作逻辑顺序设计的不合理时，将会导致此类问题的发生





