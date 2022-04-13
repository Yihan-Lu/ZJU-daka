# ZJU-daka(这东西没有库，总不能删库跑路了吧)

**ZJU每日健康上报自动化打卡教程**，利用**windows**自带的功能：**任务计划程序**，以及**edge浏览器(chrome浏览器也行)插件**，实现每日健康上报：

<img src="https://user-images.githubusercontent.com/48110180/161521489-5bca2e53-c342-4c38-bfde-faed5573c18c.png" width="500px">

 - 需本地运行或服务器(自己的电脑)
 - 默认每次提交上次所提交的内容（只有时间部分更新）
 - 打卡信息有变时，请手动打卡一次
 - 电脑只要一直处于开机状态，或者每天正常开关机，就会实现自动打卡。

## Update
### 2022.04.13
"taskkill /im msedge.exe /f  //关闭edge浏览器进程"如果关不掉，盲猜再加一个"/t"应该就可以了。

### 2022.04.07
如果定位不了，可能是以下问题：1. 挂着vpn；2. 浏览器别的插件的定位，比如Momentum的地理定位是可以手动调的。反正直要改成杭州就都可以，其余地方不知道行不行。

### 2022.04.06
内容变动，需手动填一次，多了一项选择校区，因为城西银泰出事了。BYW，一开始至少是不厌烦上海的，现在开始讨厌了，可能是我读书读的少，也没考上复旦上交，不知道上海的经济地位到底有多重要。但是，就像网友说的，杭州的医生护士去上海支援抗疫，上海的一对母女来杭州散播病毒。虽然大家都说着吴越方言，但我看到的是世界的参差。

### 2022.04.05
**第Ⅴ步修正一下**，大家可能会发现任务状态会一直处于“准备就绪”的状态，而到时间不会自己运行。这边按下图设置，应该可以，即在“起始于”填入程序或脚本的根目录。

<img src="https://user-images.githubusercontent.com/48110180/161714828-42bae286-a040-4171-9f9a-ff892fb74c06.png" width="500px">

**起始于**这个操作在windows计划任务的设置里是个可选项，所以我们在操作的时候一般都忽略了，但因此会导致错误的发生。比如我的定时任务程序中需要用到一个文件，在读的时候就会报未能找到文件“C:\Windows\system32\xxx”,又或者程序要写一个文件的时候也会报错，这些都是因为没有设置"起始于"导致的，起始于的目的是指定你执行文件的根目录，如果没有指定程序就会寻址到C:\Windows\system32这个目录下。


## 步骤

**〇.** 打开edge浏览器(chrome浏览器也行，但是得与Ⅳ对应)，**fig1、2**. Extentions→Manage extensions→Get extensions for Microsoft Edge，**fig3**. 搜索“Tampermonkey”并安装，**fig4**. 确保该插件是开启的。**Tampermonkey下载缓慢可点击此处去[官网](https://www.tampermonkey.net/)下载。**


| fig1                 | fig2                     | fig3                |fig4                 |
|:--------------------:|:------------------------:|:-------------------:|:-------------------:|
| ![image](https://user-images.githubusercontent.com/48110180/161521840-4a906494-9576-4ceb-b238-dd3c5480848d.png#pic_center) | ![image](https://user-images.githubusercontent.com/48110180/161521851-84fbd3a0-b350-467f-a49b-2d22ae05af06.png#pic_center) | ![image](https://user-images.githubusercontent.com/48110180/161521876-e3db7da9-9c78-4eac-bd74-d96f485c0833.png#pic_center) | ![image](https://user-images.githubusercontent.com/48110180/161530171-d55178db-c7ff-45bd-9165-9fc9a2ddbce8.png#pic_center) |

- - -

**Ⅰ.** **fig5**. 点击该链接进行安装：[浙江大学健康上报自动化脚本 (greasyfork.org)](https://greasyfork.org/en/scripts/430082-%E6%B5%99%E6%B1%9F%E5%A4%A7%E5%AD%A6%E5%81%A5%E5%BA%B7%E6%89%93%E5%8D%A1%E8%87%AA%E5%8A%A8%E5%8C%96%E8%84%9A%E6%9C%AC-%E4%BF%AE%E6%AD%A3%E7%89%88)；**fig6**. 安装不了的(一般不会装不了)，将该项目文件夹下的.js文件内容复制到图中位置。

| fig5                | fig6                     |
|:--------------------:|:------------------------:|
| ![image](https://user-images.githubusercontent.com/48110180/161521893-f36f4bd5-aa01-4cb0-8dd9-5ee352a0d9d3.png#pic_center) | ![image](https://user-images.githubusercontent.com/48110180/161539362-d820c319-0c1d-4172-bd01-0ed008369131.png#pic_center) | 

- - -

**Ⅱ.** 打开“任务计划程序”，“常规”如图按需自定义设置。

<img src="https://user-images.githubusercontent.com/48110180/161522050-ac89b8d8-6267-4f92-a6d4-11990aac29d5.png" width="500px">

- - -

**Ⅲ.** “触发器”如图按需自定义设置。

<img src="https://user-images.githubusercontent.com/48110180/161522064-886306c8-b1f0-40e6-99f3-95abf7cf34f1.png" width="500px">

- - -

**Ⅳ.** 在自己选定的文件夹创建一个新的txt文件(比如：ZJUdaka.txt)，并输入以下内容并保存。然后重命名为batch处理文件，即将txt后缀改为bat(比如：ZJUdaka.bat)。msedge.exe可以换成别的浏览器，比如chrome.exe。

```batch
@echo off
start msedge.exe https://healthreport.zju.edu.cn/ncov/wap/default/index  //打开健康上报网页
ping -n 60 127.0.0.1  //等待1分钟
taskkill /im msedge.exe /f  //关闭edge浏览器进程
```

- - -

**Ⅴ.** “操作”如图按需自定义设置

<img src="https://user-images.githubusercontent.com/48110180/161522109-32c512ec-7ba0-487b-bf2d-42363ef9fef8.png" width="500px">

- - -

**Ⅵ.** “条件”如图按需自定义设置。

<img src="https://user-images.githubusercontent.com/48110180/161522131-32d14288-b684-4d38-a303-faed35f4f2ef.png" width="500px">

- - -

**Ⅶ.** “设置”如图按需自定义设置。

<img src="https://user-images.githubusercontent.com/48110180/161522147-d3e1f6b4-96d8-45f8-9043-6cbb734e9f81.png" width="500px">

- - -

**Ⅷ.** 点击确定，此处输入的密码是帐号密码，不是开机的pin码。

<img src="https://user-images.githubusercontent.com/48110180/161522165-acdcd74e-a76b-445d-b7bf-1bf9ea9b7c27.png" width="500px">

- - -

**Ⅸ.** 任务计划程序库→拉到最底下找到“ZJUdaka”选中→运行→状态变为“正在运行”即可。

<img src="https://user-images.githubusercontent.com/48110180/161522183-a95c698d-93dc-4bb7-a70c-9b6e6f4b9859.png" width="500px">

- - -

**Ⅹ.** 理论上来说已经设置完成了。
