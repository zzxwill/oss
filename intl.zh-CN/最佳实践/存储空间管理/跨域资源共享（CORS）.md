# 跨域资源共享（CORS） {#concept_vb1_2vd_vdb .concept}

## 同源策略 {#section_wyh_fvd_vdb .section}

跨域访问，或者说 JavaScript 的跨域访问问题，是浏览器出于安全考虑而设置的一个限制，即同源策略。举例说明，当 A，B 两个网站属于不同的域时，如果来自于 A 网站的页面中的 JavaScript 代码希望访问 B 网站的时候，浏览器会拒绝该访问。

然而，在实际应用中，经常会有跨域访问的需求。比如用户的网站 www.a.com，后端使用了 OSS，在网页中提供了使用 JavaScript 实现的上传功能，但是在该页面中，只能向 www.a.com 发送请求，向其他网站发送的请求都会被浏览器拒绝。这样会导致用户上传的数据必须从www.a.com 中转。如果设置了跨域访问的话，用户就可以直接上传到 OSS 而无需从 www.a.com 中转。

## CORS 介绍 {#section_gz5_gvd_vdb .section}

跨域资源共享（Cross-Origin Resource Sharing，简称 CORS），是 HTML5 提供的标准跨域解决方案，具体的CORS规则可以参考 [W3C CORS规范](http://www.w3.org/TR/cors/)。

CORS 是一个由浏览器共同遵循的一套控制策略，通过HTTP的Header来进行交互。浏览器在识别到发起的请求是跨域请求的时候，会将Origin的Header加入HTTP请求发送给服务器，比如上面那个例子，Origin的Header就是www.a.com。服务器端接收到这个请求之后，会根据一定的规则判断是否允许该来源域的请求，如果允许的话，服务器在返回的响应中会附带上Access-Control-Allow-Origin这个Header，内容为www.a.com来表示允许该次跨域访问。如果服务器允许所有的跨域请求的话，将Access-Control-Allow-Origin的Header设置为\*即可，浏览器根据是否返回了对应的Header来决定该跨域请求是否成功，如果没有附加对应的Header，浏览器将会拦截该请求。

以上描述的仅仅是简单情况，CORS规范将请求分为两种类型，一种是简单请求，另外一种是带预检的请求。预检机制是一种保护机制，防止资源被本来没有权限的请求修改。浏览器会在发送实际请求之前先发送一个OPTIONS的HTTP请求来判断服务器是否能接受该跨域请求。如果不能接受的话，浏览器会直接阻止接下来实际请求的发生。

只有同时满足以下两个条件才不需要发送预检请求：

-   请求方法是如下之一：

    -   GET
    -   HEAD
    -   POST
-   所有的Header都在如下列表中：

    -   Cache-Control
    -   Content-Language
    -   Content-Type
    -   Expires
    -   Last-Modified
    -   Pragma

预检请求会附带一些关于接下来的请求的信息给服务器，主要有以下几种：

-   Origin：请求的源域信息
-   Access-Control-Request-Method：接下来的请求类型，如POST、GET等
-   Access-Control-Request-Headers：接下来的请求中包含的用户显式设置的Header列表

服务器端收到请求之后，会根据附带的信息来判断是否允许该跨域请求，信息返回同样是通过Header完成的：

-   Access-Control-Allow-Origin：允许跨域的Origin列表
-   Access-Control-Allow-Methods：允许跨域的方法列表
-   Access-Control-Allow-Headers：允许跨域的Header列表
-   Access-Control-Expose-Headers：允许暴露给JavaScript代码的Header列表
-   Access-Control-Max-Age：最大的浏览器缓存时间，单位s。

浏览器会根据以上的返回信息综合判断是否继续发送实际请求。当然，如果没有这些Header浏览器会直接阻止接下来的请求。

**说明：** 这里需要再次强调的一点是，以上行为都是浏览器自动完成的，用户无需关注这些细节。如果服务器配置正确，那么和正常非跨域请求是一样的。

CORS主要使用场景

CORS使用一定是在使用浏览器的情况下，因为控制访问权限的是浏览器而非服务器。因此使用其它的客户端的时候无需关心任何跨域问题。

使用CORS的主要应用就是在浏览器端使用Ajax直接访问OSS的数据，而无需走用户本身的应用服务器中转。无论上传或者下载。对于同时使用OSS和使用Ajax技术的网站来说，都建议使用CORS来实现与OSS的直接通信。

## OSS对CORS的支持 {#section_b32_nvd_vdb .section}

OSS提供了CORS规则的配置从而根据需求允许或者拒绝相应的跨域请求。该规则是配置在Bucket级别的。详情可以参考[PutBucketCORS](../intl.zh-CN/API 参考/跨域资源共享/PutBucketcors.md#)。

CORS请求的通过与否和OSS的身份验证等是完全独立的，即OSS的CORS规则仅仅是用来决定是否附加CORS相关的Header的一个规则。是否拦截该请求完全由浏览器决定。

使用跨域请求的时候需要关注浏览器是否开启了Cache功能。当运行在同一个浏览器上分别来源于www.a.com和www.b.com的两个页面都同时请求同一个跨域资源的时候，如果www.a.com的请求先到达服务器，服务器将资源带上Access-Control-Allow-Origin的Header为www.a.com返回给用户。这个时候www.b.com又发起了请求，浏览器会将Cache的上一次请求返回给用户，这个时候Header的内容和CORS的要求不匹配，就会导致后面的请求失败。

**说明：** 目前OSS的所有Object相关的接口都提供了CORS相关的验证，另外Multipart相关的接口目前也已经完全支持CORS验证。

## GET请求跨域示例 {#section_tj2_rvd_vdb .section}

这里举一个使用Ajax从OSS获取数据的例子。为了简单起见，使用的Bucket都是public，访问私有的Bucket的CORS配置是完全一样的，只需要在请求中附加签名即可。

简单开始

首先创建一个Bucket，这里举例为oss-cors-test，将权限设置为public-read，然后这里创建一个test.txt的文本文档，上传到这个Bucket。

单击[此处](http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/test.txt?spm=a2c4g.11186623.2.6.cdVyuR&file=test.txt)，获取test.txt的访问地址。

**说明：** 请将下文中的地址换成自己试验的地址。

首先用curl直接访问：

```
curl http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/test.txt
just for test
```

可见现在已经能正常访问了。

那么我们现在再试着使用Ajax技术来直接访问这个网站。我们写一个最简单的访问的HTML，可以将以下代码复制到本地保存成html文件然后使用浏览器打开。因为没有设置自定义的头，因此这个请求是不需要预检的。

```
<!DOCTYPE html> 
<html>
<head>
<script type="text/javascript" src="./functions.js"></script>
</head>
<body>
<script type="text/javascript">
// Create the XHR object.
function createCORSRequest(method, url) {
  var xhr = new XMLHttpRequest();
  if ("withCredentials" in xhr) {
    // XHR for Chrome/Firefox/Opera/Safari.
    xhr.open(method, url, true);
  } else if (typeof XDomainRequest != "undefined") {
    // XDomainRequest for IE.
    xhr = new XDomainRequest();
    xhr.open(method, url);
  } else {
    // CORS not supported.
    xhr = null;
  }
  return xhr;
}
// Make the actual CORS request.
function makeCorsRequest() {
  // All HTML5 Rocks properties support CORS.
  var url = 'http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/test.txt';
  var xhr = createCORSRequest('GET', url);
  if (!xhr) {
    alert('CORS not supported');
    return;
  }
  // Response handlers.
  xhr.onload = function() {
    var text = xhr.responseText;
    var title = text;
    alert('Response from CORS request to ' + url + ': ' + title);
  };
  xhr.onerror = function() {
    alert('Woops, there was an error making the request.');
  };
  xhr.send();
}
</script>
<p align="center" style="font-size: 20px;">
<a href="#" onclick="makeCorsRequest(); return false;">Run Sample</a>
</p>
</body>
</html>
```

打开之后点击链接（这里以Chrome为例），当然，肯定会失败：

利用Chrome的开发者工具来查看错误原因。

这里告诉我们是因为没有找到Access-Control-Allow-Origin这个头。显然，这就是因为服务器没有配置CORS。

再进入Header界面，可见浏览器发送了带Origin的Request，因此是一个跨域请求，在Chrome上因为是本地文件所以Origin是null。

设置 Bucket CORS

知道了上文的问题，那么现在可以设置Bucket相关的CORS来使上文的例子能够执行成功。为了直观起见，这里使用控制台来完成CORS的设置。如果用户的CORS设置不是很复杂，也建议使用控制台来完成CORS设置。CORS设置的界面如下：

CORS设置是由一条一条规则组成的，真正匹配的时候会从第一条开始逐条匹配，以最早匹配上的规则为准。现在添加第一条规则，使用最宽松的配置：

这里表示的意思就是所有的Origin都允许访问，所有的请求类型都允许访问，所有的Header都允许，最大的缓存时间为1s。

配置完成之后重新测试，结果如下：

可见现在已经可以正常访问请求了。

因此排查问题来说，如果想要排除跨域带来的访问问题，可以将CORS设置为上面这种配置。这种配置是允许所有的跨域请求的一种配置，因此如果这种配置下依然出错，那么就表明错误出现在其他的部分而不是CORS。

除了最宽松的配置之外，还可以配置更精细的控制机制来实现针对性的控制。比如对于这个场景可以使用如下最小的配置匹配成功：

因此对于大部分场景来说，用户最好根据自己的使用场景来使用最小的配置以保证安全性。

## 利用跨域实现POST上传 {#section_md4_kzd_vdb .section}

这里提供一个更复杂的例子，这里使用了带签名的POST请求，而且需要发送预检请求。

[PostObjectSample](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/postobject.tar.gz)

**说明：** 将上面的代码下载下来之后，将以下对应的部分全部修改成自己对应的内容，然后使用自己的服务器运行起来。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4409/1670_zh-CN.png)

这里继续使用上文oss-cors-test这个Bucket来进行测试，在测试之前先删除所有的CORS相关的规则，恢复初始状态。

访问这个网页，选择一个文件上传，结果如下：

同样打开开发者工具，可以看到以下内容，根据上面的Get的例子，很容易明白同样发生了跨域的错误。不过这里与Get请求的区别在于这是一个带预检的请求，所以可以从图中看到是OPTIONS的Response没有携带CORS相关的Header然后失败了。

那么我们根据对应的信息修改Bucket的CORS配置：

再重新运行，可见已经成功了，从控制台中也可以看到新上传的文件。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4409/1675_zh-CN.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4409/1676_zh-CN.png)

