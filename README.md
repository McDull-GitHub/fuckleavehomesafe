# 安心出行 Android 離線安心版
透過改修APK，移除網絡使用權限。隻APP再無可能傳送任資料去任何伺服器，達至100%安心。




## 下載
<a href="https://raw.githubusercontent.com/Plarigne/leavehomesafe-android-block-network/main/LeaveHomeSafe_1.1.4_%E5%AE%89%E5%BF%83%E7%89%883.apk">LeaveHomeSafe_1.1.4_安心版3.apk</a>

首次使用可能會卡住喺 "載入中"，Kill咗個APP，再開即可。



MD5: 084d3b38a141fe252933a0b37f3aff9b


## 原理
1. 利用 APKStudio 修改原版APK，將 AndroidManifest.xml 裡嘅  `<uses-permission android:name="android.permission.INTERNET"/>` 移除。
1. 另外由於無網絡權限會彈APP，所以亦要廢除 okhttp 同埋 firebase 程式庫。 

## 如何驗証 APK 真係無網絡權限?

可使用 https://www.sisik.eu/apk-tool 查看權限:

如下圖：
<img src="2.jpg" alt=""  width="100%" />

另外，應用程式資訊亦會顯示: 未使用任何數據：

<img src="1.jpg" alt=""  width="300" />


## 呢隻 APP 真係安全?
一般嚟講，唔建議用家安裝不明來歷嘅 APK，因為可能俾不法之徒加料。但如上所說，此 APK 已無網絡權限，都做唔到咩花樣，唔會偷到嘢。


## 你呃人，無 Source Code 嘅?
其實我都無Java Source Code，如要睇 Smali Source Code，只要用 apkstudio 打開 apk 就睇到晒。



## 我都係信唔過你個APK，有冇得自己整?
可以，不過可能要有少少心機先整到。

1. 下載 apkstudio (https://vaibhavpandey.com/apkstudio/)
1. 下載原版安心出行APK (https://apkpure.com/tw/leavehomesafe/hk.gov.ogcio.leavehomesafe)
1. 用 apkstudio 載入 apk
1. 打開 AndroidManifest.xml，然後移除以下兩行
    ```
    <uses-permission android:name="android.permission.INTERNET"/>
    ```
    ```
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE"/>
    ```
1. 打開 \smali\okhttp3\Dns$1.smali，搵 .method public lookup ，喺 Line 42 (.end annotation) 行加以下嘅 code

    ```
    new-instance p1, Ljava/net/UnknownHostException;

    const-string v0, "hostname == null"

    invoke-direct {p1, v0}, Ljava/net/UnknownHostException;-><init>(Ljava/lang/String;)V

    throw p1
    ```
1. 打開 \smali\com\google\firebase\installations\e.smali ， 移除呢3句
    ```
    iget-object v0, p0, Lcom/google/firebase/installations/e;->c:Lcom/google/firebase/installations/f;

    iget-boolean v1, p0, Lcom/google/firebase/installations/e;->d:Z

    invoke-static {v0, v1}, Lcom/google/firebase/installations/f;->a(Lcom/google/firebase/installations/f;Z)V
    ```
    
1. 儲存所有檔案後，按Build
2. Build 完後，準備Sign Key，按 Sign APK
3. 此時已生成APK，可以安裝測試
  
