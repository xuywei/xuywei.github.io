---
title: OkHttp使用详解
date: 2016-09-21 09:27:12
categories: android
tags: [Interceptor拦截器]
---
## Interceptor详解
### 什么是Interceptor：
> Interceptor翻译过来就是拦截器，它是OkHttp网络请求中抓取请求和响应必须的一个全能王。

你如果用过okhttp，一定对HttpLoggingInterceptor不陌生，这个是squareup公司写的一个样板，其实它呢也就是告诉你了任何你想拿到的数据。看源码么，go。。。
<!-- more -->
```
public final class HttpLoggingInterceptor implements Interceptor {
  private static final Charset UTF8 = Charset.forName("UTF-8");
//设置拦截级别，枚举4种
  public enum Level {
    NONE,
    BASIC,
    HEADERS,
    BODY
  }
。。。。。。。。。。
。。。。。。。。。。
。。。。。。。。。。
  @Override public Response intercept(Chain chain) throws IOException {
    Level level = this.level;
    Request request = chain.request();
    if (level == Level.NONE) {
      //不拦截，直接返回
      return chain.proceed(request);
    }
    boolean logBody = level == Level.BODY;
    boolean logHeaders = logBody || level == Level.HEADERS;
    RequestBody requestBody = request.body();
    boolean hasRequestBody = requestBody != null;
    //建立连接
    Connection connection = chain.connection();
    //拿到连接协议，如果连接不存在就直接用http_1_1协议
    Protocol protocol = connection != null ? connection.protocol() : Protocol.HTTP_1_1;
    String requestStartMessage = "--> " + request.method() + ' ' + request.url() + ' ' + protocol;
    //如果设置的级别是base，就打印头部分
    if (!logHeaders && hasRequestBody) {
      requestStartMessage += " (" + requestBody.contentLength() + "-byte body)";
    }
    logger.log(requestStartMessage);
    //如果设置的级别是头，就打印头部分
    if (logHeaders) {
      //如果有请求体，就将请求体的长度和类型打印
      if (hasRequestBody) {
        //请求头的值，存在就拦截
        if (requestBody.contentType() != null) {
          logger.log("Content-Type: " + requestBody.contentType());
        }
        //-1代表请求数据长度0
        if (requestBody.contentLength() != -1) {
          logger.log("Content-Length: " + requestBody.contentLength());
        }
      }
      //请求头部分，遍历打印
      Headers headers = request.headers();
      for (int i = 0, count = headers.size(); i < count; i++) {
        String name = headers.name(i);
        // 这里因为上面已经打印了，所以就过滤一下
        if (!"Content-Type".equalsIgnoreCase(name) && !"Content-Length".equalsIgnoreCase(name)) {
          logger.log(name + ": " + headers.value(i));
        }
      }
      //没有请求体，或者等级没有设置为打印请求体，结束打印
      if (!logBody || !hasRequestBody) {
        logger.log("--> END " + request.method())；
      } else if (bodyEncoded(request.headers())) {
        //有请求体或者log等级设置为body，打印请求头中设置的编码
        logger.log("--> END " + request.method() + " (encoded body omitted)");
      } else {
        //将请求体数据给写进缓存流
        Buffer buffer = new Buffer();
        requestBody.writeTo(buffer);
        //设置编码为utf-8
        Charset charset = UTF8;
        MediaType contentType = requestBody.contentType();
        if (contentType != null) {
          charset = contentType.charset(UTF8);
        }

        logger.log("");
      //判断是否是人类可读的字符，是就打印
        if (isPlaintext(buffer)) {
          logger.log(buffer.readString(charset));
          logger.log("--> END " + request.method()
              + " (" + requestBody.contentLength() + "-byte body)");
        } else {
          //人类不懂，就用二进制读出来
          logger.log("--> END " + request.method() + " (binary "
              + requestBody.contentLength() + "-byte body omitted)");
        }
      }
    }

    long startNs = System.nanoTime();
    Response response;
    try {
      //请求开始
      response = chain.proceed(request);
    } catch (Exception e) {
      //请求异常
      logger.log("<-- HTTP FAILED: " + e);
      throw e;
    }
    //请求花费时间
    long tookMs = TimeUnit.NANOSECONDS.toMillis(System.nanoTime() - startNs);
    //响应体了
    ResponseBody responseBody = response.body();
    long contentLength = responseBody.contentLength();
    String bodySize = contentLength != -1 ? contentLength + "-byte" : "unknown-length";
    //打印长度和响应码，响应信息，响应体对应请求体（这里要考虑重定向url），请求耗费时间，响应体长度
    logger.log("<-- " + response.code() + ' ' + response.message() + ' '
        + response.request().url() + " (" + tookMs + "ms" + (!logHeaders ? ", "
        + bodySize + " body" : "") + ')');
    //打印响应头
    if (logHeaders) {
      Headers headers = response.headers();
      for (int i = 0, count = headers.size(); i < count; i++) {
        logger.log(headers.name(i) + ": " + headers.value(i));
      }
       //响应体不存在，等级不为body
      if (!logBody || !HttpHeaders.hasBody(response)) {
        logger.log("<-- END HTTP");
      } else if (bodyEncoded(response.headers())) {
        //响应体编码不对称
        logger.log("<-- END HTTP (encoded body omitted)");
      } else {
        //source，响应体来了
        BufferedSource source = responseBody.source();
        //设置最大缓存大小，当然是缓存整个body喽，全吃
        source.request(Long.MAX_VALUE); 
        Buffer buffer = source.buffer()；
        Charset charset = UTF8;
        //拿到media类型对应的字符集
        MediaType contentType = responseBody.contentType();
        if (contentType != null) {
          try {
            charset = contentType.charset(UTF8);
          } catch (UnsupportedCharsetException e) {
            logger.log("");
            logger.log("Couldn't decode the response body; charset is likely malformed.");
            logger.log("<-- END HTTP");

            return response;
          }
        }
        //如果不是人类能看懂的，就不打印string形式的喽
        if (!isPlaintext(buffer)) {
          logger.log("");
          logger.log("<-- END HTTP (binary " + buffer.size() + "-byte body omitted)");
          return response;
        }
        //是人类懂的，就开始读string喽
        if (contentLength != 0) {
          logger.log("");
          logger.log(buffer.clone().readString(charset));
        }
        //最后打印body的长度
        logger.log("<-- END HTTP (" + buffer.size() + "-byte body)");
      }
    }

    return response;
  }

  /**
   *判断缓存流的数据是否是人类能读懂的，哈哈，也就是abc123呗
   */
  static boolean isPlaintext(Buffer buffer) {
     //先判断是不是123abc能看懂的
    try {
      Buffer prefix = new Buffer();
      long byteCount = buffer.size() < 64 ? buffer.size() : 64;
      buffer.copyTo(prefix, 0, byteCount);
      for (int i = 0; i < 16; i++) {
        if (prefix.exhausted()) {
          break;
        }
        //再判断是不是iso8859-1之类的，当然不是能懂的啊
        int codePoint = prefix.readUtf8CodePoint();
        if (Character.isISOControl(codePoint) && !Character.isWhitespace(codePoint)) {
          return false;
        }
      }
      return true;
    } catch (EOFException e) {
      return false; // Truncated UTF-8 sequence.
    }
  }
  //返回头中的编码是否存在并且不是identity，一般都是gzip,deflate,compress之中一个
  private boolean bodyEncoded(Headers headers) {
    String contentEncoding = headers.get("Content-Encoding");
    return contentEncoding != null && !contentEncoding.equalsIgnoreCase("identity");
  }
}

```
好了终于撸了一遍源码，相信还有很多朋友没有看懂，解释这些源码干嘛啊，另外推荐一篇文章，大家踊跃去看吧(感谢[ychongjie](http://home.cnblogs.com/u/yuanchongjie/)的拦截器细致分析)：
 [链接直通车](http://www.cnblogs.com/yuanchongjie/p/4969326.html?utm_source=tuicool&utm_medium=referral)
> 没有太多时间更新和维护，有什么不妥的请指出，匆忙的成果，毕竟公司团队项目你不做其他人多做，多少有点坑队友，dota骨灰级玩家怎么能做出这种事情呢，不定期更新中。。。