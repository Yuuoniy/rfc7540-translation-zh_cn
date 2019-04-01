## HTTP Message Exchanges / HTTP 消息交换
*译者注：能力有限，在不能确定的译文中，使用* \*\* \*\* *语法着重标出，校对者请留意。*

> HTTP/2 is intended to be as compatible as possible with current uses of HTTP. This means that, from the application perspective, the features of the protocol are largely unchanged. To achieve this, all request and response semantics are preserved, although the syntax of conveying those semantics has changed.

HTTP/2 致力于尽可能与当前使用的 HTTP 兼容。这意味着，从应用的视角，协议的特性主要部分是没有变化的。为了实现它，所有请求和响应的语义都被保留，尽管传输这些语义的语法改变了。

> Thus, the specification and requirements of HTTP/1.1 Semantics and Content [RFC7231], Conditional Requests [RFC7232], Range Requests [RFC7233], Caching [RFC7234], and Authentication [RFC7235] are applicable to HTTP/2. Selected portions of HTTP/1.1 Message Syntax and Routing [RFC7230], such as the HTTP and HTTPS URI schemes, are also applicable in HTTP/2, but the expression of those semantics for this protocol are defined in the sections below.

因此，HTTP/1.1 语义和内容 [RFC7231], 条件请求 [RFC7232], 范围请求 [RFC7233]，缓存 [RFC7234]，和认证 [RFC7235] 的规范和要求同样适用于 HTTP/2。HTTP/1.1 消息语法和路由 [RFC7230] 的**选定部分**，像是 HTTP 和 HTTPS URL 模式也同样适用于 HTTP/2, 但是协议如何表达这些语义在下面章节定义。

### 8.1. HTTP Request/Response Exchange / HTTP 请求/响应交换
> A client sends an HTTP request on a new stream, using a previously unused stream identifier (Section 5.1.1). A server sends an HTTP response on the same stream as the request.

客户端在一个新得流上发送 HTTP 请求，使用一个以前没有使用的流标识符（Section 5.1.1）。服务端在和请求相同的流上返回 HTTP 响应。

> An HTTP message (request or response) consists of:
1. for a response only, zero or more HEADERS frames (each followed by zero or more CONTINUATION frames) containing the message headers of informational (1xx) HTTP responses (see [RFC7230], Section 3.2 and [RFC7231], Section 6.2),
2. one HEADERS frame (followed by zero or more CONTINUATION frames) containing the message headers (see [RFC7230], Section 3.2),
3. zero or more DATA frames containing the payload body (see [RFC7230], Section 3.3), and
4. optionally, one HEADERS frame, followed by zero or more CONTINUATION frames containing the trailer-part, if present (see [RFC7230], Section 4.1.2).

一个 HTTP 消息（请求和响应）包含：
1. 只对响应来说， 0 或者以上的 HEADERS 帧（每一个跟着 0 或者以上的 CONTINUATION 帧），包含 HTTP 响应信息的消息头（see [RFC7230], Section 3.2 and [RFC7231], Section 6.2），
2. 一个 HEADERS 帧（跟着 0 或者以上的 CONTINUATION 帧），包含消息头（see [RFC7230], Section 3.2）,
3. 0 或者更多的数据帧，包含载荷体（see [RFC7230], Section 3.3），和
4. 可选的，一个 HEADERS 帧，如多存在的话，跟着 0 或者以上的包含尾部的 CONTINUATION 帧（[RFC7230], Section 4.1.2）

> The last frame in the sequence bears an END_STREAM flag, noting that a HEADERS frame bearing the END_STREAM flag can be followed by CONTINUATION frames that carry any remaining portions of the header block.

序列的最后一帧必须带有 END_STREAM 标志，**一个带有 END_STREAM 标志的 HEADERS 帧不可以被任何带有剩余部分头部块的 CONTINUATION 帧跟随**。

> Other frames (from any stream) MUST NOT occur between the HEADERS frame and any CONTINUATION frames that might follow.

其余的帧（来自任何流）不能在 HEADERS 帧和可能跟随 HEADERS 帧的 CONTINUATION 帧之间出现。

> HTTP/2 uses DATA frames to carry message payloads. The "chunked" transfer encoding defined in Section 4.1 of [RFC7230] MUST NOT be used in HTTP/2.

HTTP/2 使用 DATA 帧携带消息载荷。[RFC7230] 中 4.1 节定义的"区块"转移编码不能在 HTTP/2 中使用。

>Trailing header fields are carried in a header block that also terminates the stream. Such a header block is a sequence starting with a HEADERS frame, followed by zero or more CONTINUATION frames, where the HEADERS frame bears an END_STREAM flag. Header blocks after the first that do not terminate the stream are not part of an HTTP request or response.

头部块携带 Trailing 头部字段也能结束流。这样的头部块是一个以 HEADERS 帧开始的序列，HEADERS 帧跟着 0 或者以上的 CONTINUATION 帧，其中 HEADERS 帧带有 END_STREAM 标志。在第一个没有结束流的帧之后的头部块不是 HTTP 请求和响应的部分。

>A HEADERS frame (and associated CONTINUATION frames) can only appear at the start or end of a stream. An endpoint that receives a HEADERS frame without the END_STREAM flag set after receiving a final (non- informational) status code MUST treat the corresponding request or response as malformed (Section 8.1.2.6).

一个 HEADERS 帧（和相关联 CONTINUATION 帧）只能在流的开始或者结束出现。在收到最终（无信息）状态码之后，一个又收到没有 END_STREAM 标志的 HEADERS 帧端系统必须把响应的请求或者响应当作是不规范的。

>An HTTP request/response exchange fully consumes a single stream. A request starts with the HEADERS frame that puts the stream into an "open" state. The request ends with a frame bearing END_STREAM, which causes the stream to become "half-closed (local)" for the client and "half-closed (remote)" for the server. A response starts with a HEADERS frame and ends with a frame bearing END_STREAM, which places the stream in the "closed" state.

