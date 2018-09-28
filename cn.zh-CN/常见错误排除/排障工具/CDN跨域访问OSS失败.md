# CDN跨域访问OSS失败 {#concept_r4n_ztj_gfb .concept}

## 使用背景 {#section_mth_bg3_hfb .section}

当您使用遇到 CDN 回源到 OSS 时出现跨域问题时，可以尝试先用此脚本进行问题的初步排查和定位，使用之前请安装 pip install oss2 模块。

## 使用方法 {#section_c41_gg3_hfb .section}

```
[root@izwx4z api]# python AnalysisCore.py -h
Usage: AnalysisCore.py [options]  都是必选项

Options:
  -h, --help  show this help message and exit
  -i AK       <Accesskey>  阿里云的云账号或者具备操作 OSS 权限的 RAM 子账号，Accesskey
  -k SK       <AccessKeySecrety>  阿里云的云账号或者具备操作 OSS 权限的 RAM 子账号，Accesskey Secrety
  -e ED       dest oss <endpoint>  OSS endpoint，可以从控制台上获取
  -b BK       dest oss <bucket>  OSS bucket
  -s CDN      cors allow <domain>  跨域访问 OSS 的 CDN 域名，必须携带协议头 HTTPS/HTTP
  -m METH     cors http method <GET,POST,PUT,HEADER>  cors http method <GET,POST,PUT,HEADER>  指定跨域访问的 HTTP 请求方式，多个请求方法请用 "," 隔开
  -t TYPES    cors file type multiparts operate or single operate <multi,single>  操作 OSS 的方式是单一文件 (single) 上传/下载，还是大文件文件分片 (multi) 上传/下载
```

-   第一种情况：

    `python cors.py -i LTA43W -k VTqIcMlVNc -e oss-cn-beijing.aliyuncs.com -b yourBucket -s http://www.zhangyb.com -t multi -m POST`

    OSS Cors 设置 Access-Control-Allow-Origin Config 没有携带跨域头

    OSS 是分片 上传/下载 操作，但是没有配置暴露 Headers，<Etag|etag|\*\>

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21772/153814937313182_zh-CN.png)

-   第二种情况：

    `python cors.py -i LTA43W -k VTqIcMlVNc -e oss-cn-beijing.aliyuncs.com -b yourBucket -s http://www.zhangyb.com -t multi -m POST,DELETE,HEADE`

    CDN 回源到 OSS 的跨域请求方式没有在 OSS Cors Access-Control-Allow-Methods 中进行配置。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21772/153814937313181_zh-CN.png)

-   第三种情况：

    `python cors.py -i LTA43W -k VTqIcMlVNc -e oss-cn-beijing.aliyuncs.com -b yourBucket -s http://www.zhangyb.com -t multi -m POST,DELETE,HEADE`

    上传/下载 大文件操作时，如果是分片（multipart） 请求，需要暴露 ETag 头。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21772/153814937313183_zh-CN.png)

-   第四种情况：

    `python cors.py -i LTA43W -k VTqIcMlVNc -e oss-cn-beijing.aliyuncs.com -b yourBucket -s http://www.zhangyb.com,http://www.taobao.com -t multi -m POST,DELETE,HEADE`

    跨域的域名没有在 OSS Cors Access-Control-Allow-Origin 进行配置。


## 源码 {#section_pfv_fh3_hfb .section}

