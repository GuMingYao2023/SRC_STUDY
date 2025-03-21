

### HTML类型+SVG类型

#### HTML

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\html类型+svg类型-xss.docx"
```

上传一张图片，改为1.html,在内容中加 <script>alert(111)</script>,发送后访问地址，有些要删除boundary才可以绕过。

#### SVG类型

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\html类型+svg类型-xss.docx"
```

上传svg文件，用下面的POC百分之80可以绕过：



```xml
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
   <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
   <script type="text/javascript">
      javascript:top[`al`+`ert`](document.cookie);
   </script>
</svg>

```

#### 低危的pdf-xss也是同上

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\xss.pdf"
```

笔者认为是前端验证，不严格导致的

### 恶意登出（未成功）

具体原理：

将登出的数据包拼接到上传的图像参数上，当用户访问时，会自动登出

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\恶意退出登录.docx"
```

![image-20250321093020609](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321093020609.png)

![image-20250321093023997](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321093023997.png)

![image-20250321093026759](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321093026759.png)

### 薅羊毛

原理：重复注册注销即可

### 两个页面未授权

打开两个同一个页面，在其中一个页面登出，若另一个页面未退出，则存在此漏洞

### 简单的csrf

如果没有Referer字段和token，那么极有可能存在CSRF漏洞。

先找到可以修改信息的页面，然后抓取保存包，生成csrf POC ,新建txt,粘贴进去，改名为html,如果访问成功，即存在漏洞

![image-20250321100841849](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321100841849.png)

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\普通csrf.docx"
```

### 前端绕过

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\前端绕过.docx"
```

具体原理是先抓取正确的相应包，然后通过修改响应包来进行绕过

```
HTTP/1.1 200 OK
Date: Fri, 21 Mar 2025 02:35:41 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 47
Connection: keep-alive
Cache-Control: private
backend: 150_41

{"StatusCode":0,"Message":"验证码正确！"}
```

下面是错误的：

```
HTTP/1.1 200 OK
Date: Fri, 21 Mar 2025 02:37:49 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 47
Connection: keep-alive
Cache-Control: private
backend: 150.40

{"StatusCode":1,"Message":"验证码错误！"}
```

修改即可绕过

![image-20250321103910379](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321103910379.png)

### 任意验证码提交

随便输入验证码提交即可

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\任意验证码提交.docx"
```

### 任意用户注册

![image-20250321104426950](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321104426950.png)

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\任意注册-修复了看看就好.docx"
```

例如验证码注册时，输入正确的验证码后，将手机号/邮箱改了也能注册成功

### 跳转漏洞

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\跳转漏洞.docx"
```

一般在登录界面，观察url中是否存在类似 ?url= 或者 ?location= 等等。将后面的地址换成钓鱼页面登录后就可以跳转

![image-20250321105617260](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321105617260.png)

![image-20250321105621617](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321105621617.png)

### 隐私合规

小米商城app-存在隐私合规漏洞，它这里收集你身份证没有隐私政策，默认勾选

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\隐私合规漏洞没有隐私政策.docx"
```

详情见如下：

```
1、默认勾选
2、第一次进入没有弹窗隐私政策
5、第一次进入app点拒绝如果没有二次确认也是违规
3、弹窗没有同意和不同意选项
4、app或小程序没有注销功能
6、如果用户点击定位功能要，获取手机号或者相册或其他也是违规，需求原则定位就是定位不能获取多余与该功能点不匹配信息
7、点击一个功能弹窗获取定位，如果没有告知获取定位干嘛-违规
8、点击拒绝还是一直弹窗
9、如果没有根据需求获取信息-违规，如：定位要获取你相册
10、小程序没有注销功能
11、一揽子弹窗-违规，获取身份证-定位-相册-地址，一次在没有需求场景要求多个信息
12、推送推销个性化，如果没有关闭或拒绝的按钮，-违规
13、涉及投诉，承诺时间不得超过15个工作日，超过违规
14、网络游戏要有未成年人政策，和防沉迷，视频、网络文学要有青少年模式
15、app频繁自动启动或联动启动-违规
16、没有说明打开摄像头的目的
17、隐私合规政策出现错别字
18、隐私政策说可以选填，但打开必须要填-违规
19、隐私政策没有日期-违规
20、实名验证处没有隐私政策
	

收录隐私合规的公司：
麦当劳、爱奇艺、途虎、小米、金山，顺丰
```

### 用户名占用或修改

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\用户名占用或接管.docx"
```

![image-20250321110840376](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321110840376.png)

### JSONP劫持(未成功)

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\JSONP劫持-看看就好这种洞很少.docx"
```

利用POCBOX生成EXP

### 短信轰炸