一个 HTTP 请求/响应交换完全的消耗一个流。一个请求以 HEADERS 帧开始，将流变为"open"状态。请求以携带 END_STREAM 标志的帧结束，将客户端流的状态变为"half-closed (local)"，将服务端流的状态变为"half-closed (remote)"，一个响应以 HEADERS 帧开始，带有 END_STREAM 标志的帧结束，将流变为"closed"状态。

>An HTTP response is complete after the server sends -- or the client receives -- a frame with the END_STREAM flag set (including any CONTINUATION frames needed to complete a header block). A server can send a complete response prior to the client sending an entire request if the response does not depend on any portion of the request that has not been sent and received. When this is true, a server MAY request that the client abort transmission of a request without error by sending a RST_STREAM with an error code of NO_ERROR after sending a complete response (i.e., a frame with the END_STREAM flag). Clients MUST NOT discard responses as a result of receiving such a RST_STREAM, though clients can always discard responses at their discretion for other reasons.

一个 HTTP 响应在服务器发送或者客户端收到一个有 END_STREAM 标识的帧（包括任何需要用来完成头部块的 CONTINUATION 帧）完成。如果响应不依赖于还没被发送或者接收的请求的任何部分，在客户端发送完整的请求之前服务端可以完整的发送响应。当这成立时，服务端可能请求客户端无错误的放弃请求传输，通过在发送完整响应（类如，一个有 END_STREAM 标志的帧）之后发送一个错误码为 NO_ERROR 的 RST_STREAM。

#### 8.1.1. Upgrading from HTTP/2 / 从 HTTP/2 升级
> HTTP/2 removes support for the 101 (Switching Protocols) informational status code ([RFC7231], Section 6.2.2).
The semantics of 101 (Switching Protocols) aren't applicable to a multiplexed protocol. Alternative protocols are able to use the same mechanisms that HTTP/2 uses to negotiate their use (see Section 3).

HTTP/2 移除了对 101（协议转换）信息状态码的支持 ([RFC7231], Section 6.2.2)。
101（协议转换）不适用多路协议。**替代的协议可能使用和 HTTP/2 相同的机制，协商它们的使用。**

#### 8.1.2. HTTP Header Fields / HTTP 头部字段
>HTTP header fields carry information as a series of key-value pairs. For a listing of registered HTTP headers, see the "Message Header Field" registry maintained at <https://www.iana.org/assignments/ message-headers>.

HTTP 头部字段携带了一系列键值对信息。已注册的 HTTP 头列表，可以在"消息头部字段"注册处看到，维持在<https://www.iana.org/assignments/message-headers>。

> Just as in HTTP/1.x, header field names are strings of ASCII characters that are compared in a case-insensitive fashion. However, header field names MUST be converted to lowercase prior to their encoding in HTTP/2. A request or response containing uppercase header field names MUST be treated as malformed (Section 8.1.2.6).

就像 HTTP/1.x，头部字段名称是大小写不敏感的 ASCII 字符串。然而，头部字段名称必须在 HTTP/2 编码前转换为小写字母。包含大写头部字段名称的请求或者响应必须被当作是不规范的。

##### 8.1.2.1. Pseudo-Header Fields 伪头部字段
>While HTTP/1.x used the message start-line (see [RFC7230], Section 3.1) to convey the target URI, the method of the request, and the status code for the response, HTTP/2 uses special pseudo-header fields beginning with ':' character (ASCII 0x3a) for this purpose.

HTTP/1.x 使用消息开始行（see [RFC7230], Section 3.1）传递目标 URL，请求方法，响应状态码，HTTP/2 使用特殊的以":"开始的伪头部字段来达到这个目的。

>Pseudo-header fields are not HTTP header fields. Endpoints MUST NOT generate pseudo-header fields other than those defined in this document.

伪头部字段不是 HTTP 头部字段。端系统不能构造伪头部字段除非是在这篇文档里定义的。

>Pseudo-header fields are only valid in the context in which they are defined. Pseudo-header fields defined for requests MUST NOT appear in responses; pseudo-header fields defined for responses MUST NOT appear in requests. Pseudo-header fields MUST NOT appear in trailers. Endpoints MUST treat a request or response that contains undefined or invalid pseudo-header fields as malformed (Section 8.1.2.6).

伪头部字段只有在他们定义的语境中才有效。为请求定义的伪头部字段不能在响应中出现；为响应定义的伪头部字段不能在请求中出现。伪头部字段不能在尾部出现。端系统必须把包含未定义或无效的伪头部字段的请求或者响应当作是不规范的 (Section 8.1.2.6)。

>All pseudo-header fields MUST appear in the header block before regular header fields. Any request or response that contains a pseudo-header field that appears in a header block after a regular header field MUST be treated as malformed (Section 8.1.2.6).

所有的在头部块中的伪头部字段必须出现在**常规**的头部字段之前。任何包含伪头部字段出现在常规头部字段之后的请求或者响应必须被当作是不规范的。

##### 8.1.2.2. Connection-Specific Header Fields / Connection-Specific 头部字段
>HTTP/2 does not use the Connection header field to indicate connection-specific header fields; in this protocol, connection-specific metadata is conveyed by other means. An endpoint MUST NOT generate an HTTP/2 message containing connection-specific header fields; any message containing connection-specific header fields MUST be treated as malformed (Section 8.1.2.6).

HTTP/2 不使用 Connection 说明头部字段说明 connection-specific 头部字段；在协议中，connection-specific 元信息被使用其他方式传递。端系统不能产生包含连接-说明头部字段的 HTTP/2 消息；任何包含 connection-specific 头部字段的消息必须被当作成不规范的 (Section 8.1.2.6)。

>The only exception to this is the TE header field, which MAY be present in an HTTP/2 request; when it is, it MUST NOT contain any value other than "trailers".

唯一的例外就是 TE 头部字段，可能在 HTTP/2 请求中出现；当这时时，它必须包含非"trailers"值。

