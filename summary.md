# 面试汇总

## 网络协议相关

- websocket、http （长/短）轮询

- http 是否发送完成
    Message body = Transfer-Encoding encode(Entity body)
如何确定消息体的长度? HTTP 1.1标准给出了如下方法(按照优先级依次排列):
    1, 响应状态(Response Status)为1xx/204/304或者请求方法为HEAD时,消息体长度为0.
    2, 如果使用了非"identity"的Transfer-Encoding编码方式,则消息体长度由"chunked"编码决定,除非该消息以连接关闭为结束.
    3, 如果存在"Content-Length"实体头,则消息长度为该数值.
    3, 如果消息使用关闭连接方式代表消息体结束,则长度由关闭前收到的长度决定. 该条对HTTP Request包含的消息体不适用.

## api 安全问题

- 如何防止参数篡改
    - 客户端按照一定的顺序将参数进行签名，并将签名放到请求参数中
    - 使用秘钥对请求参数进行签名，并与签名对比
    - 签名的秘钥问题：token，hardcode
- 防止重放攻击
    - 基于时间戳：将 timestamp 与其他参数一起签名，服务端限制时间戳 60s 有效。只能减少不能杜绝重放
    - 基于 nonce： nonce 为 1 分钟之内不重复的随机字符串，服务端存储 1 分钟。