测试一下内容：

```
$curl http://oss-cors-test.oss-cn-hangzhou.aliyuncs.com/events/1447312129218-test1.txt
post object test
```

CORS配置的一些注意事项

CORS配置一共有以下几项：

-   来源：配置的时候要带上完整的域信息，比如示例中 `http://10.101.172.96:8001`.

    注意不要遗漏了协议名如http，如果端口号不是默认的还要带上端口号之类的。如果不确定的话，可以打开浏览器的调试功能查看Origin头。这一项支持使用通配符\*，但是只支持一个，可以根据实际需要灵活配置。

-   Method：按照需求开通对应的方法即可。
-   Allow Header：允许通过的Header列表，因为这里容易遗漏Header，因此建议没有特殊需求的情况下设置为\*。大小写不敏感。
-   Expose Header：暴露给浏览器的Header列表，不允许使用通配符。具体的配置需要根据应用的需求来选择，只暴露需要使用的Header，如ETag等。如果不需要暴露这些信息，这里可以不填。如果有特殊需求可以单独指定，大小写不敏感。
-   缓存时间：如果没有特殊情况可以酌情设置的大一点，比如60s。

因此，CORS的一般配置方法就是针对每个可能的访问来源单独配置规则，尽量不要将多个来源混在一个规则中，多个规则之间最好不要有覆盖冲突。其他的权限只开放需要的权限即可。

一些错误排查经验

在调试类似程序的时候，很容易发生一种情况就是将其他错误和CORS错误弄混淆了。

举个例子，当用户因为签名错误访问被拒绝的时候，因为权限验证发生在CORS验证之前，因此返回的结果中并没有带上CORS的Header信息，有些浏览器就会直接报CORS出错，但是实际服务器端的CORS配置是正确的。对于这种情况，我们有两种方式：

-   查看HTTP请求的返回值，因为CORS验证是一个独立的验证，并不影响主干流程，因此像403之类的返回值不可能是由CORS导致的，需要先排除程序原因。如果之前发送了预检请求，那么可以查看预检请求的结果，如果预检请求中返回了正确的CORS相关的Header，那么表示真实请求应该是被服务器端所允许的，因此错误只可能出现在其他的方面。

-   将服务器端的CORS设置按照如上文所示，改成允许所有来源所有类型的请求都通过的配置，再重新验证。如果还是验证失败，表明有其他的错误发生。


