# 正点原子STM32MP2 Buildroot开发指南

## 硬件支持

- [x] STM32MP257 1 + 8版本
- [ ] STM32MP257 2 + 16版本（开发中）

## 快速开始

### 1. 下载源代码

```bash
# 下载Buildroot（使用bootlin的STM32MP优化分支）
git clone -b st/2025.02.5 https://github.com/bootlin/buildroot.git

# 下载正点原子外部配置树
git clone https://github.com/AcSully/buildroot-external-st.git

# 进入Buildroot目录
cd buildroot
```

### 2. 配置并编译

```bash
# 配置正点原子STM32MP257开发板
make BR2_EXTERNAL=../buildroot-external-st stm32mp257DAK_core_defconfig

# 开始编译（使用4个线程）
make -j4
```

### 3. 烧录镜像

#### 方法一：使用STM32CubeProgrammer（推荐）

1. **准备硬件**
   - 将开发板启动模式设置为USB启动：
     - BOOT0, BOOT1, BOOT2, BOOT3 都设置为 OPEN
   - 使用USB-C线连接开发板的CN15接口到电脑

2. **烧录步骤**
   ```bash
   # 进入镜像目录
   cd output/images/
   
   # 使用STM32CubeProgrammer烧录
   sudo ~/stm32cube/bin/STM32_Programmer_CLI -c port=usb1 -w flash_full.tsv
   ```

3. **完成烧录**
   - 烧录完成后，将启动方式修改为EMMC启动：
     - BOOT0 设置为 ON
     - BOOT1, BOOT2, BOOT3 设置为 OPEN
   - 重启开发板

## 串口连接

### 硬件连接
- 使用USB-C线连接开发板的CN21接口（同时提供供电和串口功能）
- 或者使用USB转TTL模块连接开发板的CN2接口

### 串口配置
- **波特率**: 115200
- **数据位**: 8
- **停止位**: 1
- **校验位**: None
- **流控制**: None

### 串口工具
- Windows: PuTTY, SecureCRT, MobaXterm
- Linux: minicom, picocom, screen
  ```bash
  # 使用minicom连接
  sudo minicom -D /dev/ttyACM0 -b 115200
  
  # 使用screen连接
  sudo screen /dev/ttyACM0 115200
  ```

## 系统登录

串口连接后，使用以下凭据登录系统：

- **用户名**: `root`
- **密码**: `root`

## 功能测试

### Qt5 OpenGL测试

```bash
/usr/lib/qt/examples/opengl/hellogl2/hellogl2
```

## 技术规格

### 软件版本

| 组件 | 版本 |
|------|------|
| TF-A | v2.10-stm32mp-r2 |
| U-Boot | v2023.10-stm32mp-r2 |
| Linux | v6.6-stm32mp-r2 |
| OP-TEE | 4.0.0-stm32mp-r2 |

### 支持的功能

- **图形**: OpenGL ES, Qt5
- **连接**: WiFi, Bluetooth, CAN
- **安全**: OP-TEE可信执行环境
- **更新**: RAUC OTA远程升级
- **存储**: eMMC, SD卡
- **调试**: UART串口调试

## 开发环境要求

### 系统要求

- Linux发行版（Ubuntu 20.04+, Debian 11+等）
- 至少8GB RAM（推荐16GB）
- 至少50GB可用磁盘空间

### 依赖包安装

```bash
# Ubuntu/Debian
sudo apt install debianutils sed make binutils build-essential gcc g++ bash patch gzip bzip2 perl tar cpio unzip rsync file bc git
```

## 目录结构

```
buildroot-external-st/
├── board/stmicroelectronics/stm32mp2/  # 板级支持文件
│   ├── linux-dts/                      # Linux设备树
│   ├── uboot-dts/                      # U-Boot设备树
│   ├── tfa-dts/                        # TF-A设备树
│   └── optee-dts/                      # OP-TEE设备树
├── configs/                            # 配置文件
│   └── stm32mp257DAK_core_defconfig    # 正点原子配置
├── package/                            # 软件包
└── docs/                               # 文档
```

## 常见问题

### 1. 编译失败

确保已安装所有依赖包，并检查网络连接。

### 2. 烧录失败

- 检查USB连接是否正常
- 确认STM32CubeProgrammer版本支持STM32MP2
- 验证启动模式配置正确

### 3. 系统无法启动

- 检查EMMC启动模式配置
- 验证串口连接和波特率设置（115200）
- 确认镜像烧录完整

## 参考资料

- [正点原子STM32MP2开发板文档](https://www.alientek.com)
- [STM32MP257官方资料](https://www.st.com/en/microcontrollers-microprocessors/stm32mp2-series.html)
- [Buildroot官方文档](https://buildroot.org/downloads/manual/manual.html)
- [STMicroelectronics Buildroot外部树](https://github.com/bootlin/buildroot-external-st)

## 技术支持

如有技术问题，请参考：
1. 正点原子官方论坛
2. ST官方技术支持
3. Buildroot社区邮件列表