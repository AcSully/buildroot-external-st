# 正点原子STM32MP2 buildroot

## 支持正点原子硬件

- [x] STM32MP257 1 + 8版本
- [ ] STM32MP257 2 + 16版本

## 下载源代码

```
git clone -b st/2025.02.5 https://github.com/bootlin/buildroot.git

git clone https://github.com/AcSully/buildroot-external-st.git

cd buildroot


make BR2_EXTERNAL=../buildroot-external-st stm32mp257DAK_core_defconfig

make -j4
```



## 烧录镜像



打开STM32CubxMX，选择output/images/flash_full.tsv，烧录完成后，启动方式修改成EMMC启动.



## 验证

### 接串口后输入进入系统

username: root

password: root

### QT测试

```
/usr/lib/qt/examples/opengl/hellogl2/hellogl2
```
