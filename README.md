# typora-plugin-bilibili
哔哩哔哩图片上传, Typora插件，实现图片粘贴即可上传到哔哩哔哩，并替换链接

fork 来自 [typora-plugin-bilibili](https://github.com/xlzy520/typora-plugin-bilibili)

## 重要提示
**由于B站相簿的上传API自身出现问题，现在切换到动态的图片API，因此需要多加一个参数csrf(为Cookie里面的bili_jct)**

示例
```bash

插件客户端路径 token=0829d25Cdd19b*b1 csrf=cb397c0fbf619237
```

### 用Go重写，产物缩小5倍体积，点击下载即可
<img width="777" alt="image" src="https://user-images.githubusercontent.com/28336270/167284443-9120b23b-fd22-4766-ae0d-4c047b988e9d.png">
之前的
<img width="553" alt="image" src="https://user-images.githubusercontent.com/28336270/167284741-c78e3d98-618d-43a7-b910-f96a8cc940cb.png">

### 插件下载

#### 测试版
- [Windows](https://github.com/MengNianxiaoyao/typora-plugin-bilibili/actions/workflows/ci.yml)
- [Mac](https://github.com/MengNianxiaoyao/typora-plugin-bilibili/actions/workflows/ci.yml)
- [Linux](https://github.com/MengNianxiaoyao/typora-plugin-bilibili/actions/workflows/ci.yml)

#### 正式版
- [Windows](https://github.com/MengNianxiaoyao/typora-plugin-bilibili/releases/latest/download/main.exe)
- [Mac](https://github.com/MengNianxiaoyao/typora-plugin-bilibili/releases/latest/download/main)
- [Linux](https://github.com/MengNianxiaoyao/typora-plugin-bilibili/releases/latest/download/main-linux)

### typora免费版下载
- [Windows](https://typora.io/windows/dev_release.html)      
- [windows x64 国内OSS镜像下载](https://jiali0126.oss-cn-shenzhen.aliyuncs.com/typora/typora-update-x64-1117.exe)
- [Mac](https://typora.io/dev_release.html)

## 感谢
License Certificate for JetBrains All Products Pack
[<img src="https://resources.jetbrains.com/storage/products/company/brand/logos/jb_beam.svg" alt="GitHub license" style="zoom:50%;" />](https://jb.gg/OpenSourceSupport)  

### 开发文档
[开发文档](./dev.md)

### 直接使用

1. 上一步根据自己的系统下载相应的包
2. 获取SESSDATA: 登录哔哩哔哩→F12打开控制台→Application→Cookies→SESSDATA
   ![](https://i0.hdslb.com/bfs/album/fe1a58c25c42743d5f1e186639218ee75a133df2.png)
   
3. 获取csrf: 登录哔哩哔哩→F12打开控制台→Application→Cookies→bili_jct
4. 进入Typora设置，选择图像Tab，插入图片时选择**上传图片**，然后将**插件的绝对路径**填入**命令**。如下地方，例如

   ```bash
    # Mac、Linux
   /Users/xxx/bilibili/typora-plugin-bilibili-macos token=你的SESSDATA csrf=你的bili_jct
   # Windows
   D:\Downloads\typora-plugin-bilibili-win.exe token=你的SESSDATA csrf=你的bili_jct
   ```
   **其中很重要的后面的 `token=你的SESSDATA` ,没有这句的话，无法上传成功，如果发现上传失败，那应该就是SESSDATA过期了，需要手动更新**


#### MacOS
![image-20210608201909889](https://i0.hdslb.com/bfs/album/0f8ad346424ccd2c035c83449e716f0bbf4971b4.png)

**特别的**

**Macos 平台的都是需要授权该可执行文件的**
1. M1芯片的Mac，需要执行以下命令
```bash
chmod a+x ./ 文件名
```
2. 非M1芯片的，设置打开方式为终端打开，尝试打开时会提示无权限，然后去系统偏好设置->通用，点击允许
![](https://i0.hdslb.com/bfs/album/1b86699505befa32f7d87d8024df0c0f2d84ecb9.png)
![](https://i0.hdslb.com/bfs/album/b0eb89a08e4fd3e6ca8063dd71ce6fc2467e69dc.png)

#### Windows
![](https://i0.hdslb.com/bfs/album/3990cc67983fa55b28cf3536c40f7febaf0dfb43.png)

**填入下载的exe文件的完整路径**

### 演示

https://user-images.githubusercontent.com/28336270/118472778-d3d77b80-b73b-11eb-951a-7efb1e5bf15f.mov

http://i0.hdslb.com/bfs/album/34bc7b5a1bd591a1b682fec4593345e4a9e3bfe9.png


### 404解决方案
#### 全站图片使用
在html的head标签中设置如下标志，那么全站资源引用都不会携带referrer

```html
<meta name="referrer" content="no-referrer">
```
### 新窗口打开
主要设置rel="noreferrer"，使用window.open打开的话是会默认携带referrer的，第一次还是会403

```html
<a rel="noreferrer" target="_blank"></a>
```


### 图片参数

格式：(图像原链接)@(\d+[whsepqoc]_?)*(\.(|webp|gif|png|jpg|jpeg))?$
- w:[1, 9223372036854775807] (width，图像宽度)
- h:[1, 9223372036854775807] (height，图像高度)
- s:[1, 9223372036854775807] (作用未知)
- e:[0,2] (resize，0:保留比例取其小，1:保留比例取其大，2:不保留原比例，不与c混用)
- p:[1,1000] (默认100，放大倍数，不与c混用)
- q:[1,100] (quality，默认75，图像质量)
- o:[0,1] (作用未知)
- c:[0,1] (clip，0:默认，1:裁剪)
- webp,png,jpeg,gif(不加则保留原格式)
- 不区分大小写，相同的参数后面覆盖前面
- 计算后的实际w*h不能大于原w*h，否则wh参数失效


### 相似推荐
[哔哩哔哩图床-Chrome插件](https://github.com/xlzy520/bilibili-img-uploader) 提供粘贴图片上传到哔哩哔哩，并进行记录管理