1：有很多产品，点了第一个，再点第二个，第三个。。。。。

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\短信轰炸.docx"
```

2：开多个窗口同时发送验证码

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\短信轰炸2.docx"
```

3：BP抓包，将某一个参数改为其它值

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\短信轰炸绕过这个修复了看看就好.docx"
```

![image-20250321112124577](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321112124577.png)

4：一直重复下单造成短信轰炸

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\短信轰炸之重复下单.docx"
```

![image-20250321112225185](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321112225185.png)

5：短信验证码绕过：大小写绕过，空格绕过

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\邮箱轰炸绕过.docx"
```

### 越权

修改为别人的uid...

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\越权.docx"
```

在找回密码界面

![image-20250321112833464](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321112833464.png)

### 删除验证码绕过注册

注册时删除验证码即可注册

![image-20250321112939218](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321112939218.png)

### 删除cookie访问未授权（未成功）

```
"C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\删除cookie未授权查看订单.docx"
```

### 上传文件的相应包导致的信息泄露

![img](file:///C:/Users/Gu/Desktop/第一节课/第一节课/第一节课/信息泄露：可能修复了看一下知道思路就好/91ce215ce6f0534f6e64c1f8019510ef.jpg)

![0a7860641f81163b204e75003582462b](C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\信息泄露：可能修复了看一下知道思路就好\0a7860641f81163b204e75003582462b.jpg)

![58e7d0c9f660526268a8a75dd6404731](C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\信息泄露：可能修复了看一下知道思路就好\58e7d0c9f660526268a8a75dd6404731.jpg)

![dc4776035a1aa9ebb96b237eaa9e79f4](C:\Users\Gu\Desktop\第一节课\第一节课\第一节课\信息泄露：可能修复了看一下知道思路就好\dc4776035a1aa9ebb96b237eaa9e79f4.jpg)

### IP伪造（如何进行XSS？不被允许CSRF？）

适用于可以查看登录日志的系统

加上

```
X-Forwarded-For:127.0.0.2"<h1>aaa</h1>
```

登陆后可以看到登录日志被修改为127.0.0.2（后面的弹窗可能未执行，进不去说明有过滤）

### sessionkey任意注册和任意登录

主要是微信小程序，看看http包有无sessionkey,没有就打不了

利用工具解密

![image-20250321132012272](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321132012272.png)

### webpack未打包好造成的未授权

去js里面搜搜有无js.map,访问原地之后进行反编译即可，

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\webpack没有打包好造成的未授权.docx"
```

命令：

```
reverse-sourcemap -v xxx.js.map -o outou
```

### 重复点赞（未成功）

抓取到包后进入repeater板块，在cookie（任意文件头）中随便加一些内容

去并发或者一直发送来实现点赞

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\点赞之成为榜一绕过.docx"
```

### 短信轰炸新奇思路

重复下单重复取消就造成短信轰炸

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\短信轰炸新奇思路-修复了看看就好.docx"
```

### 耗光库存

原理是有些逻辑问题：正常是付完款后才会占用，但有些是下单就占用

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\耗光库存.docx"
```

### 逻辑漏洞占用资源

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\逻辑漏洞占用资源-修复了看看就好.docx"
```

例如预约，取号，抓到数据包之后一直放包，会导致资源被占用

### 扫码登录漏洞

公众号多，有些二维码漏洞不需要二次登录，直接扫就可以进去，可以拿来钓鱼

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\扫码登录漏洞.docx"
```

### 图片钓鱼连接

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\图片替换钓鱼链接钓鱼.docx"
```

![image-20250321150814102](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321150814102.png)

### 实名验证资源耗用

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\实名验证处资源消耗.docx"
```

抓包后一直放就行了

![image-20250321150928799](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321150928799.png)

### SWARKS邮箱伪造

### 微信扫码绑定账号接管（未成功，无code）

扫码抓包，生成CSRF直接绑定

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\微信扫码账号接管.docx"
```

或者复制 url 连接，全部放包后到另一个浏览器里面复制访问

### 信息泄露（电话号码当作用户名）

![image-20250321153000176](C:\Users\Gu\AppData\Roaming\Typora\typora-user-images\image-20250321153000176.png)

对1428爆破

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\信息泄露.docx"
```

### 用户拥有两个邮箱

去个人中心绑定两次不同邮箱，一般来说绑定第二个邮箱第一个邮箱就会解除绑定，但这里不会，绑定完两个邮箱之后，退出登录，用第一次绑定的邮箱进行登录，会发现可以登录

```
"C:\Users\Gu\Desktop\第二节课\视频只是演示主要复现一次里面的文档\账号接管之邮箱接管.docx"
```

