# RFC 7540 Translation (in progress)

[HTTP/2 Standards](https://tools.ietf.org/html/rfc7540), 欢迎参与翻译！
**新来的同学请先移步至 issue**: https://github.com/yuuoniy/rfc7540-translation-zh_cn/issues/1

+ 请遵循"fork & pull request"协作模型，不要直接 commit 到原始仓库。
+ 发起 pull request 之前请 fetch/pull 检查与 upstream 分支的差异获取最近更新。
（更新时间 2019.4.1)

翻译进度：
+ [x] 1. [介绍](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/1-zh-cn.md)
+ [x] 2. [概览](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/2-zh-cn.md)
  - [x] 2.1. [文档结构](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/2-zh-cn.md#21-文档结构)
  - [x] 2.2. [约定与术语](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/2-zh-cn.md#22-约定与术语)
+ [x] 3. [开始 HTTP/2](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/3-zh-cn.md)
  - [x] 3.1. [HTTP/2 版本标识](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/3-zh-cn.md#31-http2-version-identification--http2版本标识)
  - [x] 3.2. [为"http" URIs 启用 HTTP/2 协议](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/3-zh-cn.md#32-starting-http2-for-http-uris--为http-uris启用http2协议)
  - [x] 3.2.1. [HTTP2-Settings 头部字段](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/3-zh-cn.md#321-http2-settings-header-field--http2-settings首部字段)
  - [x] 3.3. [为"https" URIs 启用 HTTP/2 协议](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/3-zh-cn.md#33-starting-http2-for-https-uris--为https-uris启用http2协议)
  - [x] 3.4. [先验下启用 HTTP/2](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/3-zh-cn.md#34-starting-http2-with-prior-knowledge--先验下启用http2)
  - [x] 3.5. [HTTP/2 连接序言](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/3-zh-cn.md#35-http2-connection-preface--http2连接前奏)
+ [x] 4. [HTTP 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/4-zh-cn.md)
  - [x] 4.1. [帧格式](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/4-zh-cn.md#41-帧格式)
  - [x] 4.2. [帧大小](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/4-zh-cn.md#42-帧大小)
  - [x] 4.3. [Header 压缩和解压](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/4-zh-cn.md#43-header压缩和解压)
+ [x] 5.  [流与多路复用](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md)
  - [x] 5.1. [流的状态](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#51-流的状态)
  - [x] 5.1.1. [流标识符](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#511-stream标识符)
  - [x] 5.1.2 [流的并发性](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#512-流的并发性)
  - [x] 5.2 [流控](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#52-流控)
  - [x] 5.2.1 [流控原则](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#521-流控原则)
  - [x] 5.2.2 [合理使用流控](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#522-合理使用流控)
  - [x] 5.3 [流的优先级](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#53-流的优先级)
  - [x] 5.3.1 [流的依赖关系](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#531-流的依赖关系)
  - [x] 5.3.2 [依赖权重](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#532-依赖权重)
  - [x] 5.3.3 [优先级重排](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#533-优先级依赖重排)
  - [x] 5.3.4 [优先级状态管理](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#534-优先级状态管理)
  - [x] 5.3.5 [默认优先级](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#535-默认优先级)
  - [x] 5.4 [错误处理](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#54-错误处理)
  - [x] 5.4.1 [连接错误](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#541-连接错误处理)
  - [x] 5.4.2 [流错误](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#542-流错误处理)
  - [x] 5.4.3 [连接终止](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#543-连接终止)
  - [x] 5.5 [HTTP/2 扩展](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/5-zh-cn.md#55-http2扩展)
+ [x] 6. [帧的定义](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md)
  - [x] 6.1 [DATA 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#61-data)
  - [x] 6.2 [HEADERS 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#62-headers)
  - [x] 6. [PRIORITY 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#63-priority)
  - [x] 6.4 [RST_STREAM 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#64-rst_stream)
  - [x] 6.5 [SETTINGS 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#65-settings)
  - [x] 6.5.1 [SETTINGS 帧格式](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#651-settings格式)
  - [x] 6.5.2 [已定义的 SETTINGS 参数](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#652-已定义的settings参数)
  - [x] 6.5.3 [设置同步](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#653-设置同步)
  - [x] 6.6 [PUSH_PROMISE 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#66-push_promise)
  - [x] 6.7 [PING 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#67-ping)
  - [x] 6.8 [GOAWAY 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#68-goaway)
  - [x] 6.9 [WINDOW_UPDATE 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#69-window_update)
  - [x] 6.9.1 [流控窗口](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#691-flow-control窗口)
  - [x] 6.9.2 [初始的 Flow-Control 窗口大小](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#692-初始的flow-control窗口大小)
  - [x] 6.9.3 [减小流窗口大小](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#693-减小流窗口大小)
  - [x] 6.10 [CONTINUATION 帧](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/6-zh-cn.md#610-continuation)
+ [x] 7. [错误码](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/7-zh-cn.md)
+ [x] 8. [HTTP 消息交换](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md)
   - [x] 8.1 [HTTP 请求/响应交换](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#81-http-requestresponse-exchange--http-请求响应交换)
   - [x] 8.1.1 [从 HTTP/2 升级](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#811-upgrading-from-http2--从http2升级)
   - [x] 8.1.2 [HTTP 头部字段](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#812-http-header-fields--http头部字段)
   - [x] 8.1.2.1 [伪头部字段](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#8121-pseudo-header-fields-伪头部字段)
   - [x] 8.1.2.2 [针对连接的头部字段](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#8122-connection-specific-header-fields--connection-specific头部字段)
   - [x] 8.1.2.3 [请求伪头部字段](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#8123-request-pseudo-header-fields--请求伪头部字段)
   - [x] 8.1.2.4 [响应伪头部字段](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#8124-response-pseudo-header-fields--响应伪头部字段)
   - [x] 8.1.2.5 [压缩 Cookie 头部字段](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#8125-compressing-the-cookie-header-field--压缩cookie头部字段)
   - [x] 8.1.2.6 [不规范的请求和响应](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#8126-malformed-requests-and-responses--不规范的请求和响应)
   - [x] 8.1.3 [例子](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#813-examples--例子)
   - [x] 8.1.4 [HTTP/2 请求可靠机制](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#814-request-reliability-mechanisms-in-http2--http2请求可靠机制)
   - [x] 8.2 [服务端推送](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#82-server-push--服务端推送)
   - [x] 8.2.1 [推送请求](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#821-push-requests--推送请求)
   - [x] 8.2.2 [推送响应](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#822-push-responses--推送响应)
   - [x] 8.3 [CONNECT 方法](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/8-zh-cn.md#83-the-connect-method--connect方法)
+ [x] 9. [其他 HTTP 要求/注意事项](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/9-zh-cn.md)
   - [x] 9.1 [连接管理](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/9-zh-cn.md#91-连接管理)
   - [x] 9.1.1 [连接复用](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/9-zh-cn.md#911-连接复用)
   - [x] 9.1.2 [421（误导请求）状态码](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/9-zh-cn.md#912-421误导请求状态码)
   - [x] 9.2 [使用 TLS 功能](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/9-zh-cn.md#92-使用tls功能)
   - [x] 9.2.1 [TLS1.2 功能](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/9-zh-cn.md#921-tls-12功能)
   - [x] 9.2.2 [TLS1.2 加密套件](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/9-zh-cn.md#922-tls-12加密套件)
+ [ ] 10. [安全性考虑](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/10-zh-cn.md#10-安全性考虑)
  - [x] 10.1. [服务器授权](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/10-zh-cn.md#101-服务器授权)
  - [x] 10.2. [跨协议攻击](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/10-zh-cn.md#102-跨协议攻击)
  - [x] 10.3. [伪装中介攻击](https://github.com/yuuoniy/rfc7540-translation-zh_cn/blob/master/10-zh-cn.md#103-伪装中介攻击)
+ [ ] 11. IANA Considerations
+ [ ] 12. References
+ [ ] Appendix A. TLS 1.2 Cipher Suite Black List

校对审核进度：
+ [x] 1. Instroduction
+ [ ] 2. Overview
+ [ ] 3. Starting HTTP/2
+ [x] 4. HTTP 帧
+ [ ] 5. 流与多路复用
  - [ ] 5.1. 流的状态
  - [ ] 5.1.1. 流标识符
  - [ ] 5.1.2 流的并发性
  - [ ] 5.2 流控
  - [ ] 5.2.1 流控原则
  - [ ] 5.2.2 合理使用流控
  - [ ] 5.3 流的优先级
  - [ ] 5.3.1 流的依赖关系
  - [ ] 5.3.2 依赖权重
  - [ ] 5.3.3 优先级重排
  - [ ] 5.3.4 优先级状态管理
  - [ ] 5.3.5 默认优先级
  - [ ] 5.4 错误处理
  - [ ] 5.4.1 连接错误
  - [ ] 5.4.2 流错误
  - [ ] 5.4.3 连接终止
  - [ ] 5.5 HTTP/2 扩展
+ [ ] 6. Frame Definitions
+ [ ] 7. Error Codes
+ [ ] 8. HTTP Message Exchanges
+ [ ] 9. Additional HTTP Requirements/Considerations
+ [ ] 10. Security Considerations
+ [ ] 11. IANA Considerations
+ [ ] 12. References
+ [ ] Appendix A. TLS 1.2 Cipher Suite Black List
