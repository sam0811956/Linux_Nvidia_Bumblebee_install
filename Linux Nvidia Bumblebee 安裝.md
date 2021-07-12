# Linux Nvidia Bumblebee 安裝

## 先偵測本機上的顯卡
```
# lspci |egrep 'VGA|3D'
```
看看是否有 VGA、3D controller
如有的話，代表本顯卡有 Bumblebee 功能(通常是有M的型號)。

## 將含有 Nvidia 的相關套件移除
```
# apt remove --purge nvidia* libnvidia* bumblebee* primus* -y
```
## 將 nouveau 驅動加入黑名單
```
# nano /etc/modprobe.d/blacklist.conf
blacklist nouveau
```
加完後先重新啟動電腦
```
# reboot
```
# 在 /etc/apt/source.list 加入 backports
```
deb http://deb.debian.org/debian/ buster main non-free contrib
deb-src http://deb.debian.org/debian/ buster main non-free contrib

deb http://security.debian.org/debian-security buster/updates main contrib non-free
deb-src http://security.debian.org/debian-security buster/updates main contrib non-free

deb http://deb.debian.org/debian/ buster-updates main contrib non-free
deb-src http://deb.debian.org/debian/ buster-updates main contrib non-free

deb http://deb.debian.org/debian/ buster-backports main contrib non-free
deb-src http://deb.debian.org/debian/ buster-backports main contrib non-free
```
http://deb.debian.org/debian/ 可換成所用的鏡像站

## 啟用對 i386 架構支持
```
# dpkg --add-architecture i386
```
## 更新套件
```
# apt update && apt upgrade -y
```
## 安裝 Nvidia-detect
```
# apt install nvidia-detect
```
執行 `# nvidia-detect`
安裝系統推薦的套件，例如: nvidia-legacy-xxx-driver
```
# apt install nvidia-legacy-xxx-driver
```

## 安裝 Bumblebee

```
# apt install bumblebee-nvidia primus primus-libs:i386 mesa-utils -y
```
## 安裝 VirtualGL 3D加速器

```
wget https://sourceforge.net/projects/virtualgl/files/2.6.3/virtualgl_2.6.3_amd64.deb -P /tmp/
# dpkg -i /tmp/virtualgl_*.deb

如果有錯誤訊息
# apt -f install
```
# 軟連結 glxspheres64 
```
# ln -s /opt/VirtualGL/bin/glxspheres64 /usr/local/bin/
```
## 將使用者加入 bumblebee 群組
```
 # adduser $USER bumblebee
```
## 如何驅動 bumblebee 功能
執行 optirun (要開啟的程式)

例如:要使用 render，需要用到顯卡功能
```
optirun render
```

## 查看獨顯狀態
```
cat /proc/acpi/bbswitch
```
如果是 OFF ，代表 bumblebee 顯卡未啟動。
則是 ON，代表bumblebee 顯卡已啟動。

# 查看 nvidia-setting
```
# optirun -b none nvidia-settings -c :8
```