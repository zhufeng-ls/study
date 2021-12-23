## ac650

> 厂商: 腾达
>
> 型号: 0bda:c811

型号使用 `lsusb` 查看

驱动安装:

```
$ sudo apt update
$ sudo apt install build-essential git dkms
$ git clone https://github.com/brektrou/rtl8821CU.git
$ cd rtl8821CU
$ chmod +x dkms-install.sh
$ sudo ./dkms-install.sh
$ sudo modprobe 8821cu 
```

然后重启