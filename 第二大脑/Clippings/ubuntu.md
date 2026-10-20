可以。下面我直接给你一套针对你这台 **华硕天选 6 Pro + Windows 11 + 1TB SSD + NVIDIA 显卡 + ESP32 + ROS 2** 的完整教程。

最终目标固定为：

```
Windows 11
├─ 微信 / QQ / Office / Blender / 游戏 / 日常软件
└─ WSL2：保留，做轻量 Linux 开发

Ubuntu 24.04.5.1 LTS 原生
├─ ROS 2 Jazzy
├─ RViz2
├─ Gazebo Harmonic
├─ C++ / Python / CMake
├─ OpenCV
├─ ESP32 / USB / 串口
└─ 后续 MoveIt2 / Nav2 / LeRobot / 具身智能
```

Ubuntu 官方目前确实提供 `ubuntu-24.04.5.1-desktop-amd64.iso`，约 5.8GB；ROS 2 Jazzy 官方支持 Ubuntu 24.04 Noble amd64，并且 Jazzy 是 LTS，支持到 **2029 年 5 月**。[Ubuntu Releases](https://www.us.releases.ubuntu.com/releases/releases/noble/?C=N%3BO%3DD&utm_source=chatgpt.com)

---

# 阶段 0：先理解我们到底要做什么

你的 1TB SSD 目前大概是：

```
1TB SSD
│
├── EFI 启动分区
│
├── Windows C:   630.67GB
│
├── Windows D:   292.97GB
│
└── Recovery 等小分区
```

我们最后想变成：

```
1TB SSD
│
├── EFI
│    ├── Windows Boot Manager
│    └── Ubuntu
│
├── Windows C:
│
├── Windows D:
│
├── Ubuntu /
│    └── ext4 约 120GB
│
└── Windows Recovery
```

开机以后你可以选择：

```
Ubuntu
Windows Boot Manager
```

这叫 **双系统 / Dual Boot**。

不是虚拟机，也不是 WSL。

---

# 阶段 1：把 Windows 的 BitLocker 处理干净

这是你**现在正在做的步骤**。

你刚才已经查出来：

```
C:
100% 加密
TPM + 数字密码

D:
100% 加密
自动解锁启用
数字密码 + 外部密钥
```

所以现在先不要分区、不要做 Ubuntu 安装。

## 1.1 先保存恢复密钥

管理员 PowerShell：

```
manage-bde -protectors -get C:
```

然后：

```
manage-bde -protectors -get D:
```

找：

```
数字密码

123456-123456-123456-123456-123456-123456-123456-123456
```

这是 48 位恢复密码。

**保存到手机或者其他设备，不要只放在 C/D 盘。**

微软说明 Windows 的 Device Encryption 会使用 BitLocker 技术，而且可能同时加密系统盘和固定数据盘。[微软支持](https://support.microsoft.com/zh-cn/windows/security/encryption/device-encryption-in-windows?utm_source=chatgpt.com)

---

## 1.2 先解密 D

因为你的 D：

```
自动解锁: 已启用
```

而自动解锁密钥和 C 盘有关。

管理员 PowerShell：

```
manage-bde -off D:
```

微软官方定义这个命令就是：

> 解密该驱动器并关闭 BitLocker。[微软学习](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/manage-bde-off?utm_source=chatgpt.com)

然后：

```
manage-bde -status D:
```

你可能看到：

```
转换状态: 正在解密
已加密百分比: 85.3%
```

过一会：

```
60%
30%
10%
0%
```

你需要等到：

```
转换状态: 已完全解密
已加密百分比: 0.0%
保护状态: 保护已关闭
```

### 如果 `manage-bde -off D:` 也报版本不支持

进入：

```
设置
→ 隐私和安全性
→ 设备加密
```

关闭：

```
设备加密
```

然后重新执行：

```
manage-bde -status
```

观察 C/D 是否都进入“正在解密”。

---

# 阶段 2：再关闭 C 盘 BitLocker

D 已经：

```
0.0%
已完全解密
```

之后运行：

```
manage-bde -off C:
```

然后：

```
manage-bde -status C:
```

同样一直等到：

```
转换状态: 已完全解密
已加密百分比: 0.0%
保护状态: 保护已关闭
```

微软明确说明，`manage-bde -off` 在完成后会关闭 BitLocker 并移除对应保护器。[微软学习](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/manage-bde-off?utm_source=chatgpt.com)

### 最终验收

执行：

```
manage-bde -status
```

我们要看到：

```
C:
已完全解密
0.0%

D:
已完全解密
0.0%
```

**看到这个才能进入下一阶段。**

---

# 阶段 3：备份 Windows

双系统本身很成熟，但涉及：

```
分区
UEFI
Bootloader
操作系统安装
```

所以必须做基本备份。

至少保存这些：

- 桌面、文档、下载里的重要文件
- 学校资料
- 项目代码
- Git 未 push 内容
- Obsidian 知识库
- 浏览器重要数据
- Windows/BitLocker 恢复密钥

Ubuntu 官方同样建议在已有系统上安装之前备份数据。[Ubuntu](https://ubuntu.com/tutorials/install-ubuntu-desktop?utm_source=chatgpt.com)

---

# 阶段 4：确认 Windows 是 UEFI

按：

```
Win + R
```

输入：

```
msinfo32
```

打开“系统信息”。

找：

```
BIOS 模式
```

应该看到：

```
UEFI
```

再找：

```
安全启动状态
```

可能是：

```
启用
```

这没有关系。

### 现在不要关闭 Secure Boot

Ubuntu 本身支持 UEFI Secure Boot，而且 Ubuntu 官方提供的 NVIDIA 驱动通常也是签名驱动；`ubuntu-drivers` 默认优先选择能与 Secure Boot 配合的包。[Ubuntu](https://ubuntu.com/desktop/docs/en/latest/how-to/graphics/install-nvidia-drivers/?utm_source=chatgpt.com)

所以当前：

```
Secure Boot：保持原样
TPM：保持原样
UEFI：保持原样
```

尤其：

**不要清 TPM。**

---

# 阶段 5：关闭 Windows Fast Startup

这是“双系统”很重要的一步。

Windows 的“快速启动”并不是真正彻底关机，它会把内核状态写入休眠文件再关机。微软官方解释 Fast Startup 实际使用了类似休眠的机制。[微软学习](https://learn.microsoft.com/en-us/troubleshoot/windows-client/setup-upgrade-and-drivers/fast-startup-causes-system-hibernation-shutdown-fail?utm_source=chatgpt.com)

进入：

```
控制面板
↓
硬件和声音
↓
电源选项
↓
选择电源按钮的功能
↓
更改当前不可用的设置
```

取消：

```
☐ 启用快速启动（推荐）
```

点击：

```
保存修改
```

以后双系统环境中这样更干净。

---

# 阶段 6：从 Windows 腾出 120GB 给 Ubuntu

等 BitLocker 完全解密以后再操作。

按：

```
Win + X
```

选择：

```
磁盘管理
```

你应该看到：

```
磁盘 0

EFI
C:
D:
Recovery
```

## 给 Ubuntu 多大？

我建议你：

```
120GB
```

原因是你只有大约 250GB 剩余空间，还需要给 Windows 留余量。

120GB 已经足够你前期：

```
ROS2
RViz
Gazebo
代码
ESP32
OpenCV
少量模型
Docker
```

使用。

以后真开始跑大型 AI 数据集，再考虑扩盘或者增加 SSD。

---

## 从哪个盘压缩？

你现在：

```
C = 630GB
D = 293GB
```

哪个盘有足够的空闲空间，就从哪个压缩。

例如 C 还有大量空闲：

```
右键 C:
→ 压缩卷
```

输入：

```
122880 MB
```

因为：

```
120 × 1024 = 122880 MB
```

然后点击：

```
压缩
```

完成以后应该看到：

```
C:
D:
未分配 120GB
```

通常“未分配”显示为黑色。

## 非常重要

**不要对这 120GB 做任何操作。**

不要：

```
新建简单卷
格式化 NTFS
格式化 exFAT
分配盘符
```

就让它保持：

```
未分配
Unallocated
```

Ubuntu 安装器会处理它。

---

# 阶段 7：下载 Ubuntu ISO

我们明确选：

```
Ubuntu 24.04.5.1 LTS
Desktop
AMD64
```

不是：

```
Ubuntu 26.04
ARM64
Server
WSL
```

Ubuntu 官方 24.04.5.1 release 页面当前提供：

````
ubuntu-24.04.5.1-desktop-amd64.iso
约 5.8GB
``` :chatgpt-content-reference{index="7"}


[Ubuntu 24.04.5.1 官方下载页](https://releases.ubuntu.com/24.04.5.1/?utm_source=chatgpt.com)

### AMD64 不等于只能 AMD CPU

`AMD64` 就是现在普通 PC 使用的：

```text
x86-64
````

Intel / AMD 64 位电脑都是这个。

---

# 阶段 8：验证 ISO 文件

这是可选，但我建议做。

下载完成以后打开 PowerShell，假设 ISO 在：

```
C:\Users\你的用户名\Downloads
```

执行：

```
cd $HOME\Downloads
```

然后：

```
Get-FileHash .\ubuntu-24.04.5.1-desktop-amd64.iso -Algorithm SHA256
```

会得到一串：

```
SHA256
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

然后在 Ubuntu 官方下载目录找到：

```
SHA256SUMS
```

比较：

```
官方 SHA256
=
你电脑算出的 SHA256
```

完全一致才最好。

---

# 阶段 9：制作 Ubuntu U 盘

准备：

```
U盘 ≥ 8GB
建议 16GB+
```

注意：

**U盘会被清空。**

Ubuntu 官方现在仍推荐 Windows 用户可以使用 Rufus 制作启动盘。[Ubuntu](https://ubuntu.com/desktop/docs/en/latest/how-to/create-a-bootable-usb-stick/?utm_source=chatgpt.com)

[Rufus 官网](https://rufus.ie/?utm_source=chatgpt.com)

插入 U 盘，打开 Rufus。

设置：

```
设备：
你的U盘

引导类型：
ubuntu-24.04.5.1-desktop-amd64.iso

分区类型：
GPT

目标系统：
UEFI（非 CSM）
```

其他保持默认。

点击：

```
开始
```

如果出现：

```
ISOHybrid image detected
```

选择：

```
Write in ISO Image mode
```

Ubuntu 官方 Rufus 指南也是这个选择。[Ubuntu](https://ubuntu.com/desktop/docs/en/latest/how-to/create-a-bootable-usb-stick/?utm_source=chatgpt.com)

等状态显示：

```
READY
```

启动盘就制作完成。

---

# 阶段 10：第一次从 U 盘启动

这里开始涉及 BIOS，但我们尽量**不修改 BIOS**。

U盘插着。

Windows：

```
关机
```

不要休眠。

华硕官方方式：

```
按住 Esc
+
按电源键
```

等 Boot Menu 出现后松开 Esc。[ASUS Global](https://www.asus.com/sg/support/faq/1013017/?utm_source=chatgpt.com)

你应该看到类似：

```
Windows Boot Manager

UEFI: Kingston DataTraveler
UEFI: SanDisk
UEFI: xxxx
```

选择你的：

```
UEFI: USB
```

回车。

如果 Esc 不行：

```
关机
↓
按住 F2
↓
按电源
```

进入 BIOS。

ASUS 官方确认笔记本通常可通过按住 F2 再开机进入 UEFI BIOS。[ASUS Global](https://www.asus.com/us/support/faq/1008829/?utm_source=chatgpt.com)

然后可以：

```
F8 → Boot Menu
```

选择 U 盘。

---

# 阶段 11：先 Try Ubuntu，不安装

启动以后应该看到 Ubuntu 页面。

可能出现：

```
Try or Install Ubuntu
```

进入。

语言我建议：

```
English
```

键盘：

```
English (US)
```

以后你看 Linux / ROS 报错会方便很多。

进入后选择：

```
Try Ubuntu
```

而不是立即：

```
Install Ubuntu
```

Ubuntu Desktop ISO 本身就是设计成可以先在不修改硬盘的情况下试用系统。[Ubuntu Releases](https://us.releases.ubuntu.com/24.04.5.1/?utm_source=chatgpt.com)

---

# 阶段 12：在 Try Ubuntu 里测试你的天选 6 Pro

这是非常重要的一步。

先测试：

|项目|要求|
|---|---|
|Wi-Fi|能联网|
|键盘|正常|
|触控板|正常|
|声音|正常|
|蓝牙|能开启|
|亮度|能调|
|USB|能识别|
|摄像头|最好正常|
|分辨率|正常|
|SSD|能识别|
|NVIDIA|至少硬件被识别|

按：

```
Ctrl + Alt + T
```

打开 Terminal。

执行：

```
lspci | grep -Ei "VGA|3D|Display"
```

正常应该看到：

```
Integrated Graphics ...
NVIDIA Corporation ...
```

然后：

```
lsblk
```

应该看到：

```
nvme0n1
```

大约：

```
931GB
```

这就是你的 1TB SSD。

然后：

```
lsusb
```

测试 USB。

---

# 阶段 13：确认 Ubuntu 能看到那 120GB

执行：

```
lsblk -f
```

你可能会看到：

```
nvme0n1
├─nvme0n1p1   vfat
├─nvme0n1p2
├─nvme0n1p3   ntfs
├─nvme0n1p4   ntfs
└─...
```

未分配空间本身不会表现成普通文件系统分区，这是正常的。

真正安装的时候 Ubuntu 安装器会识别。

---

# 阶段 14：如果 Try Ubuntu 一切正常，正式安装

桌面找到：

```
Install Ubuntu
```

双击。

按照安装器继续。

通常会经历：

```
语言
↓
辅助功能
↓
键盘
↓
网络
↓
安装类型
```

选择：

```
Interactive installation
```

软件：

```
Default selection
```

即可。

---

# 阶段 15：第三方软件建议打开

安装器如果出现：

```
Install third-party software
```

或者类似：

```
Install third-party software for graphics and Wi-Fi hardware
```

建议勾选。

原因主要就是：

```
NVIDIA
Wi-Fi
多媒体相关驱动
```

---

# 阶段 16：最危险的页面——Disk Setup

这一步一定慢慢看。

如果出现：

```
Install Ubuntu alongside Windows Boot Manager
```

这是我们最希望看到的选项。

Ubuntu 官方安装器支持在保留 Windows 的情况下进行 alongside 安装；如果 BitLocker 仍然开启，安装器可能无法安全获取 Windows 分区信息，所以我们前面才要求完全解密。[Ubuntu](https://ubuntu.com/tutorials/install-ubuntu-desktop?utm_source=chatgpt.com)

选择：

```
Install Ubuntu alongside Windows Boot Manager
```

## 千万不要选

```
Erase disk and install Ubuntu
```

因为：

```
Erase disk
=
整个 SSD 清空
=
Windows + C + D 全没
```

看到这个选项要条件反射：

**不选。**

---

# 阶段 17：如果没有 Alongside 选项怎么办

不要慌。

还有：

```
Manual installation
```

但这是备用方案。

你应该能够看到一个大约：

```
120GB
Free Space
```

### 简单手动分区方案

用这 120GB 建：

```
ext4
Mount point: /
Size: 几乎全部120GB
```

不需要单独创建：

```
swap
/home
```

Ubuntu 可以使用 swapfile。

同时你原本会有一个小型 EFI 分区，例如：

```
FAT32
100MB～1GB
EFI System Partition
```

它是 Windows 已经创建的。

如果手动模式要求指定 EFI：

```
Mount point:
/boot/efi
```

**绝对不要勾 Format。**

也就是：

```
EFI:
继续共用
不格式化

120GB:
ext4
/
格式化
```

这样：

```
Windows Boot Manager
+
Ubuntu GRUB
```

可以共存在 EFI 分区里。

如果你到这个页面和我描述的完全不同，**不要猜分区**。

---

# 阶段 18：Ubuntu 磁盘加密暂时不要开

如果安装器问 Ubuntu 自己是否启用：

```
Encryption
TPM-backed Full Disk Encryption
```

我建议你第一次安装 ROS2 工作站：

```
不启用
```

Ubuntu 24.04 的 TPM-backed full-disk encryption 在初期就存在一些与第三方驱动/内核模块相关的兼容限制，而你的电脑恰好又需要 NVIDIA。[Ubuntu](https://ubuntu.com/blog/ubuntu-desktop-24-04-noble-numbat-deep-dive?utm_source=chatgpt.com)

所以现在目标是：

```
稳定 ROS2 开发环境
```

不是再给 Ubuntu 增加一层复杂度。

---

# 阶段 19：创建 Ubuntu 用户

例如：

```
Your name:
Chen

Computer name:
tuf-ros2

Username:
chy
```

密码一定记住。

因为以后经常会：

```
sudo apt install ...
```

sudo 会要求这个密码。

---

# 阶段 20：正式安装

安装器最终会给你一个总结。

这里重点确认：

```
Ubuntu
安装到那120GB空间

Windows partitions
没有被格式化
```

再点击：

```
Install
```

等待安装结束。

然后：

```
Restart Now
```

系统提示拔 U 盘时：

```
拔掉 U 盘
```

回车。

---

# 阶段 21：第一次看到 GRUB

重启以后理想情况：

```
GNU GRUB

Ubuntu

Advanced options for Ubuntu

Windows Boot Manager
```

以后：

```
学习机器人
→ Ubuntu

日常 Windows
→ Windows Boot Manager
```

先分别测试两次。

第一次进：

```
Ubuntu
```

确认正常。

然后重启选：

```
Windows Boot Manager
```

确认 Windows 也正常。

这一步很重要。

---

# 阶段 22：如果没有 GRUB

比如直接进 Ubuntu 或直接进 Windows。

先不要认为系统没了。

关机。

华硕：

```
Esc + 电源
```

进入 Boot Menu。

一般应该存在：

```
ubuntu
Windows Boot Manager
```

两个 UEFI 启动项。

只要两个都在，系统通常都还好好的。

---

# 阶段 23：Ubuntu 第一次更新

进入 Ubuntu。

打开 Terminal：

```
sudo apt update
```

然后：

```
sudo apt full-upgrade -y
```

完成：

```
sudo reboot
```

---

# 阶段 24：安装基础开发工具

重新进系统：

```
sudo apt install -y \
git \
curl \
wget \
vim \
htop \
build-essential \
cmake \
python3 \
python3-pip \
usbutils \
pciutils
```

检查：

```
gcc --version
```

```
g++ --version
```

```
cmake --version
```

```
python3 --version
```

```
git --version
```

以后你的 Linux 开发环境基本就有了。

---

# 阶段 25：检查 Ubuntu 版本

运行：

```
lsb_release -a
```

或者：

```
cat /etc/os-release
```

应该看到：

```
Ubuntu 24.04.x LTS
Codename: noble
```

这是 ROS 2 Jazzy 最关键的验证。

Jazzy 的 Ubuntu deb 包官方目标平台就是 Ubuntu Noble 24.04 amd64 / arm64。[ROS 2 Repo](https://repo.test.ros2.org/en/jazzy/Installation/Ubuntu-Install-Debs.html?utm_source=chatgpt.com)

---

# 阶段 26：安装 NVIDIA 驱动

先：

```
lspci | grep -Ei "VGA|3D"
```

然后：

```
sudo ubuntu-drivers list
```

查看 Ubuntu 推荐驱动。

再：

```
sudo ubuntu-drivers install
```

完成：

```
sudo reboot
```

回来以后：

```
nvidia-smi
```

如果出现：

```
NVIDIA-SMI
Driver Version
CUDA Version
GPU Name
```

显卡驱动成功。

Ubuntu 官方明确推荐通过：

```
Additional Drivers
```

或者：

```
ubuntu-drivers
```

管理 NVIDIA 驱动，并提醒不要随意使用外部来源覆盖 Ubuntu 驱动，因为可能破坏 Secure Boot 或系统兼容性。[Ubuntu](https://ubuntu.com/desktop/docs/en/latest/how-to/graphics/install-nvidia-drivers/?utm_source=chatgpt.com)

### 所以不要这么搞

不要百度：

```
NVIDIA官网
↓
下载 xxx.run
↓
sudo sh NVIDIA.run
```

先完全不要走这种路线。

---

# 阶段 27：测试 ESP32

把你的：

**ESP32EA MOC + ESP32EXP1**

插电脑。

运行：

```
lsusb
```

应该看到类似：

```
CP210x
CH340
USB Serial
Espressif
```

具体取决于板子的 USB 转串口芯片。

然后：

```
ls /dev/ttyUSB*
```

或者：

```
ls /dev/ttyACM*
```

可能出现：

```
/dev/ttyUSB0
```

以后你会经常接触这个东西。

Linux 世界里：

```
Windows COM3
≈
Linux /dev/ttyUSB0
```

---

# 阶段 28：让当前账户可以访问串口

通常：

```
sudo usermod -aG dialout $USER
```

然后：

```
reboot
```

回来后：

```
groups
```

应该能看到：

```
dialout
```

这样以后 Arduino / ESP-IDF / PlatformIO / micro-ROS 操作串口会方便很多。

---

# 阶段 29：安装 ROS 2 Jazzy

到这里才真正开始 ROS2。

先检查 locale：

```
locale
```

应该包含：

```
UTF-8
```

如果没有：

```
sudo apt update
sudo apt install locales -y
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

ROS Jazzy 官方安装说明要求使用支持 UTF-8 的 locale。[ROS 2 Repo](https://repo.test.ros2.org/en/jazzy/Installation/Ubuntu-Install-Debs.html?utm_source=chatgpt.com)

---

# 阶段 30：启用 Universe

```
sudo apt install software-properties-common -y
```

然后：

```
sudo add-apt-repository universe
```

---

# 阶段 31：添加 ROS 官方软件源

先：

```
sudo apt update
sudo apt install curl -y
```

然后：

```
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
```

下载官方 ROS apt source：

```
curl -L -o /tmp/ros2-apt-source.deb \
"https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
```

然后：

```
sudo dpkg -i /tmp/ros2-apt-source.deb
```

这是当前 ROS 2 Jazzy 文档使用的软件源安装方式。[ROS 2 Repo](https://repo.test.ros2.org/en/jazzy/Installation/Ubuntu-Install-Debs.html?utm_source=chatgpt.com)

---

# 阶段 32：安装 ROS 2 Desktop

先：

```
sudo apt update
```

```
sudo apt upgrade -y
```

安装：

```
sudo apt install ros-jazzy-desktop -y
```

然后开发工具：

```
sudo apt install ros-dev-tools -y
```

`ros-jazzy-desktop` 包括 ROS2、RViz、Demo、教程所需的桌面组件，非常适合你这种机器人学习环境。[ROS 2 Repo](https://repo.test.ros2.org/en/jazzy/Installation/Ubuntu-Install-Debs.html?utm_source=chatgpt.com)

---

# 阶段 33：启用 ROS2 环境

运行：

```
source /opt/ros/jazzy/setup.bash
```

检查：

```
ros2 --help
```

如果正常显示命令说明：

```
ros2
action
bag
component
daemon
interface
launch
node
param
pkg
run
service
topic
...
```

成功。

为了以后每次打开终端不用输入：

```
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc
```

然后：

```
source ~/.bashrc
```

---

# 阶段 34：第一个 ROS2 测试

开终端 1：

```
ros2 run demo_nodes_cpp talker
```

应该不停输出：

```
Publishing: 'Hello World: 1'
Publishing: 'Hello World: 2'
...
```

再开终端 2：

```
ros2 run demo_nodes_py listener
```

应该看到：

```
I heard: 'Hello World: 1'
I heard: 'Hello World: 2'
```

这一步非常有意义：

```
C++ Node
      ↓
ROS2 Topic
      ↓
Python Node
```

说明：

```
ROS2 DDS 通信
C++ API
Python API
```

全部正常。官方安装文档也使用 talker/listener 作为验证方法。[GitHub](https://github.com/BENKRID-abdenour/ROS2_Jazzy_Jalisco_Instalation_and_Configuration/blob/main/README.md?utm_source=chatgpt.com)

---

# 阶段 35：测试 RViz2

运行：

```
rviz2
```

如果出现 GUI：

```
RViz
Displays
Views
Grid
```

说明：

```
ROS2 GUI
OpenGL
显卡
Qt
```

基本都工作。

---

# 阶段 36：Gazebo 用哪个？

你的组合固定：

```
Ubuntu 24.04
+
ROS 2 Jazzy
+
Gazebo Harmonic
```

这是 Gazebo 官方明确推荐的 ROS 2 Jazzy 搭配。[Gazebo](https://gazebosim.org/docs/garden/ros_installation/?utm_source=chatgpt.com)

不是网上很多旧教程的：

```
Gazebo Classic
gazebo11
```

Gazebo Classic 已经结束生命周期。

你以后看到：

```
gazebo
gazebo_ros_pkgs
```

先看教程年代。

现在更应该学习：

```
Gazebo Harmonic
gz sim
ros_gz
```

---

# 阶段 37：Gazebo 可以稍后安装

我不建议系统刚装好就一次性把：

```
CUDA
ROS2
Gazebo
MoveIt
Nav2
Isaac
micro-ROS
LeRobot
```

全部塞进去。

正确节奏应该是：

```
Ubuntu
  ↓
Linux基础
  ↓
ROS2基础
  ↓
RViz
  ↓
URDF / TF
  ↓
Gazebo Harmonic
  ↓
ESP32
  ↓
真实机器人
```

这样出错时你知道是哪一层的问题。

---

# 阶段 38：建立你的第一个 ROS2 Workspace

ROS2 稳定以后：

```
mkdir -p ~/ros2_ws/src
```

进入：

```
cd ~/ros2_ws
```

编译：

```
colcon build
```

然后：

```
source install/setup.bash
```

以后你的机器人项目基本都在：

```
~/ros2_ws/
│
├── src/
│   ├── robot_description
│   ├── robot_bringup
│   ├── perception
│   └── control
│
├── build/
├── install/
└── log/
```

这就是 ROS2 标准工作空间思维。

---

# 你的整个学习/系统结构

装完以后，我建议保持：

```
Windows
│
├── 日常生活
├── 微信
├── Office
├── Blender
├── 游戏
├── Codex
│
└── WSL2
     └── 轻量Linux开发


Ubuntu 24.04
│
├── Linux
├── Git
├── CMake
├── Python
├── C++
│
├── ROS2 Jazzy
│   ├── Node
│   ├── Topic
│   ├── Service
│   ├── Action
│   ├── Parameter
│   ├── Launch
│   ├── TF2
│   └── rosbag
│
├── RViz2
│
├── URDF / Xacro
│
├── Gazebo Harmonic
│
├── OpenCV / YOLO
│
├── ESP32
│   ├── Serial
│   └── micro-ROS
│
├── Nav2
├── MoveIt2
│
└── LeRobot / 具身智能
```

这套环境跟你的机器人工程路线是匹配的。

---

# 现在别一次做到阶段 38

**你当前只做到下面这个验收点：**

|当前步骤|完成标准|
|---|---|
|BitLocker D|`0.0% 已完全解密`|
|BitLocker C|`0.0% 已完全解密`|
|数据备份|完成|
|Windows Fast Startup|关闭|
|UEFI|确认为 UEFI|
|Ubuntu空间|120GB 未分配|
|ISO|`24.04.5.1 desktop amd64`|
|Rufus|READY|
|U盘启动|成功|
|Try Ubuntu|Wi-Fi/声音/显卡/SSD正常|

**到这里再正式安装 Ubuntu。**

尤其你现在还处于 BitLocker 阶段，所以当前第一目标仍然是：

```
manage-bde -status
```

最终让 **C 和 D 都变成 `0.0% / 已完全解密`**。之后我们再进入“压缩 120GB → Rufus → 华硕 Esc 启动菜单 → Try Ubuntu”。这样一步一步走，翻车概率最低。