```
#-*- coding:utf8 -*-
#!/usr/bin/env python
#Author: hanli
#Update: 2018-09-22

from optparse import OptionParser
import collections
import oss2
import sys
import re

class MainFunction():

  def __init__(self,options):

    self.args = collections.OrderedDict()
    self.args["ak"] = options.ak
    self.args["sk"] = options.sk
    self.args["ed"] = options.ed
    self.args["bk"] = options.bk
    self.args["cdn"] = options.cdn
    self.args["types"] = options.types
    self.args["meth"] = options.meth
    self.left = '\033[1;31;40m'
    self.right = '\033[0m'

  # check input parse

  def CheckParse(self):

    try:

      for keys in self.args:
        if self.args[keys] == None:
          self.ConsoleLog(0)
          return False

      if not (self.args['ed'].startswith("http://") or self.args['ed'].startswith("https://")):
       self.args['ed'] = "http://"+self.args['ed']

      if not (self.args['cdn'].startswith("http://") or self.args['cdn'].startswith("https://")):
        self.ConsoleLog(1)
        return False

      if not re.match("(GET|POST|PUT|HEAD|DELETE|ORIGIN)",self.args['meth']):
        self.ConsoleLog(7)
        return False

      if not re.match("multi|single",self.args['types']):
        self.ConsoleLog(2)
        return False
    
    except Exception as e:
      self.ConsoleLog(e)

    return True

  # valid cors config 

  def ValidConf(self,origins,methods,headers,expose,flag=True):

    try:

      if not (re.match(self.args['cdn'],origins) or origins=="*"):
        flag=False
        self.ConsoleLog(3)
 
      for meth in self.args['meth'].split(","):
        if not (meth in methods.split(",") or methods=="*"):
          flag=False
          self.ConsoleLog(4,meth)

      if self.args['types'] == "multi":
        if not (re.match("(etag|Etag| )",expose)):
          flag=False
          self.ConsoleLog(6)

      if headers == None:
        flag=False
        self.ConsoleLog(5)

    except Exception as e:
      self.ConsoleLog(e) 

    return flag

  # get oss cors config

  def GetCors(self):

    try:
      auth = oss2.Auth(self.args['ak'], self.args['sk'])
      bucket = oss2.Bucket(auth, self.args['ed'], self.args['bk'])
      cors = bucket.get_bucket_cors()
    except oss2.exceptions.ServerError as e:
      self.ConsoleLog(e)
    except oss2.exceptions.NoSuchCors as e:
      self.ConsoleLog(e)
    except oss2.exceptions.NoSuchBucket as e:
      self.ConsoleLog(e)
    except oss2.exceptions.AccessDenied as e:
      self.ConsoleLog(e)
    else:
      for rule in cors.rules:
        if (self.ValidConf(",".join(rule.allowed_origins),",".join(rule.allowed_methods),
        ",".join(rule.allowed_headers),",".join(rule.expose_headers))):
          self.ConsoleLog(8)

  # output error log

  def ConsoleLog(self,level,meth=None):

    if level == 0:
      print('{0}[ERROR]{1}All parameters cannot be empty'.format(self.left,self.right))
      
    if level == 1:
      print('{0}[ERROR]{1}<-s> CDN domain must be http|https start'.format(self.left,self.right))

    if level == 2:
      print('{0}[ERROR]{1}<-t> Must be multi|single'.format(self.left,self.right))
    
    if level == 3:
      print('{0}[CheckResult]{1}OSS Access-Control-Allow-Origin Config Error {2}, You can fill in * or http://domain'.format(self.left,self.right,self.args['cdn']))

    if level == 4:
      print('{0}[CheckResult]{1}OSS Access-Control-Allow-Methods Config Error {2}, You can fill in * or methods'.format(self.left,self.right,meth))

    if level == 5:
      print('{0}[CheckResult]{1}Access-Control-Allow-Headers OSS Config Error, You can fill in * or headers'.format(self.left,self.right))

    if level == 6:
      print('{0}[CheckResult]{1}Multipart operate must set expose <etag|Etag|*>'.format(self.left,self.right))

    if level == 7:
      print('{0}[ERROR]{1}<-m> Must be <GET|POST|PUT|HEAD|DELETE|ORIGIN> ,multiple method please "," split'.format(self.left,self.right))

    if level == 8:
      sys.exit("[CheckResult]OSS CORS Config Alright OK")

    if not isinstance(level,int):
      print(level)

#      expr1 = lambda x:x if x.startswith("http://") or x.startswith("https://") else "https://"+x
#      expr2 = lambda y:y if y.startswith("http://") or y.startswith("https://") else "https://"+y

if __name__ == "__main__":

  parser = OptionParser()
  parser.add_option("-i",dest="ak",help="<Accesskey>")
  parser.add_option("-k",dest="sk",help="<AccessKeySecrety>")
  parser.add_option("-e",dest="ed",help="dest oss <endpoint>")
  parser.add_option("-b",dest="bk",help="dest oss <bucket>")
  parser.add_option("-s",dest="cdn",help="cors allow <domain>")
  parser.add_option("-m",dest="meth",help="cors http method <GET,POST,PUT,HEADER>")
  parser.add_option("-t",dest="types",help="cors file type multiparts operate or single operate<multi,single>")
  (options, args) = parser.parse_args()
  handler = MainFunction(options)

  if handler.CheckParse():
    handler.GetCors()
```

