# Proxmox

Proxmox VE（英語：Proxmox Virtual Environment，通常簡稱為Proxmox），是一個開源的伺服器虛擬化環境Linux發行版。基於Debian，使用基於Ubuntu的客製化核心

## 安裝前

在實體機器上有兩個安裝方法
- USB
- 光碟

### USB
使用USB製作開機光碟需要使用製作開機USB的程式，這裡我使用<a href="https://rufus.ie/">Rufus</a>來製作，製作過程中請選擇以DD映像模式寫入
![image](proxmox/usb-1.PNG)</br>
![image](proxmox/usb-2.PNG)</br>

## 開始安裝

![image](proxmox/install-1.PNG)</br>

選擇地區"台灣"打一兩個字就有選項給你選
![image](proxmox/install-2.PNG)</br>
![image](proxmox/install-3.PNG)</br>
![image](proxmox/install-4.PNG)</br>
![image](proxmox/install-5.PNG)</br>

## 初次登入

帳號預設為root，根據提示我們可以去IP:8006查看web GUI
![image](proxmox/start-1.PNG)</br>

## 剛安裝完成 Proxmox 後，務必要立即做系統的安全性更新。

### 安裝vim，因為預設得vi輸入方式非常不好用，要先update再安裝，不然會有錯誤

```
apt-get update
apt-get install vim
```


### 變更 ipv4和ipv6 的優先順序為 ipv4 優先(速度會較快)
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


## 將預設認購套件庫取消(官方只是希望使用者能付費來提供遠端的維護)
pico /etc/apt/sources.list
加上這一行並存檔 (6.0-1 版 生效)
deb http://download.proxmox.com/debian jessie pve-no-subscription
再執行更新
~# apt-get update
~# apt-get dist-upgrade -y --force-yes

更新後會將 Proxmox VE 更新至官方目前的最新套件版本

## GUI

以下為GUI畫面，登入時可以選擇語言，翻譯還算滿完整的
![image](proxmox/start-2.PNG)</br>

local裡面有upload可以供上傳ISO檔等資料
![image](proxmox/start-3.PNG)</br>
![image](proxmox/start-4.PNG)</br>

# 

上傳完就可以建立虛擬機 VM ID是虛擬機的編號，從100開始，Name為虛擬機的名稱
![image](proxmox/start-5.PNG)</br>
![image](proxmox/start-6.PNG)</br>

Graphic card可以選擇虛擬機顯卡類型
![image](proxmox/start-7.PNG)</br>

Bus/Device可以選擇SATA等硬碟類型
![image](proxmox/start-8.PNG)</br>

Sockets:CPU數量，Cores:核心數
![image](proxmox/start-9.PNG)</br>
![image](proxmox/start-10.PNG)</br>
![image](proxmox/start-11.PNG)</br>

確認所有配置
![image](proxmox/start-12.PNG)</br>

## 使用虛擬機

安裝好虛擬機會再介面左手邊的位置，點下去跑出該虛擬機的效能監控，基本設定，防火牆設定...等資訊
![image](proxmox/vm-1.PNG)</br>
console點開就是我們虛擬機的畫面，點左側按鈕可以開始、關閉、暫停...等，也可以放大到全螢幕。
![image](proxmox/vm-2.PNG)</br>

## 巢狀虛擬機 Proxmox on Hype-v

如果想在hype-v下安裝proxmox可能會遇到proxmox裡虛擬化技術位開啟的問題，win server2016以上版本可以在power shell內開啟巢狀虛擬化技術，因為使用的機器為win2008，所以這裡就沒有實作跟確認是否可以執行。能確定的是2008無法使用這個指令。
```
Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $true
```
參考網址:https://docs.microsoft.com/zh-tw/virtualization/hyper-v-on-windows/user-guide/nested-virtualization


