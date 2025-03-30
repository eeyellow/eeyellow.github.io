---
title: HomeAssistant 01 - 安裝
date: 2025-03-30 22:06:58
tags: [HomeAssistant, Synology]
categories: 
    - NAS
---

# **HomeAssistant 01 - 安裝**

今天的目標是在 Synology NAS 上使用 Docker 運行 HomeAssistant。

先列出配備與環境：

1. **NAS**：Synology DS224+
2. **OS**：DSM 7.2.2-72806 Update 3
3. **CPU**：Intel Celeron J4125
4. **RAM**：18GB (原廠2GB + 擴充16GB)

## **安裝步驟**

1. 套件中心 -> Container Manager

2. Container Manager -> 倉庫伺服器 -> 搜尋 *home-assistant* -> 下載

3. Container Manager -> 容器 -> 新增

    {% asset_img 01.png %}

    * 啟用自動重啟要打勾
    
    * 資源限制要依照自己的環境規格設置，我之後也是要依照使用狀況再調整。

4. 儲存空間設定 -> 設定資料夾 /docker/home_assistant/config : /config

    {% asset_img 02.png %}

5. 環境設定 -> 新增 -> TZ : Asia/Taipei

    {% asset_img 03.png %}

6. 網路 -> 選擇 host

    {% asset_img 04.png %}

7. 設定完成後，等待一段時間，HomeAssistant 就會啟動完成。輸入 `http://NAS_IP:8123` 進入 HomeAssistant。

    {% asset_img 05.png %}

8. 第一次進入需要設定帳號、密碼、家庭所在位置，其他設定就用預設。登入後，就可以看到 HomeAssistant 的介面了。

    {% asset_img 06.png %}
