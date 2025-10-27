---
title: .NET 無Admin權限安裝
date: 2025-10-26 17:21:05
tags: [NET]
categories: 
    - 程式
---

# **.NET 無Admin權限安裝**

如果因為各種因素沒有管理員Admin權限（Windows），想安裝.NET，可參考以下步驟：

### **1. 下載Binary**

[https://dotnet.microsoft.com/en-us/download/dotnet/8.0](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)

找到 Binaries 下載，並解壓縮

### **2. 設定環境變數**

使用 GUI 開啟會需要管理員權限，可以使用 Powershell，輸入 `Open-EnvironmentVariables`

```
PATH=path\to\sdk\;%PATH%

DOTNET_ROOT=path\to\sdk\

DOTNET_MULTILEVEL_LOOKUP=0
```

### **3. 測試是否安裝成功**

開啟新的 Powershell，輸入 `dotnet`



