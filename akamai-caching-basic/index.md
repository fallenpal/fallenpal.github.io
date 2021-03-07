# Akamai Caching Behaviors


一些比较特殊的状态码的缓存规则笔记。
<!--more-->

# Akamai Caching Behaviors

## HTTP Status Codes

### 302
A `302` Moved Temporarily is cached depending on whether or not it contains a Cache-Control or Expires header. If neither of these headers is present in the response, the `302` is treated as uncacheable. If either of these headers is present, then the response is considered cacheable. Cacheable 302s receive the same time-to-live that a 200 OK response receives.

`301状态码`：默认情况下，Edge会使用对待200状态码的做法，处理301

`302状态码`：取决于源站响应是否包含`Cache-control` 或 `Expire` 头。如果两者都不包含，则不缓存。否则使用对待200状态码的做法。如果配置了metadata: `cache:cache-302`， 则不管是否存在这两个头，都会缓存该内容。

### 403
默认是不缓存`403`的，仅当手动开启`cache:cache-403` 后，才会缓存403。

## Negatively Cache

### nagtive-ttl
Akamai Edge servers will cache "negative" responses, which include the `204`, `305`, `404`, `405`, and `501` response codes, received from the origin server for 10 seconds by default unless otherwise explicitly stated or customized.

Akamai 默认情况下，缓存以下“错误码“，TTL为10秒：`204`, `305`, `404`, `405`, and `501`。ACC内的选项为 `Cache HTTP error responses`, metadata内为 `<cache:negative-ttl>`

除了`<cache:negative-ttl>`以外，metadata还提供了第二个选项`<cache:negative-ttl2>`，用于自定义某些状态码的缓存时间。比如：
```xml
<match:response.status value=”404”> <cache:negative-ttl2>
      <status>on</status>
<value>1d</value> </cache:negative-ttl2>
</match:response.status>
```

### 保留stale cache
`cache:preserve-stale-objects`这个选项使得Akamai从源站收到 `400` , `500` , `502` , `503` , `504` 时，会使用stale的缓存答复客户。该选项默认为打开的。

因此，默认情况下，Akamai不会缓存 `400` , `500` , `502` , `503` , `504` 这些状态码，除非把`cache:preserve-stale-objects` 设置为off。