>This means that an intermediary transforming an HTTP/1.x message to HTTP/2 will need to remove any header fields nominated by the Connection header field, along with the Connection header field itself. Such intermediaries SHOULD also remove other connection-specific header fields, such as Keep-Alive, Proxy-Connection, Transfer-Encoding, and Upgrade, even if they are not nominated by the Connection header field.

这意味着任何将 HTTP/1.x 消息转换为 HTTP/2 的中介必须移除任何被 Connection 头部字段**提名**的头部字段，和 Connection 头部字段自己。这些中介应该移除其他 connection-specific 头部字段，比如 Keep-Alive, Proxy-Connection，Transfer-Encoding，和 Upgrade，即使他们没有被连接头部字段提名。

> Note: HTTP/2 purposefully does not support upgrade to another protocol. The handshake methods described in Section 3 are believed sufficient to negotiate the use of alternative protocols.

注意：HTTP/2 无意支持升级到其他协议。第三节介绍的握手方法被认为是有效的来协商使用替代协议。

##### 8.1.2.3. Request Pseudo-Header Fields / 请求伪头部字段
>The following pseudo-header fields are defined for HTTP/2 requests:
- The ":method" pseudo-header field includes the HTTP method ([RFC7231], Section 4).
- The ":scheme" pseudo-header field includes the scheme portion of the target URI ([RFC3986], Section 3.1).
":scheme" is not restricted to "http" and "https" schemed URIs. A proxy or gateway can translate requests for non-HTTP schemes, enabling the use of HTTP to interact with non-HTTP services.
- The ":authority" pseudo-header field includes the authority portion of the target URI ([RFC3986], Section 3.2). The authority MUST NOT include the deprecated "userinfo" subcomponent for "http" or "https" schemed URIs.<br>
To ensure that the HTTP/1.1 request line can be reproduced accurately, this pseudo-header field MUST be omitted when translating from an HTTP/1.1 request that has a request target in origin or asterisk form (see [RFC7230], Section 5.3). Clients that generate HTTP/2 requests directly SHOULD use the ":authority" pseudo-header field instead of the Host header field. An intermediary that converts an HTTP/2 request to HTTP/1.1 MUST create a Host header field if one is not present in a request by copying the value of the ":authority" pseudo-header field.
- The ":path" pseudo-header field includes the path and query parts of the target URI (the "path-absolute" production and optionally a '?' character followed by the "query" production (see Sections 3.3 and 3.4 of [RFC3986]). A request in asterisk form includes the value '\*' for the ":path" pseudo-header field.<br>
This pseudo-header field MUST NOT be empty for "http" or "https" URIs; "http" or "https" URIs that do not contain a path component MUST include a value of '/'. The exception to this rule is an OPTIONS request for an "http" or "https" URI that does not include a path component; these MUST include a ":path" pseudo-header field with a value of '*' (see [RFC7230], Section 5.3.4).

下列 HTTP/2 伪头部字段为 HTTP/2 请求定义：
- ":method"伪头部字段包含 HTTP 方法 ([RFC7231], Section 4)。
- ":scheme"伪头部字段包含目标 URL 模式部分 ([RFC3986], Section 3.1)。
- ":authority"伪头部字段包含目标 RUL 认证部分 ([RFC3986], Section 3.2)。认证不能包含"http"或"https"URLs 的废弃的"userinfo"子部分。<br>
为了确保 HTTP/1.1 请求行可以被准确的重现，当从在** origin 或 asterisk **形式中有请求目标的 HTTP/1.1 请求翻译时，这个伪头部字段必须被忽略 (see [RFC7230], Section 5.3)。发起 HTTP/2 请求的客户端应该直接使用":authority"伪头部字段来替代 Host 头部字段。将 HTTP/2 请求转换为 HTTP/1.1 的中介创建一个 Host 头部字段如果请求中不存在，通过复制":authority"伪头部字段的值来实现。
- ":path"伪头部字段包含目标 URL 的路径和查询部分（绝对路径产生式和一个跟着"？"字符的查询产生式）。星号形式的请求包含值为"\*"的":path"伪头部字段。<br>
这个伪头部字段对"http"或"https"URLs 来说不能为空；不包含 path 部分‘http’或‘https’URLs 必须包含一个‘/’值。这个规则的例外是不包含 path 部分的"http"或"https"URL 的 OPTIONS 请求；这个必须包含值为"*"的":path"伪头部字段 (see [RFC7230], Section 5.3.4)。

>All HTTP/2 requests MUST include exactly one valid value for the ":method", ":scheme", and ":path" pseudo-header fields, unless it is a CONNECT request (Section 8.3). An HTTP request that omits mandatory pseudo-header fields is malformed (Section 8.1.2.6).

所有的 HTTP/2 请求必须准确的包含有效的":method", ":scheme", 和 ":path"伪头部字段，除非它是一个 CONNECT 请求 (Section 8.3)。一个省略强制性伪头部字段的 HTTP 请求是不规范的 (Section 8.1.2.6).

> HTTP/2 does not define a way to carry the version identifier that is included in the HTTP/1.1 request line.

HTTP/2 没有定义一个携带包含在 HTTP/1.1 请求行中的版本标识符的方法。

##### 8.1.2.4. Response Pseudo-Header Fields / 响应伪头部字段
> For HTTP/2 responses, a single ":status" pseudo-header field is defined that carries the HTTP status code field (see [RFC7231], Section 6). This pseudo-header field MUST be included in all responses; otherwise, the response is malformed (Section 8.1.2.6).

对于 HTTP/2 响应，一个":status"伪头部字段被定义来携带 HTTP 状态码字段 (see [RFC7231], Section 6)。这个伪头部字段必须被所有的响应包含；否则，响应是不规范的 (Section 8.1.2.6).

> HTTP/2 does not define a way to carry the version or reason phrase that is included in an HTTP/1.1 status line.

HTTP/2 没有定义携带包含在 HTTP/1.1 状态行中的版本或原因解释的方法。

##### 8.1.2.5. Compressing the Cookie Header Field / 压缩 Cookie 头部字段
> The Cookie header field [COOKIE] uses a semi-colon (";") to delimit cookie-pairs (or "crumbs"). This header field doesn't follow the list construction rules in HTTP (see [RFC7230], Section 3.2.2), which prevents cookie-pairs from being separated into different name-value pairs. This can significantly reduce compression efficiency as individual cookie-pairs are updated.

Cookie 头部字段 [COOKIE] 使用分号"；"分割 cookie 对（或 "crumbs"）。这个头部字段没有遵循防止 cookie-pairs 被分割到不同的键值对的 HTTP 列表构造法则 (see [RFC7230], Section 3.2.2)。这可以显著地提升了压缩效率当单独的 cookie-pairs 被更新时。

> To allow for better compression efficiency, the Cookie header field MAY be split into separate header fields, each with one or more cookie-pairs. If there are multiple Cookie header fields after decompression, these MUST be concatenated into a single octet string using the two-octet delimiter of 0x3B, 0x20 (the ASCII string "; ") before being passed into a non-HTTP/2 context, such as an HTTP/1.1 connection, or a generic HTTP server application.

为了得到更好的压缩效率，Cookie 头部字段可能被分割成多个头部字段，每一个有一个或多个 cookie-pairs。在他们被传递到非 HTTP/2 语境中，比如 HTTP/1.1 连接，或者一个通用的 HTTP 服务应用，如果解压缩之后有多个 Cookie 头部字段，他们必须被连接成一个使用 0x3B, 0x20（ASCII 字符串 "; "）这两个分隔符的序列。

> Therefore, the following two lists of Cookie header fields are semantically equivalent.<br>
  cookie: a=b; c=d; e=f<br>
  cookie: a=b<br>
  cookie: c=d<br>
  cookie: e=f

因此，下列两个 Cookie 头部字段在语义上是相等的。<br>
cookie: a=b<br>
cookie: c=d<br>
cookie: e=f

##### 8.1.2.6. Malformed Requests and Responses / 不规范的请求和响应
> A malformed request or response is one that is an otherwise valid sequence of HTTP/2 frames but is invalid due to the presence of extraneous frames, prohibited header fields, the absence of mandatory header fields, or the inclusion of uppercase header field names.

一个不规范的请求或者响应是一个有效的 HTTP/2 帧序列，但是因为额外的帧，禁止的头部字段，缺少强制性头部字段，或者包含大写头部字段名字而无效。

> A request or response that includes a payload body can include a content-length header field. A request or response is also malformed if the value of a content-length header field does not equal the sum of the DATA frame payload lengths that form the body. A response that is defined to have no payload, as described in [RFC7230], Section 3.3.2, can have a non-zero content-length header field, even though no content is included in DATA frames.

一个包含载荷的请求或者响应可以包含 content-length 头部字段。如果 content-length 字段的值不等于组成 body 的 DATA 帧载荷，请求或者响应也是不规范的。一个被定义没有载荷的响应，在 [RFC7230] 3.3.2 节中描述，可以有一个非零的 content-length 值，即使在 DATA 中没有内容包含。

> Intermediaries that process HTTP requests or responses (i.e., any intermediary not acting as a tunnel) MUST NOT forward a malformed request or response. Malformed requests or responses that are detected MUST be treated as a stream error (Section 5.4.2) of type PROTOCOL_ERROR.

处理 HTTP 请求和响应的中介（比如任何不是隧道角色的中介）不能前递一个不规范的请求或者响应。被侦测出来不规范的请求或者响应必须被当成 PROTOCOL_ERROR 类型的流错误（Section 5.4.2）

> For malformed requests, a server MAY send an HTTP response prior to closing or resetting the stream. Clients MUST NOT accept a malformed response. Note that these requirements are intended to protect against several types of common attacks against HTTP; they are deliberately strict because being permissive can expose implementations to these vulnerabilities.

对一个不规范的请求，在关闭或者重置流之前服务端可能发送 HTTP 响应。客户端不能接收不规范的响应。注意这些要求是为了防止几种类型的常见 HTTP 攻击；他们是故意的如此严格，因为宽松可能使这些漏洞被揭发实现。

#### 8.1.3. Examples / 例子
> This section shows HTTP/1.1 requests and responses, with illustrations of equivalent HTTP/2 requests and responses.

这部分展示了一些有响应 HTTP/2 请求和响应的 HTTP/1.1 请求和响应。

> An HTTP GET request includes request header fields and no payload body and is therefore transmitted as a single HEADERS frame, followed by zero or more CONTINUATION frames containing the serialized block of request header fields. The HEADERS frame in the following has both the END_HEADERS and END_STREAM flags set; no CONTINUATION frames are sent.

一个 HTTP GET 请求包含请求头部字段没有载荷体，因此被以一个单独的 HEADERS 帧传输，跟着零个或多个包含请求头部字段序列块的 CONTINUATION 帧。下面的这个 HEADERS 帧设置了 END_HEADERS 和 END_STREAM 标志位；没有发送 CONTINUATION 帧。

```
GET /resource HTTP/1.1           HEADERS
Host: example.org          ==>     + END_STREAM
Accept: image/jpeg                 + END_HEADERS
                                        :method = GET
                                        :scheme = https
                                        :path = /resource
                                        host = example.org
                                        accept = image/jpeg
```

> Similarly, a response that includes only response header fields is transmitted as a HEADERS frame (again, followed by zero or more CONTINUATION frames) containing the serialized block of response header fields.

同时，只包含响应头部字段的响应的也被以一个包含响应头部字段序列块的 HEADERS 帧传输（当然，跟着零或多个 CONTINUATION 帧）

```
HTTP/1.1 304 Not Modified        HEADERS
ETag: "xyzzy"              ==>     + END_STREAM
Expires: Thu, 23 Jan ...           + END_HEADERS
                                     :status = 304
                                     etag = "xyzzy"
                                     expires = Thu, 23 Jan ...
```

> An HTTP POST request that includes request header fields and payload data is transmitted as one HEADERS frame, followed by zero or more CONTINUATION frames containing the request header fields, followed by one or more DATA frames, with the last CONTINUATION (or HEADERS) frame having the END_HEADERS flag set and the final DATA frame having the END_STREAM flag set:

一个包含请求头部字段和载荷数据的 HTTP POST 被以一个单独的 HEADERS 帧传输，跟着零个或多个包含请求头部字段序列块的 CONTINUATION 帧
，再跟着一个或多个 DATA 帧，其中最后一个 CONTINUATION 帧（或 HEADERS 帧）设置了 END_HEADERS 标志位，最后一个 DATA 帧设置了 END_STREAM 标志位。

```
POST /resource HTTP/1.1          HEADERS
Host: example.org          ==>     - END_STREAM
Content-Type: image/jpeg           - END_HEADERS
Content-Length: 123                  :method = POST
                                     :path = /resource
{binary data}                        :scheme = https

                                 CONTINUATION
                                   + END_HEADERS
                                     content-type = image/jpeg
                                     host = example.org
                                     content-length = 123

                                 DATA
                                   + END_STREAM
                                 {binary data}
```

> Note that data contributing to any given header field could be spread between header block fragments. The allocation of header fields to frames in this example is illustrative only.

**注意任何给定头部字段的相关数据可能会在头部块段之间散布。**这个例子中的帧的头部字段的分配仅作为说明。

> A response that includes header fields and payload data is transmitted as a HEADERS frame, followed by zero or more CONTINUATION frames, followed by one or more DATA frames, with the last DATA frame in the sequence having the END_STREAM flag set:

一个包含请求头部字段和载荷数据的响应被以一个单独的 HEADERS 帧传输，跟着零个或多个的 CONTINUATION 帧，再跟着一个或多个 DATA 帧，其中序列中最后一个 DATA 帧设置了 END_STREAM 标志位。

```
HTTP/1.1 200 OK                  HEADERS
Content-Type: image/jpeg   ==>     - END_STREAM
Content-Length: 123                + END_HEADERS
                                     :status = 200
{binary data}                        content-type = image/jpeg
                                     content-length = 123

                                 DATA
                                   + END_STREAM
                                 {binary data}
```

> An informational response using a 1xx status code other than 101 is transmitted as a HEADERS frame, followed by zero or more CONTINUATION frames.

使用除了 101 之外的 1xx 状态码的信息响应以一个 HEADERS 帧传输，跟着零个或多个的 CONTINUATION 帧。

> Trailing header fields are sent as a header block after both the request or response header block and all the DATA frames have been sent. The HEADERS frame starting the trailers header block has the END_STREAM flag set.
The following example includes both a 100 (Continue) status code, which is sent in response to a request containing a "100-continue" token in the Expect header field, and trailing header fields:

Trailing 头部字段在请求和响应头部块以及所有的 DATA 帧传输完成之后以头部块的形式传输。HEADERS 帧设置 END_STREAM 标志开始尾部头部字段传输。下面的例子包含了一个 100（Continue）状态码和尾部头部字段，被作为对在 Except 头部字段包含"100-continue"标识对的请求的响应中发送。

```
HTTP/1.1 100 Continue            HEADERS
Extension-Field: bar       ==>     - END_STREAM
                                   + END_HEADERS
                                     :status = 100
                                     extension-field = bar

HTTP/1.1 200 OK                  HEADERS
Content-Type: image/jpeg   ==>     - END_STREAM
Transfer-Encoding: chunked         + END_HEADERS
Trailer: Foo                         :status = 200
                                     content-length = 123
123                                  content-type = image/jpeg
{binary data}                        trailer = Foo
0
Foo: bar                         DATA
                                   - END_STREAM
                                 {binary data}

                                 HEADERS
                                   + END_STREAM
                                   + END_HEADERS
                                     foo = bar
```

#### 8.1.4. Request Reliability Mechanisms in HTTP/2 / HTTP/2 请求可靠机制
> In HTTP/1.1, an HTTP client is unable to retry a non-idempotent request when an error occurs because there is no means to determine the nature of the error. It is possible that some server processing occurred prior to the error, which could result in undesirable effects if the request were reattempted.

在 HTTP/1.1 中，当错误发生时一个 HTTP 客户端不能重复一个非幂等的请求，因为没有方法确定错误的原因。可能一些服务器处理在错误发生之前已经处理了，当重新尝试请求时可能会造成不期望的后果。

> HTTP/2 provides two mechanisms for providing a guarantee to a client that a request has not been processed:
- The GOAWAY frame indicates the highest stream number that might have been processed. Requests on streams with higher numbers are therefore guaranteed to be safe to retry.
- The REFUSED_STREAM error code can be included in a RST_STREAM frame to indicate that the stream is being closed prior to any processing having occurred. Any request that was sent on the reset stream can be safely retried.

HTTP/2 提供了两套机制能向客户端保证一个请求没有被处理。
- GOAWAY 帧表示了可能被处理的最高流标识符。在有更高流标识符的流上的请求能保证安全的重试。
- REFUSED_STREAM 错误码可以被包含在 RST_STREAM 帧中来表示流在任何处理发生之前就被关闭了。

> Requests that have not been processed have not failed; clients MAY automatically retry them, even those with non-idempotent methods.

还没有被处理的请求没失败；客户端可以自动的重试他们，即使他们有非幂等的方法。

> A server MUST NOT indicate that a stream has not been processed unless it can guarantee that fact. If frames that are on a stream are passed to the application layer for any stream, then REFUSED_STREAM MUST NOT be used for that stream, and a GOAWAY frame MUST include a stream identifier that is greater than or equal to the given stream identifier.

服务端不能表明一个流还没有被处理除非能保证这个事实。如果任何一个流上的帧已经传递到应用层，REFUSED_STREAM 不能被用在这个流上，并且一个 GOAWAY 帧必须包含一个大于等于给定流标识符的流标识符。

> In addition to these mechanisms, the PING frame provides a way for a client to easily test a connection. Connections that remain idle can become broken as some middleboxes (for instance, network address translators or load balancers) silently discard connection bindings. The PING frame allows a client to safely test whether a connection is still active without sending a request.

在这些机制之外，PING 帧给客户端提供了一种简单测试连接的方式。闲置状态的连接可能被中间设备（例如，网络地址转换器或载荷均衡）静默的放弃连接绑定破坏。PING 帧允许客户端安全的测试连接是否是活动的而不必发送请求。

### 8.2. Server Push / 服务端推送
> HTTP/2 allows a server to pre-emptively send (or "push") responses (along with corresponding "promised" requests) to a client in association with a previous client-initiated request. This can be useful when the server knows the client will need to have those responses available in order to fully process the response to the original request.

HTTP/2 允许服务器先行发送（或推送）与先前客户端初始请求相关的响应（对应"许诺"的请求）到客户端。这在服务端知道客户端将会须要这些响应来完全地处理原始请求的响应时会很有用。

> A client can request that server push be disabled, though this is negotiated for each hop independently. The SETTINGS_ENABLE_PUSH setting can be set to 0 to indicate that server push is disabled.

客户端可以请求服务端推送禁用，尽管这在每一跳独立的协商。SETTINGS_ENABLE_PUSH 被设置为 0 表示服务端推送关闭。

> Promised requests MUST be cacheable (see [RFC7231], Section 4.2.3), MUST be safe (see [RFC7231], Section 4.2.1), and MUST NOT include a request body. Clients that receive a promised request that is not cacheable, that is not known to be safe, or that indicates the presence of a request body MUST reset the promised stream with a stream error (Section 5.4.2) of type PROTOCOL_ERROR. Note this could result in the promised stream being reset if the client does not recognize a newly defined method as being safe.

许诺的请求必须是可缓存的（see [RFC7231], Section 4.2.3），必须是安全的（see [RFC7231], Section 4.2.1），必须不能包含请求体。客户端收到不是可缓存的，不能被识别为安全的或者是表明存在请求体的许诺请求必须重置许诺流并设置 PROTOCOL_ERROR 类型的流错误（Section 5.4.2）。注意这可能导致在客户端不能识别一个新的方法是不是安全的时候重置流。

> Pushed responses that are cacheable (see [RFC7234], Section 3) can be stored by the client, if it implements an HTTP cache. Pushed responses are considered successfully validated on the origin server (e.g., if the "no-cache" cache response directive is present ([RFC7234], Section 5.2.2)) while the stream identified by the promised stream ID is still open.

可缓存的的推送响应可以被客户端存储，如果客户端实现了 HTTP 缓存。推送响应在许诺流 ID 的流标识符依旧打开时，被认为是在原始服务端上成功有效的（例如，如果"no-cache"响应指令存在 ([RFC7234], Section 5.2.2)）。

> Pushed responses that are not cacheable MUST NOT be stored by any HTTP cache. They MAY be made available to the application separately.

不可缓存的推送响应肯定不能被任何 HTTP 缓存存储。这可能在应用分离的时候有效。

> The server MUST include a value in the ":authority" pseudo-header field for which the server is authoritative (see Section 10.1). A client MUST treat a PUSH_PROMISE for which the server is not authoritative as a stream error (Section 5.4.2) of type PROTOCOL_ERROR.

服务端如果是认证的 (see Section 10.1)，必须包含":authority"伪头部字段的值。一个客户端对待一个服务端没有认证的 PUSH_PROMISE 为 PROTOCOL_ERROR 类型的流错误（Section 5.4.2）。

> An intermediary can receive pushes from the server and choose not to forward them on to the client. In other words, how to make use of the pushed information is up to that intermediary. Equally, the intermediary might choose to make additional pushes to the client, without any action taken by the server.

中介可以从服务端接收推送并且选择是否将他们前递给客户端。换一句话说，如何利用推送信息取决于中介。同等的，中介也可以选择推送额外的响应给客户端，在没有接收到服务器任何动作的前提下。

> A client cannot push. Thus, servers MUST treat the receipt of a PUSH_PROMISE frame as a connection error (Section 5.4.1) of type PROTOCOL_ERROR. Clients MUST reject any attempt to change the SETTINGS_ENABLE_PUSH setting to a value other than 0 by treating the message as a connection error (Section 5.4.1) of type PROTOCOL_ERROR.

客户端是不能推送的。因此，服务端必须把 PUSH_PROMISE 帧当作是 PROTOCOL_ERROR 类型的连接错误 (Section 5.4.1)。客户端必须决绝任何尝试把 SETTINGS_ENABLE_PUSH 设置为非零值的消息，并把消息当作是 PROTOCOL_ERROR 类型的连接错误。

#### 8.2.1. Push Requests / 推送请求

> Server push is semantically equivalent to a server responding to a request; however, in this case, that request is also sent by the server, as a PUSH_PROMISE frame.

服务端推送语义上相当于服务端响应一个请求；然而，这种情况下，请求仍然是服务端以 PUSH_PROMISE 帧方式发送。

> The PUSH_PROMISE frame includes a header block that contains a complete set of request header fields that the server attributes to the request. It is not possible to push a response to a request that includes a request body.
Pushed responses are always associated with an explicit request from the client. The PUSH_PROMISE frames sent by the server are sent on that explicit request's stream. The PUSH_PROMISE frame also includes a promised stream identifier, chosen from the stream identifiers available to the server (see Section 5.1.1).

PUSH_PROMISE 帧包含了包含所有服务端在请求里设置的请求头部字段的头部块。这是不可能向一个包含请求体的请求推送响应的。推送响应总是与来自客户端的明确的请求相关。被服务器发送的 PUSH_PROMISE 帧在这个对应的请求流上发送。PUSH_PROMISE 帧当然包含了许诺流标识符，从服务端可用的流标识符空间里选择。

> The header fields in PUSH_PROMISE and any subsequent CONTINUATION frames MUST be a valid and complete set of request header fields (Section 8.1.2.3). The server MUST include a method in the ":method" pseudo-header field that is safe and cacheable. If a client receives a PUSH_PROMISE that does not include a complete and valid set of header fields or the ":method" pseudo-header field identifies a method that is not safe, it MUST respond with a stream error (Section 5.4.2) of type PROTOCOL_ERROR.

PUSH_PROMISE 和任何后续 CONTINUATION 帧里的头部字段必须包含有效完整的请求头部字段 (Section 8.1.2.3)。服务端必须必须包含安全可缓存的":method"伪头部字段。如果客户端收到了没有完整包含有效请求头部字段或者":method"伪头部字段的方法是不安全的 PUSH_PROMISE 帧，他必须被当作是 PROTOCOL_ERROR 类型的流错误 (Section 5.4.2)。

> The server SHOULD send PUSH_PROMISE (Section 6.6) frames prior to sending any frames that reference the promised responses. This avoids a race where clients issue requests prior to receiving any PUSH_PROMISE frames.

服务端应该在发送任何与许诺响应相关的帧之前发送 PUSH_PROMISE(Section 6.6)。这避免了客户端在收到 PUSH_PROMISE 帧之前发送请求的竞争。

> For example, if the server receives a request for a document containing embedded links to multiple image files and the server chooses to push those additional images to the client, sending PUSH_PROMISE frames before the DATA frames that contain the image links ensures that the client is able to see that a resource will be pushed before discovering embedded links. Similarly, if the server pushes responses referenced by the header block (for instance, in Link header fields), sending a PUSH_PROMISE before sending the header block ensures that clients do not request those resources.

例如，如果服务器收到了一个包含多个图像文件的内嵌连接的文档，服务器选择推送这些额外的图像到客户端，并在 DATA 帧之前发送包含图像连接的 PUSH_PROMISE 来确保客户端在发现这些内置连接之前能确保这些资源会被推送。同样的，如果服务端推送了被头部块（for instance, in Link header fields）引用的响应，在发送头部块之前发送 PUSH_PROMISE 帧来确保客户端不会请求这些资源。

> PUSH_PROMISE frames MUST NOT be sent by the client.

PUSH_PROMISE 帧不能被客户端发送。

> PUSH_PROMISE frames can be sent by the server in response to any client-initiated stream, but the stream MUST be in either the "open" or "half-closed (remote)" state with respect to the server. PUSH_PROMISE frames are interspersed with the frames that comprise a response, though they cannot be interspersed with HEADERS and CONTINUATION frames that comprise a single header block.

PUSH_PROMISE 帧可以被服务端发送来响应客户端初始流，但是流对应服务端的状态必须被设置为"open"或"half-closed (remote)"。PUSH_PROMISE 帧点缀在组成响应的帧之间，但是不能点缀在组成一个完整头部块的 HEADERS 和 CONTINUATION 帧。

> Sending a PUSH_PROMISE frame creates a new stream and puts the stream into the "reserved (local)" state for the server and the "reserved (remote)" state for the client.

发送一个 PUSH_PROMISE 帧创建了一个新的流，并将服务器端流的状态改为"reserved (local)"，客户端的状态改为"reserved (remote)"。

#### 8.2.2. Push Responses / 推送响应

> After sending the PUSH_PROMISE frame, the server can begin delivering the pushed response as a response (Section 8.1.2.4) on a server-initiated stream that uses the promised stream identifier. The server uses this stream to transmit an HTTP response, using the same sequence of frames as defined in Section 8.1. This stream becomes "half-closed" to the client (Section 5.1) after the initial HEADERS frame is sent.

在发送 PUSH_PROMISE 帧之后，服务端可以开始传递被推送的响应作为响应在使用许诺流标识符服务端初始的流上。服务端使用这个流传递 HTTP 响应，使用和在 8.1 节中定义的相同的帧序列。在初始 HEADERS 被发送之后这个流对于客户端变成"half-closed"状态 (Section 5.1)。

> Once a client receives a PUSH_PROMISE frame and chooses to accept the pushed response, the client SHOULD NOT issue any requests for the promised response until after the promised stream has closed.

如果客户端收到了 PUSH_PROMISE 帧并选择接收推送响应，客户端不应该在许诺流关闭之前发起任何关于许诺响应的请求。

> If the client determines, for any reason, that it does not wish to receive the pushed response from the server or if the server takes too long to begin sending the promised response, the client can send a RST_STREAM frame, using either the CANCEL or REFUSED_STREAM code and referencing the pushed stream's identifier.

如果客户端因为某些原因，决定不接收从服务端来的推送响应，或者服务端花费了太长的时间来发送许诺响应，客户端可以发送 RST_STREAM 帧，使用 CANCEL 或 REFUSED_STREAM 码并且引用推送流标识符。

> A client can use the SETTINGS_MAX_CONCURRENT_STREAMS setting to limit the number of responses that can be concurrently pushed by a server. Advertising a SETTINGS_MAX_CONCURRENT_STREAMS value of zero disables server push by preventing the server from creating the necessary streams. This does not prohibit a server from sending PUSH_PROMISE frames; clients need to reset any promised streams that are not wanted.

客户端可以使用 SETTINGS_MAX_CONCURRENT_STREAMS 来限制服务器推送的最大并行响应数。设置 SETTINGS_MAX_CONCURRENT_STREAMS 为零可以禁用推送来阻止服务器创建不必要的流。这不禁止服务端发送 PUSH_PROMISE 帧；客户端需要重置任何不期望的许诺流。

> Clients receiving a pushed response MUST validate that either the server is authoritative (see Section 10.1) or the proxy that provided the pushed response is configured for the corresponding request. For example, a server that offers a certificate for only the "example.com" DNS-ID or Common Name is not permitted to push a response for "https://www.example.org/doc".

客户端收到的任何推送响应必须验证服务器是认证的 (see Section 10.1) 或者代理被配置为为相应的请求提供推送响应。例如，只提供"example.com" DNS-ID 或者 Common Name 证书的服务器是不允许为"https://www.example.org/doc"推送响应的。

> The response for a PUSH_PROMISE stream begins with a HEADERS frame, which immediately puts the stream into the "half-closed (remote)" state for the server and "half-closed (local)" state for the client, and ends with a frame bearing END_STREAM, which places the stream in the "closed" state.

以 HEADERS 帧开始的 PUSH_PROMISE 流响应，立即将服务端状态变为"half-closed (remote)"，客户端状态变为"half-closed (local)"，并以设置了 END_STREAM 的帧结束，这会将流置为"closed"状态。

> Note: The client never sends a frame with the END_STREAM flag for a server push.

注意：客户端绝不对服务端推送发送设置 END_STREAM 标志的帧。

### 8.3. The CONNECT Method / CONNECT 方法
> In HTTP/1.x, the pseudo-method CONNECT ([RFC7231], Section 4.3.6) is used to convert an HTTP connection into a tunnel to a remote host. CONNECT is primarily used with HTTP proxies to establish a TLS session with an origin server for the purposes of interacting with "https" resources.

在 HTTP/1.x, 伪方法 CONNECT([RFC7231], Section 4.3.6) 被用来将 HTTP 连接转换成到一个远程主机的隧道。CONNECT 主要被 HTTP 代理用来建立和远程服务端的 TLS 会话，为了和"https"资源交互。

> In HTTP/2, the CONNECT method is used to establish a tunnel over a single HTTP/2 stream to a remote host for similar purposes. The HTTP header field mapping works as defined in Section 8.1.2.3 ("Request Pseudo-Header Fields"), with a few differences. Specifically:
- The ":method" pseudo-header field is set to "CONNECT".
- The ":scheme" and ":path" pseudo-header fields MUST be omitted.
- The ":authority" pseudo-header field contains the host and port to connect to (equivalent to the authority-form of the request-target of CONNECT requests (see [RFC7230], Section 5.3)).

在 HTTP/2，CONNECT 方法被用来在一个 HTTP/2 流上建立一个隧道，为了同样的目的。HTTP 头部字段映射按照在 8.1.2.3 节 ("Request Pseudo-Header Fields") 定义工作，但是有点不同。说明如下：
- ":method"伪头部字段被设置为"CONNECT"。
- ":scheme" 和 ":path"伪头部字段必须被忽略
- ":authority"伪头部字段包含了需要连接的主机和端口（和 CONNECT 请求目标的 authority-form 格式相同 (see [RFC7230], Section 5.3)）。

> A CONNECT request that does not conform to these restrictions is malformed (Section 8.1.2.6).

没有遵守这些约束的 CONNECT 请求是不规范的 (Section 8.1.2.6)。

> A proxy that supports CONNECT establishes a TCP connection [TCP] to the server identified in the ":authority" pseudo-header field. Once this connection is successfully established, the proxy sends a HEADERS frame containing a 2xx series status code to the client, as defined in [RFC7231], Section 4.3.6.

支持用 CONNECT 的代理和":authority"伪头部字段表示的服务器建立 TCP 连接 [TCP]。一旦连接成功建立了，代理发送包含 2xx 序列状态码的头部帧到客户端，在 [RFC7231] 中 4.3.6 节定义。

> After the initial HEADERS frame sent by each peer, all subsequent DATA frames correspond to data sent on the TCP connection. The payload of any DATA frames sent by the client is transmitted by the proxy to the TCP server; data received from the TCP server is assembled into DATA frames by the proxy. Frame types other than DATA or stream management frames (RST_STREAM, WINDOW_UPDATE, and PRIORITY) MUST NOT be sent on a connected stream and MUST be treated as a stream error (Section 5.4.2) if received.

在初始头部帧被每一端发送后，所有的对应数据的后续 DATA 帧被 TCP 连接发送。被客户端发送的 DATA 帧载荷被代理传送到 TCP 服务端；从 TCP 服务端收到的数据被代理重新组合成 DATA 帧。DATA 帧类型之外的帧或者流管理帧 (RST_STREAM, WINDOW_UPDATE, 和 PRIORITY) 不能在已经连接的流发送并且如果收到了被当作成流错误 (Section 5.4.2)。

> The TCP connection can be closed by either peer. The END_STREAM flag on a DATA frame is treated as being equivalent to the TCP FIN bit. A client is expected to send a DATA frame with the END_STREAM flag set after receiving a frame bearing the END_STREAM flag. A proxy that receives a DATA frame with the END_STREAM flag set sends the attached data with the FIN bit set on the last TCP segment. A proxy that receives a TCP segment with the FIN bit set sends a DATA frame with the END_STREAM flag set. Note that the final TCP segment or DATA frame could be empty.

TCP 连接可以被每一端关闭。设置 END_STREAM 的 DATA 帧被同等当作为 TCP FIN 位。在收到设置 END_STREAM 的帧后，客户端期望发送有 END_STREAM 标志的 DATA 帧。收到设置 END_STREAM 标志的 DATA 帧的代理在最后的一个 TCP 段上发送设置了 FIN 位的相关数据。收到设置了 FIN 位的 TCP 段的代理发送设置 END_STREAM 的数据帧。注意最后的 TCP 段或数据帧可以为空。

> A TCP connection error is signaled with RST_STREAM. A proxy treats any error in the TCP connection, which includes receiving a TCP segment with the RST bit set, as a stream error (Section 5.4.2) of type CONNECT_ERROR. Correspondingly, a proxy MUST send a TCP segment with the RST bit set if it detects an error with the stream or the HTTP/2 connection.

一个 TCP 连接错误使用 RST_STREAM 表明。代理把 TCP 连接中的任何错误，包括收到了一个设置 RST 位的 TCP 段当作是 CONNECT_ERROR 类型的流错误 (Section 5.4.2)。同样的，代理必须发送一个设置了 FIN 位的 TCP 段如果检测到了流或者 HTTP/2 连接的任何错误。
