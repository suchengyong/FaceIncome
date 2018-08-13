# FaceIncome
小程序颜值计算器

本款小程序纯属娱乐，里面的评分笑笑就好。

本项目主要实现了以人脸检测API调用的面部识别功能，开发过程中有几个坑点和处理方式记录下:

1. 小程序接口几乎都是异步处理，所以代码的回调非常多，不怕麻烦的用promise包装下，写起来好看点。
2. 因为国内某些可用的图床被微信封掉了，所以图片不能通过URL的方式传入API，只能在客服端BASE64处理再上传，如果谁有自己的图床可以改为uploadfile的方式，代码中注释掉了这个函数，使用这种方式就不需要进行下面的图片压缩了
3. 功能里主要的图片上传需要做压缩处理，不然图片过大，请求时间会很长，用户等待时间过长，体验不好，所以项目根据不同的图片大小设置了不同的压缩比，保证图片上传到API的识别率的同时，平衡了等待结果的时间，这块可以再做优化。
4. 图片压缩中，需要利用canvas进行图片重绘的原理，等比例尺寸压缩。处理过程如下：
   1. 先用wx.getImageInfo获取图片信息。
   2. 用wx.canvasDrawImage画图。
   3. 用wx.canvasGetImageData获取图片二进制数据。
   4. 最后用第三方包upng进行base64编码
5. 结果展示页面有两种方式可选：
   1. 是不做任何人脸标记的全图展示，这个很简单，不做说明。
   2. 另一种是对检测到的人脸做出标记。这里可以分为两种方案:
      1. 在原图上人脸部位画框，展示页面只展示原图，下面为对应人脸的检测文本数据。
      2. 根据原图，把检测到的人脸分别用canvas画出来，放入不同的展示页，考虑到展示效果，本项目使用这种方式
6. 在选上述第二种方案展示画图时，需要注意API返回的人脸位置数据是经过压缩的，所以要准确的画图，需要根据压缩比还原位置数据，还需要考虑的是脸部展示的范围，一般人脸检测的位置都是从眉毛到嘴部以上和两边耳朵的位置，为效果需要调整这个位置框稍微大于原始位置框。

本项目是穷人的开发方法，客户端的图片处理导致渲染速度不是很理想，如果各位都是壕，服务器域名走起就行了。

欢迎使用本项目作为大家的小程序插件，下面是已经上线的版本，祝各位扫扫感受到世界满满的恶意~~~~

[![IMG_0591.JPG](https://i.loli.net/2018/08/13/5b70fc371b588.jpg)](https://i.loli.net/2018/08/13/5b70fc371b588.jpg)
[![gh_961dbd6f3596_258.jpg](https://i.loli.net/2018/08/13/5b70fc3735249.jpg)](https://i.loli.net/2018/08/13/5b70fc3735249.jpg)
[![gh_f1b87667acfd_258.jpg](https://i.loli.net/2018/08/13/5b70fc3741991.jpg)](https://i.loli.net/2018/08/13/5b70fc3741991.jpg)

