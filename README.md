# Proxmox

Proxmox VE（英語：Proxmox Virtual Environment，通常簡稱為Proxmox），是一個開源的伺服器虛擬化環境Linux發行版。基於Debian，使用基於Ubuntu的客製化核心

## 安裝

在實體機器上有兩個安裝方法
- USB
- 光碟

## USB
使用USB製作開機光碟需要使用製作開機USB的程式，這裡我使用<a href="https://rufus.ie/">Rufus</a>來製作，製作過程中請選擇以DD映像模式寫入
![image](proxmox/usb-1.PNG)</br>
![image](proxmox/usb-2.PNG)</br>

如果想在hype-v下安裝proxmox可能會遇到proxmox裡虛擬化技術位開啟的問題，win server2016以上版本可以在power shell內開啟巢狀虛擬化技術
```
Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $true
```
參考網址:https://docs.microsoft.com/zh-tw/virtualization/hyper-v-on-windows/user-guide/nested-virtualization

剛安裝完成 Proxmox 後，務必要立即做系統的安全性更新。

1.安裝vim
```
apt-get update
apt-get install vim
```


2.變更 ipv4和ipv6 的優先順序為 ipv4 優先(速度會較快)
某些更新站台或政府網站會同時回應 ipv4和ipv6，但ipv6卻無法正常回應，
這時在預設以 ipv6 為優先的情況下會造成連不上或系統無回應.
而遇到ipv6無回應的情況下會等待約21秒的時間才會切換回 ipv4，而造成等待的困擾.

關閉ipv6/只使用ipv4
先修改 /etc/gai.conf
pico /etc/gai.conf

找到
#precedence ::ffff:0:0/96  100
這一行，將前面的 # 刪除並存檔(立即生效)
precedence ::ffff:0:0/96  100


3.將預設認購套件庫取消(官方只是希望使用者能付費來提供遠端的維護)
pico /etc/apt/sources.list
加上這一行並存檔 (6.0-1 版 生效)
deb http://download.proxmox.com/debian jessie pve-no-subscription
再執行更新
~# apt-get update
~# apt-get dist-upgrade -y --force-yes

更新後會將 Proxmox VE 更新至官方目前的最新套件版本
