商业版项目组成由以上三个子项目以及数据库文件

(1)后端IceWK-ment项目
![alt icecms](https://img.kancloud.cn/a4/3f/a43f8a044051168614c683e459a1976f_1198x953.png)
使用数据库可视化工具navicat或者sqlyog等软件创建数据库

然后导入数据库sql文件。另外，注意要按照文档中 “常见问题” 栏目 第13条的要求执行对应SQL，清理quartz表数据。否则初次启动后端项目会报错。

注意：如果还是报错，再参考文档中 “常见问题” 栏目 第3条的要求配置数据库。

在后端yml中选择激活dev本地或prod线上配置

注意：使用v1.4版本以下的话，按照yml说明在yml文件中配置对应的配置项。

使用v1.4及以上版本的话，这些配置都在后台管理系统的“配置中心”进行自定义配置。

注意:像数据库和redis配置这些都还在yml中进行。

按照yml配置数据库名称端口和账号密码，生产环境配置application-prod.yml

redis配置项在这里，默认不用改
运行后端api前务必启动redis和mysql。

mvn clean和install即可打成jar包，可运行jar就在targer文件夹目录下。
如果脱离IDEA，需要单独运行jar，执行

java -jar project-fast.jar --spring.profiles.active=dev即可

启动后端项目之前，最重要的事情是一定要先启动redis服务，再启动后端服务。否则后端无法启动，因为后端启动会初始化各种配置项至redis缓存中。

(2)移动端linfeng-community-uniapp项目


找到utils/config.js配置api接口地址

修改小程序appid（如果你要用小程序版本就一定要改这里！！）

h5的腾讯地图key改成你自己的，不然h5端无法定位
申请key的网址（ https://lbs.qq.com/dev/console/application/mine ）

按上图所示即可运行小程序，如果你是第一次运行uniapp项目到微信开发者工具，

需要去配置一下微信开发者工具的安全端口，不然无法唤起微信开发者工具（具体百度看博客示例参考 https://blog.csdn.net/lyx1838102537/article/details/122491185 ）。
uniapp端运行到小程序以后不要着急微信登录，这时会报错“openid解析失败”，因为你的后台管理系统还需要配置小程序appid和密钥。不然无法登录。

在V1.5.0版本中引入了uniAD广告模块

申请地址：https://uniad.dcloud.net.cn/login
注意：如果你的小程序没有申请广告插件就打开这两处注释运行，是不能运行成功的。系统会提示你申请插件。不需要广告模块的话，这两块注释掉即可。

如何获取广告的adpid,请仔细阅读：https://ask.dcloud.net.cn/article/36769
申请会花几天时间才能通过审核。

(3)后台管理端linfeng-community-vue项目


这是开发环境配置端口的地方

这是生产环境接口地址配置地方

安装依赖有的用户可能会报错，报错了按报错日志要求安装其他需要的本机依赖。

依赖安装后运行npm run dev即可启动项目
初始账号：admin 密码：123456

如果你使用V1.4以下版本，而且使用的是小程序，那这里一定记得要配置！！
如果你使用V1.4及以上版本，配置项统一在“配置中心”配置。

图片存储使用oss，这里选择你的图片存储类型并进行配置，
如果你选择阿里云存储，需要你在如下图所示的uniapp项目位置进行修改，不然视频截图无法获取。

在v1.4.1中，新增了邮箱验证码登录，如果需要该功能，需要在后端项目application.yml中配置参数，如下图a所示

这里的password参数是指 邮箱授权码，而不是你的qq密码。
图b

注意：yml中一共有两处邮箱需要配置！一个是图a，一个如上图b的email.send处！！！（很多客户反应配置不成功，其实就是漏了如上图b的email.send配置）

那么如何找到图a中的配置参数？

首先，我们需要打开QQ邮箱的SMTP服务，因为QQ邮箱对于一般的用户都是默认关闭SMTP服务的。

打开QQ邮箱，点击设置。点击帐户。

找到SMTP服务的选项，可以看到此处默认是关闭的，点击开启，然后腾讯会进行一些身份验证，身份验证通过以后，腾讯会给出一个用于使用SMTP的16位口令，此处这个口令就是yml要填写的password参数。