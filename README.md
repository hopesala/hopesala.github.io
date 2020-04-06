## Welcome to GitHub Pages

# 哈尔滨工业大学 计算机科学与技术学院
## 规格严格 功夫到家
You can use the [editor on GitHub](https://github.com/hopesala/hopesala.github.io/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/hopesala/hopesala.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.



第一个Xilinx Vitis IDE入门helloworld程序（包含Vivado，Vitis和Petalinux，超长&超多图）
Xilinx最近推出了新一代Vitis统一软件平台工具，结合官方文档进行了学习，整理相关文档如下，供参考～

操作系统：Ubuntu 18.04.4 LTS
命令lsb_release -a
 
安装Vitis
https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vitis.html
在线安装网速很慢，离线下载安装包，Xilinx_Vitis_2019.2_1106_2127.tar 30.76GB
解压缩
tar xvf Xilinx_Vitis_2019.2_1106_2127.tar
 
安装，需要至少120G磁盘空间，最好150G以上
cd Xilinx_Vitis_2019.2_1106_2127/
./xsetup
 

安装完成
 
 
安装petalinux
略

进入正题
1.	 首先使用Vivado生成.xsa文件
打开Vivado 2019.2，File->New Project
 
Next,项目名称edt_zcu102_demo
 
默认
 
接下来的两个界面Add Sources和Add Constraints都直接Next，然后选择Boards，选择ZCU102，Next
 
 
Finish，之后项目自动打开，点Create Block Design
 
设计名称edt_zcu102_demo,然后OK
 
Add IP，如下图红色箭头所示
 
输入znyq进行过滤，并选择Zynq UltraScale+ MPSoC
 
点击Run Block Automation
 
默认，点OK
 
双击红框位置，查看自动化效果
 
点左侧PS-PL Configuration，展开，并将红框中的勾选框取消勾选，结果如图，OK
 
空白处右键，点击Validate Design
 
验证成功提示
 
右键Design Sources下的文件，点击Create HDL Wrapper
 
默认，点OK
 
创建完成之后，展开edt_zcu102_demo_wrapper，右键左侧红箭头，然后点Generate Output Products…
 
默认，点Generate
 
生成完成
 
导出硬件
 
点OK
 

生成最终的edt_zcu102_demo_wrapper.xsa文件，后文petalinux会用到该文件

2.	下面，有请Xilinx Vitis IDE 2019.2闪亮登场！
2.1 创建平台项目PlatformProject
打开Vitis IDE ，Workspace选择默认，进入
 
平台项目名称test_a53_demo,Next
 
默认，从XSA文件创建，Next
 
XSA文件选择上文Vivado生成的edt_zcu102_demo_wrapper.xsa文件所在路径

 
点击Finish，依次点击下图红色箭头处，最后点击Modify BSP Settings…
 
添加如下三个库：xilffs, xilpm, xilsecure，点击OK

 

右键该平台，如下图，Build Project
 

Build完成
 
2.2 创建应用工程ApplicationProject
创建了平台工程Platform Project，接下来创建应用工程Application Project
 
项目名称：test_a53_application_demo，Next
 
选择刚才创建的Platform
 
默认，Next
 
选择Hello World，点击Finish
 
修改src下的helloworld.c文件，并保存
 
右键项目，如下图，Build Project
 
由于没有硬件ZCU102板子，采用Emulation仿真，右键点击Run As/Lunch on Emulator
 
点击Start Emulator and Run
 
运行成功，Emulation Console输出printf信息～
 


3.	使用petalinux生成linux镜像
petalinux和bsp（Board Support Packages）文件这里下载https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools.html
我的petalinux安装在~/Desktop/petalinux/下
执行命令source ~/Desktop/petalinux/settings.sh
进入bsp文件所在目录
创建工程命令petalinux-create -t project -s xilinx-zcu102-v2019.2-final.bsp
 

根据edt_zcu102_ demo_wrapper.xsa文件所在目录（注意不是文件，也不要拷贝.xsa至当前目录下）重新配置petalinux-config --get-hw-description='/home/caochenghua/project_1edt_zcu102/' 
弹出如下界面，修改配置信息
 
保存并退出

 
编辑.dtsi文件为如下内容
vi ./project-spec/meta-user/recipes-bsp/device-tree/files/system-user.dtsi
 
完成后，petalinux-build，第一次build需要联网下载sstate-cache超级慢，经过漫长的等待后…终于成功
 
生成的文件在images/linux目录下
 
生成Boot image镜像文件
petalinux-package --boot --fsbl zynqmp_fsbl.elf --u-boot
 
生成的BOOT.BIN image.ub u-boot.elf文件如下
 





参考文献
ug1209-embedded-design-tutorial.pdf
下载地址https://www.xilinx.com/support/documentation/sw_manuals/xilinx2019_2/ug1209-embedded-design-tutorial.pdf

