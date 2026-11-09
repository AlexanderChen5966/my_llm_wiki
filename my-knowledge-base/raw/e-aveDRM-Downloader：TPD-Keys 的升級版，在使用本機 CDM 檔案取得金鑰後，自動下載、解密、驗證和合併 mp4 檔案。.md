---
title: "e-ave/DRM-Downloader：TPD-Keys 的升級版，在使用本機 CDM 檔案取得金鑰後，自動下載、解密、驗證和合併 mp4 檔案。"
source: "https://github.com/e-ave/DRM-Downloader"
author:
published:
created: 2026-11-03
description:
tags:
  - "clippings"
status: processed
---
## DRM下載器

## 這段腳本的作用是什麼？

這是 TPD-Keys 的修改版，它會在取得金鑰後自動下載、解密、驗證並合併 mp4 檔案。此版本專為音訊和視訊僅使用相同密鑰的串流媒體而設計。

## PATH 要求

1. [Python](https://www.python.org/downloads/release/python-390/)
2. [ffmpeg](https://ffmpeg.org/download.html)
3. [mp4解密](https://www.bento4.com/downloads/)
4. [aria2](https://github.com/aria2/aria2)
5. [yt-dlp](https://github.com/yt-dlp/yt-dlp)

## Python 要求

1. 請求
2. 芙烈達
3. frida-tools
4. 原型緩衝區
5. Pythoncryptodome
6. pywidevine
7. httpx

## 如何使用此程式（Google瀏覽器）

### 準備

1. 取得 CDM 檔案（.wvd 檔案）（請參閱下文，了解如何使用 Android Studio 模擬器取得 CDM 檔案）
2. 安裝 [Tampermonkey](https://www.tampermonkey.net/) 瀏覽器擴充功能
3. 在 Tampermonkey 中 安裝 [EME Logger 腳本](https://greasyfork.org/en/scripts/373903-eme-logger)
4. 按 CTRL+SHIFT+I 開啟開發者工具
5. 在開發者視窗的「控制台」標籤中，將下列內容新增至篩選框中：MediaKeySession::generateRequest
	- 這樣，當影片載入時，PSSH 資訊就會在控制台標籤中彈出。
6. 在開發者視窗的「網路」標籤中，將以下內容新增至篩選框中：index.mpd
	- 這樣一來，當影片載入時，index.mpd 就會出現在「網路」標籤中。也可能是 manifest.mpd。
7. （僅限模式 7）在開發者視窗的「網路」標籤中，將以下內容新增至篩選框中：method:POST
	- 找到第二個彈出的“?specConform=true”，點擊它，然後在請求頭中向下滾動，
		- 找到 `X-Dt-Custom-Data` 並複製該值作為您的 -xdtcd 值

### 運行腳本

1. 您可以不帶任何參數來執行腳本，系統會提示您輸入所需資訊。
2. 您也可以透過傳遞參數來運行腳本，而無需輸入提示。 `drmdl.py -out "OUTPUT FILE PATH" -pssh "PSSH_HERE" -url "index.mpd URL HERE" -xdtcd "X-Dt-Custom-Data value here"`

### 所有模式的選項

- \-輸出“path/to/outputfile.mp4”
- \-wvd "WVD 檔案路徑"
- - 可選。預設為“./file.wvd”。
- \-url "index.mpd 的 URL"
- \-pssh "PSSH 值"

### 所有模式均提供可選選項

- \-cc "國家代碼"
- - 設定所需代理國家的兩位字母國家代碼。

### 每種模式的特定選項

- 使用 -selection 選項選擇模式。預設模式為 7。

#### 模式 1（通用，無標頭）

- \-lic "許可證伺服器網址"

#### 模式 2（通用模式，附通用標頭）

- \-lic "許可證伺服器網址"

#### 模式 3（通用，帶有來自 DRMHeaders.py 的自訂標頭）

- 在 DRMHeaders.py 中設定所需的自訂標頭。

#### 模式 4（JSON Widevine 挑戰，標頭來自 DRMHeaders.py）

- 在 DRMHeaders.py 中設定所需的自訂標頭。
- \-lic "許可證伺服器網址"
- \-pid "此處填寫進程 ID"

#### 模式 5（Canal+ 直播電視）

- \-頻道“EpgId 在這裡”
- \-live\_token "此處填入即時令牌"

#### 模式 6（YouTube）

- \-lic "許可證伺服器網址"

#### 模式 7（帶有 X-Dt-Custom-Data 的通用標頭）

- \-xdtcd "此處填入自訂 X-Dt-Custom-Data 值"
- \-lic "許可證伺服器網址"
	- 可選。預設值可在腳本中變更。預設值為 [https://lic.drmtoday.com/license-proxy-widevine/cenc/?specConform=true](https://lic.drmtoday.com/license-proxy-widevine/cenc/?specConform=true)

## 常見問題

###### start\_process 時出現 FileNotFoundError 錯誤

請確保已安裝所有依賴項並將其新增至 PATH 環境變數。

## 如何取得 CDM 文件

### 透過從安卓裝置提取金鑰來建立 wvd（Widevine 裝置檔案）。

我按照這個指南從 Android Studio 模擬器中提取了它： [https://forum.videohelp.com/threads/408031-Dumping-Your-own-L3-CDM-with-Android-Studio/](https://forum.videohelp.com/threads/408031-Dumping-Your-own-L3-CDM-with-Android-Studio/)

#### 先決條件

1. 首先請確保您的電腦支援 Intel HAXM。如果不支持，您可能需要使用安卓手機。
2. 在 BIOS 中啟用 VT-x 和 VT-d
3. 停用 Windows 上的虛擬機器管理程式

#### 設定模擬器

1. 安裝 Android Studio
2. 當 Android Studio 提示時，請安裝 Intel HAXM。
3. 創建設備
4. Pixel 6
5. Pie 28 / 安卓 9.0
6. 運行新的安卓設備

#### 為所有內容安裝 Python 依賴項

```
pip install frida
pip install frida-tools
pip install protobuf
pip install pycryptodome
pip install requests
pip install httpx
pip install pywidevine
```

#### 在模擬器上啟動 Frida 伺服器

##### 準備

1. 下載 frida-server-xxx-android-x86.xz [https://github.com/frida/frida/releases](https://github.com/frida/frida/releases)
2. 請確保它與您透過 pip 安裝的版本相同。
3. 解壓縮檔案（我用的是peazip），然後將檔案名稱重新命名為「frida」。
4. 將你重新命名為 frida 的檔案移到 `C:\Users\yourname\AppData\Local\Android\Sdk\platform-tools`

##### 命令

1. 開啟命令提示字元並導航到同一目錄 `cd C:\Users\yourname\AppData\Local\Android\Sdk\platform-tools`
2. 透過執行以下命令檢查 Android 虛擬機器是否正在執行 `adb devices`
3. 現在執行以下命令，在安卓裝置上啟動 Frida 伺服器。

```
adb push frida /sdcard/
adb shell
su
cp /sdcard/frida /data/local/tmp
chmod +x /data/local/tmp/frida
/data/local/tmp/frida
```

#### 運行 wvdumper

[https://github.com/wvdumper/dumper](https://github.com/wvdumper/dumper)

`python dump_keys.py`

#### 匯出密鑰文件

1. 在安卓虛擬機器上開啟 Chrome 瀏覽器，然後存取任何受 DRM 保護的影片。

您的 CDM 金鑰檔案將產生在 key\_dumps 資料夾中：

`client_id.bin` 和 `private_key.pem`

該指南建議將它們重命名為 `device_client_id_blob` “和”。 `device_private_key`

#### 從密鑰檔案建立 device.wvd 檔案：

1. 新建一個資料夾
2. 其中加入 device\_client\_id\_blob 和 device\_private\_key 文件
3. 然後建立一個名為 output 的新資料夾。
4. 然後運行此命令在輸出資料夾中建立 wvd 文件

```
pywidevine create-device -k device_private_key -c device_client_id_blob -t "ANDROID" -l 3 -o output
```

將輸出資料夾中的檔案重新命名為 file.wvd，並將其移至與 drmdl.py 相同的資料夾中。或者，您可以將其重新命名為任何您喜歡的名稱，並將其新增 `-wvd "pathtofile.wvd"` 至命令中。

```
python drmdl.py -out "file.mp4" -pssh "" -url "" -xdtcd "" -wvd "pathtofile.wvd"
```