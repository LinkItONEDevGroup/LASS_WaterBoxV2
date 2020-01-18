# LASS_WaterBoxV2

### ***` LASS IS YOURS`***

# **1. 功能介紹** 

水盒子是一台完全開源，可讓Maker視需求自行更換感測項目的水質感測設備。水盒子採用臺灣聯發科設計製造的[LinkIt7697](https://www.facebook.com/CO2.Earth/photos/a.251561691555854/3017834234928572/?type=3&theater)開發版作為微控制器(MCU)，以免費開源軟體ArduinoIDE作為開發環境，同時將常用的Arduino模組集成設計在一塊PCB電路板上，這些功能可經由開源的Library作為驅動，目前的測試程式碼跟主要運作程式碼也是以MIT授權的方式開放所有人使用，程式碼相關內容請參考後續章節[韌體說明](https://paper.dropbox.com/doc/Task-ArSnnSLW1tOUG0HMCmNQeOfpAg-gMGZS1OGSWaFkNvnUZOKw#:uid=195831292420979170624019&h2=3.-%E9%9F%8C%E9%AB%94%E8%AA%AA%E6%98%8E)。

目前水盒子可量測項目為**水溫**、**導電度(Electrical Conductivity, EC)**及**酸鹼度(pH)**等自然水質基本項目，資料可以*.CSV格式(或其他格式)儲存於MircoSD卡內，經WiFi上傳至雲端(LASS或ThingSpeak)或以LoRa/BLE 通訊方式傳輸到其他裝置上；由於自然水體內水質情況會隨著不同地點而有不同的干擾，即使為無工業污染的水體也會有生物膜附著在電極表面造成感測電極干擾，因此建議每周維護檢查電極偏差的情況，確保所量測水質資料具參考價值。

***水盒子是為了擴增***[***LASS環境感測器網路系統***](https://lass-net.org/)***的感測領域而設計的開源專案，單一一台水盒子只能知道一池水的水質狀況，而經由公民科學參與大量布建則可揭露環境/污染變化的分布及趨勢；因此，希望使用者能上傳/分享水盒子的感測資料，一同為我們所生活這塊土地努力。***

# **2. 硬體說明**
- WaterBox主機外殼為120mm×120mm×65mm的ABS材質防水盒，主要功能如下：
![](Docs/assets/images/hardware.png)

## **2.1 使用材料(BOM)**

目前水盒子材料包內容包含內容如下表。

| 項目                                                                           | 數量 | 單位 | 說明                    |
| ---------------------------------------------------------------------------- | -- | -- | --------------------- |
| 水盒子電路板                                                                       | 1  | 片  | 水盒子各模組的集成電路版          |
| LinkIt 7697 開發版                                                              | 1  | 片  | 水盒子的MCU               |
| CR1220電池(SONY)                                                               | 1  | 個  | 水盒子RTC的電池             |
| 塑膠防水工控盒(G278C, 120*120*60)                                                   | 1  | 個  | 主機外殼                  |
| 0.96寸 OLED顯示螢幕128*64 黑底白字                                                    | 1  | 片  | 水盒子系統訊息顯示螢幕(SSH1106)  |
| 太陽能板 3W 110x162mm                                                            | 1  | 片  | 充電用太陽能板               |
| JST2.0單頭線+2p母座/30cm                                                          | 1  | 條  | 太陽能板用電線               |
| Micro USB 傳輸線 1米                                                             | 1  | 條  | 韌體上傳/測試用；主機電池充電用      |
| Micro SD 16GB 超高速 (Class 10)                                                 | 1  | 片  | SD卡；儲存資料用             |
| 聚合物鋰電池-2000mAh 3.7V                                                          | 1  | 個  | 用於主機電源供電              |
| 3M黑色魔鬼氈25mm*40mm                                                             | 1  | 片  | 用於固定電池                |
| 杜邦端子2.54mm母(半鍍金)<br>3P 杜邦母座                                                  | 1  | 組  | 溫度sensor用轉接頭(DS18B20) |
| M3六角細牙銅柱25mm<br>十字圓頭螺絲 M3x12mm<br>五彩六角螺帽M3<br>十字圓頭螺絲 M3*8mm (五彩)<br>HP-5扣式塞頭 | 1  | 組  | 主機固定用                 |

## **2.2 組裝順序**
- pH及EC的固定開孔比較難安裝，安裝時需多加留意。
https://www.dropbox.com/s/p1fkxj5bptwpakw/WaterBox_V2.1.avi?dl=0

## **2.3 充電方式**

A.使用太陽能板充電
B.使用電路板上USB charge，以MicroUSB線(type B)充電(建議使用1.0A以上)

![充電時紅色LED只是燈會亮起](Docs/assets/images/red_led.png)
![充滿電後量綠色的LED燈](Docs/assets/images/red_led.png)

## **2.4 用電分析**
- 目前水盒子運作時用電約為120mA，以套件包所含的聚合物鋰電池(2000mAh)來說，無外部供電情況下約可持續運作13個小時。
- 經實際測試，水盒子搭配3W太陽能板在戶外可連續運作約10天。

| 項目                     | 使用電流(mA)         |
| ---------------------- | ------------------    |
| MCU ( LinkIt 7697 )    | 約120.2mA，峰值150.6mA |
| OLED ( SSH1106 )       | 約16.16mA             |
| pH ( DFRobot SEN0161 ) | 約 28.2mA             |
| EC ( DFRobot DFR300)   | 約 10.5mA             |
| Tempture (DS18B20)     | 還沒測                 |



## **2.5 建議防水方式**
- 原本的ABS防水盒外殼附有防水膠，安裝時須留意膠條接合的地方避免產生孔隙。
- (不用鎖緊)，再以對角的安式將四周螺絲鎖緊，避免一次將單一螺絲鎖緊使盒蓋和盒體之間產生孔隙，造成水氣滲入。
- 針對防水盒開孔部分，建議以中性防水型的矽利康填補空隙，避免水氣由開孔處入滲。
![防水盒上的膠條應該要填滿不能有空隙](Docs/assets/images/waterproof_1.jpeg)
![建議額外使用矽利康將開孔處填滿，同時避免BNC街頭生鏽；矽利康應選用中性防水型](Docs/assets/images/waterproof_2.jpeg)

# **3. 韌體說明**
## **3.1 LinkIt 7697 介紹**
- [Get started (Arduino)](https://docs.labs.mediatek.com/resource/linkit7697-arduino/en/environment-setup)
- [Documents](https://docs.labs.mediatek.com/resource/linkit7697-arduino/en/download)
## **3.2 LinkIt 7697 腳位使用情況**
- LinkIt 7697 腳位功能介紹
![https://docs.labs.mediatek.com/linkit-7697-blocklyduino/linkit-7697-12880255.html](Docs/assets/images/linkit7697_pin.png)



- **LinkIt 7697 各腳位初始狀態**
    下表列出了 LinkIt 7697 每隻腳位的開機初始電位狀態 (高電位為 H、低電位為 L) 及相關說明。
    - **Pin #**：表示該腳位在開發板上的絲印編號。
    - **GPIO #**：表示該腳位在開發板腳位圖和 datasheet 裡所標示的 ****GPIO 編號。
    - **Resetting**：表示按下 LinkIt 7697 **RST** 鍵時的腳位狀態。
    - **Bootloader default**：開機完成後，在 Arduino APP 執行前的腳位狀態。
    
| **Pin #** | **GPIO #** | **Resetting** | **Bootloader deafult** | LASS WaterBox(V2.1)                      | **說明**                                                                       |
| --------- | ---------- | ------------- | ---------------------- | ---------------------------------------- | ---------------------------------------------------------------------------- |
| `P0`      | `2`        | H             | H                      | USB (UART_RX)                            | Resetting 過程中，該腳位會被 CP2102N 拉高。<br>開機完成後，bootloader 將之設定為 UART 模式，因此也會處於高電位。 |
| `P1`      | `3`        | L             | H                      | USB (UART_TX)                            | 開機完成後，bootloader 將之設定為 UART 模式，因此處於高電位。                                      |
| `P2`      | `0`        | L             | L*                     | LoRa module DIO0                         | *將 Arduino BSP 的 bootloader 版本更新至 v0.8.1 後的狀態。                               |
| `P3`      | `39`       | L             | L                      | LoRa module Reset                        |                                                                              |
| `P4`      | `34`       | H             | L                      | SPI_CS for SD Card                       | Resetting 過程中的高電位狀態為晶片本身的預設行為。                                               |
| `P5`      | `33`       | H             | L                      | mode Switch                              | Resetting 過程中的高電位狀態為晶片本身的預設行為。                                               |
| `P6`      | `37`       | L             | H                      | power switch for pH EC module            | 開機完成後，bootloader 會先將之設定為 UART 模式 (後再轉為 GPIO 模式)，因此處於高電位。                     |
| `P7`      | `36`       | L             | L                      | Temperature Pin<br>with 4.7K ohm pull-up |                                                                              |
| `P8`      | `27`       | H             | H                      | I2C_SCK                                  | 開發板本身自帶 I2C 的 pull-up 電路。                                                    |
| `P9`      | `28`       | H             | H                      | I2C_SDA                                  | 開發板本身自帶 I2C 的 pull-up 電路。                                                    |
| `P10`     | `32`       | L             | L                      | SPI_CS for LoRa module                   |                                                                              |
| `P11`     | `29`       | L             | L                      | SPI_MOSI                                 |                                                                              |
| `P12`     | `30`       | L             | L                      | SPI_MISO                                 |                                                                              |
| `P13`     | `31`       | L             | L                      | SPI_SCK                                  |                                                                              |
| `P14`     | `57`       | L             | L                      | Module Power Control                     |                                                                              |
| `P15`     | `58`       | L             | L                      | No Used                                  |                                                                              |
| `P16`     | `59`       | L             | L                      | No Used                                  |                                                                              |
| `P17`     | `60`       | L             | L                      | No Used                                  |                                                                              |



- 參考來源：
## **3.3 GitHub**
- 資料都放在GitHub上，MIT授權
- 分為三個部分
    - 單一功能測試[：UnitTest](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest)
    - 整機程式碼[：](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest)[firmware](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/firmware)
    - 模組驗證[：Verification](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/Verification)
https://github.com/LinkItONEDevGroup/LASS_WaterBoxV2

    - 使用方式：下載後需設定ArduinoIDE，確保有抓到GitHub內附的ArduinoLibrary。
![點選偏好設定](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577419117891_.png)
![在草稿碼簿的位置設定為GitHub檔案的ArduinoLibrary](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577419110061_.png)

## **3.4 單元功能測試**
- **ADS1115**
    - Code[：TestCode_pinState_2.0_ADS1115](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest/TestCode_pinState_2.0_ADS1115)
    - 注意項目：
        - 因為所有模組的供電由P14的腳位控制，所以測試時需先將P14設定為高電位才能啟動模組
        - ADS1115包含4組16bit ADC，因此各組ADC對應如下
            - A0：pH電壓        (輸入端已經分壓，因此量測到的電壓為輸入電壓×0.5)
            - A1：導電度電壓   (輸入端已經分壓，因此量測到的電壓為輸入電壓×0.5)
            - A2：旋鈕  VR1    (順時針轉增加，順時針轉到底為-1)
            - A3：旋鈕  VR2   (順時針轉增加，順時針轉到底為-1)
        - TestCode同時包含SW1(P5)及SW2(P6)狀態測試
    - 測試畫面
![SW2下拉低電位時只能量測 EC的電壓](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577431474512_.png)
![SW2上拉低電位時只能量測 pH的電壓](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577431565382_.png)

- **DS3231 RTC功能**
    - Code：[TestCode_ds3231_v2.0](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest/TestCode_ds3231_v2.0)
    - 注意項目：
        - 測試碼改自RTCLib的範例
        - 因為所有模組的供電由P14的腳位控制，所以測試時需先將P14設定為高電位才能啟動模組
    - 測試畫面
![上傳成功後，Arduino的序列埠監空視窗會顯示目前RTC的訊息](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577432804303_.png)

- **SD 卡寫入功能**
    - Code：[TestCode_SD_2.0_CardInfo](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest/TestCode_SD_2.0_CardInfo)
    - 注意項目
        - 因為所有模組的供電由P14的腳位控制，所以測試時需先將P14設定為高電位才能啟動模組
    - 測試畫面
![上傳成功後，Arduino的序列埠監空視窗會顯示目前SD卡的資料](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577432959516_.png)

- **OLED 顯示功能**
    - Code：[TestCode_SH1106_SystemInfo_v2.0](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest/TestCode_SH1106_SystemInfo_v2.0)
    - 注意項目
        - 因為所有模組的供電由P14的腳位控制，所以測試時需先將P14設定為高電位才能啟動模組
    - 測試畫面
![上傳成功後，主機上的OLED會開始顯示訊息](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577433144263_file.jpeg)

- **LoRa 功能**
    - Code
        - 接收資料[：TestCode_LoRaReceiver_2.0](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest/TestCode_LoRaReceiver_2.0)
        - 發送資料[：](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest/TestCode_LoRaReceiver_2.0)[TestCode_LoRaSender_2.0](https://github.com/kylecat/LASS_WaterBoxV2/tree/master/UnitTest/TestCode_LoRaSender_2.0)
    - 注意項目
        - 因為所有模組的供電由P14的腳位控制，所以測試時需先將P14設定為高電位才能啟動模組
        - 由於LinkIt7697對於外部中斷的function跟Arduino不同，應此需修改LoRa.cpp內的程式碼
        - ArduinoLibrary裡面的函式庫已完成修改
## **3.5 整機運作功能**
- 詳細的運作內容請參考程式碼內註解
![LASS WaterBox_V2.1運作流程](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577502091191_.png)

# **4. 購買**
- 水盒子所需所有套件均可以在ICShop上購買到

https://www.icshop.com.tw/product_info.php/products_id/26350

![](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577417363415_.png)

- 加購與選項說明
# **5.資料上傳**
- 預設上傳方式為ThingSpeak及LASS平台；LASS已有全台灣水質開放資料彙整平台『山河事件簿』
- 《山河事件簿》介紹：
    - [+LASS 專案-山河事件簿](https://paper.dropbox.com/doc/LASS-cV5q46R2fGvCgMtfP8cg3) 
## **5.1 LASS上傳資料設定：**[**《山河事件簿》**](https://riverlog.lass-net.org/)
![水盒子API、格式資料、傳輸圖](https://paper-attachments.dropbox.com/s_C5682542114918DF255C865C0345EEAD57AC86A92591AB9B9FD908C31C90CEC4_1575478126281_Untitled+Diagram.jpg)

- **傳輸方式：**
    - 基本上有三種腳色：水盒子/中研/山河事件簿
    - 水盒子先透過restful API上傳資料 → 中研的API轉發LASS MQTT → 山河事件簿訂閱 MQTT sever


- ***RESTful API 說明：***
    https://pm25.lass-net.org/Upload/waterbox_tw.php?topic=[]&device_id=[]&key=[]&msg=[]
    - 需要參數有
        topic => MQTT用的頻道 預設:"WaterBox_TW"
        device_id => 設備編號 使用習慣12碼 MAC address EX:"9C65F920C020"
        key => 用於識別作者/來源 預設是"NoKey" 使用HMAC
        msg => MQTT所要發送的內容 格式如下:
            |key_1=value|key_2=value|key_3=value|key_4=value|key_5=value|
    - MQTT訊息各項目 利用"="連接數值並且彼此利用"|"符號隔開
        - 例如這樣 => |KEY=value|

- ***RESTful使用範例：***
        topic=WaterBox_TW
        device_id=XXXXXXXXXXXX
        key=NoKey
        msg=MQTTmessage
    - 傳送參數如下:
    https://pm25.lass-net.org/Upload/waterbox_tw.php?topic=WaterBox_TW&device_id=XXXXXXXXXXXX&key=NoKey&msg=MQTTmessage
    - 成功的話就會在LASS MQTT平台 LASS/Test/WaterBox_TW/XXXXXXXXXXXX 頻道顯示 MQTTmessage，訂閱LASS/Test/WaterBox_TW/# 即可接收此專案各感測器資料


- **MQTT訊息項目**(***MQTTmessage***)**清單與參考數值如下：**
        device=Linkit7697                //感測器主控制板
        device_id =9C65F920C020          //感測器編號，建議用WiFi Mac
        ver_app = 1.1.0                  //軟體版本
        app = WaterBox_TW                //專案名稱
        FAKE_GPS = 1                     //是否使用FAKE_GPS
        gps_lat = 25.1933                //緯度
        gps_lon = 121.787                //經度
        date = 2019-03-21                //日期
        time = 06:53:55                  //時間
        tick=714436.97
        s_t0                              //水溫(-20.0~150.0C)
        s_ph                              //酸鹼度(0.00~-14.00)
        s_ec                              //導電度(0~200000 uS/cm)
        s_Tb                              //濁度(0~10000 NTU)
        s_Lv                              //水位(0.000~20.000 M)
        s_DO                              //溶氧(DO 0.00~12.00 mg/L)
        s_orp                             //氧化還原電位(ORP -2000~2000 mV)



## **5.2 ThingSpeak設定**
- ThingSpeak 的申請和設定可以參考下列網址：
https://makerpro.cc/2016/02/upload-sensor-data-from-ameba-to-thingspeak/

![上傳後的資料畫面](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1576139139765_.png)




# **6. 擴充性**

水盒子除了目前配置的酸鹼度(pH)、導電度(EC)、溫度之外等三樣感測sensor之外，電路板上有預留3組I2C接點，可擴充其他I2C介面的感測模組及0-5V Analog 輸出訊號的模組。

## **6.1 可擴充模組**

目前可擴充的水質相關模組清單如下，這些模組原廠均開放Arduino sample code供使用者測試。

| **Sensing item** | **Company**     | **protocols**   | **Hardware interface**             | **Sample Code** | **Where to buy it**                                                                     |
| ---------------- | --------------- | --------------- | ---------------------------------- | --------------- | --------------------------------------------------------------------------------------- |
| pH               | AtlasScientific | I2C             | EZO™ pH Circuit                    | Yes             | https://www.atlas-scientific.com/ph.html                                                |
| EC               | AtlasScientific | I2C             | EZO™ Conductivity Circuit          | Yes             | https://www.atlas-scientific.com/conductivity.html                                      |
| ORP              | AtlasScientific | I2C             | EZO™ ORP Circuit                   | Yes             | https://www.atlas-scientific.com/orp.html                                               |
| Temperture       | AtlasScientific | I2C             | EZO™ RTD Circuit                   | Yes             | https://www.atlas-scientific.com/temperature.html                                       |
| DO               | AtlasScientific | I2C             | EZO™ Dissolved <br>Oxygen Circuit  | Yes             | https://www.atlas-scientific.com/dissolved-oxygen.html                                  |
| Color            | AtlasScientific | I2C             | 2.54mm Dupont Line                 | Yes             | https://www.atlas-scientific.com/product_pages/probes/ezo-rgb.html                      |
| Pressure         | AtlasScientific | I2C             | 2.54mm Dupont Line                 | Yes             | https://www.atlas-scientific.com/product_pages/pressure/ezo-prs.html                    |
| flow             | AtlasScientific | I2C             | EZO™ Embedded Flow Meter Totalizer | Yes             | https://www.atlas-scientific.com/flow-meters.html                                       |
| distance         | MaxBotix        | PWM             | 2.54mm Dupont Line                 | **NO**          | https://www.maxbotix.com/                                                               |
| Turbidity        | DFRobot         | Voltage analoge | 2.54mm Dupont Line                 | Yes             | https://www.dfrobot.com/product-1394.html?search=turbidity&description=true             |
| Ammonium         | Veriner         | Voltage analoge | British telephone socket           | Yes             | https://www.vernier.com/products/sensors/ion-selective-electrodes/labquest-ise/nh4-bta/ |
| Calcium          | Veriner         | Voltage analoge | British telephone socket           | Yes             | https://www.vernier.com/products/sensors/ion-selective-electrodes/labquest-ise/ca-bta/  |
| Nitrate          | Veriner         | Voltage analoge | British telephone socket           | Yes             | https://www.vernier.com/products/sensors/ion-selective-electrodes/labquest-ise/no3-bta/ |
| Chloride         | Veriner         | Voltage analoge | British telephone socket           | Yes             | https://www.vernier.com/products/sensors/ion-selective-electrodes/labquest-ise/cl-bta/  |
| Potassium        | Veriner         | Voltage analoge | British telephone socket           | Yes             | https://www.vernier.com/products/sensors/ion-selective-electrodes/labquest-ise/k-bta/   |

# **7. 第一次就上手**
## **7.1 購買**
- 到ICShop買(見[4.-購買](https://github.com/kylecat/LASS_WaterBoxV2/blob/master/README.md#4-%E8%B3%BC%E8%B2%B7) )
## **7.2 組裝**
- ICShop 將會提供代客組裝服務 (當然會需要收組裝費用)
- 須留意的細節
![箭頭指的位置都是GND](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577685785560_.png)

    - 溫度、pH、EC模組連接的方向(正面看，由左至右依序為GND、VCC、訊號輸出)
    - 充電時應先關閉電源，線都接好之後再供電(USB Charge要多留意)
- 組裝好之後的測試流程
    - 裝好LinkIt 7697、OLED、RTC電池、SD卡、溫度、pH、EC模組
    - 連接電池、太陽能板的接頭，先把開關關閉
            (最後再送電的觀念很重要)
    - 各模組的單元測試都跑一遍，確認畫面/序列埠顯示正常
            (見[3.4-單元功能測試](https://github.com/kylecat/LASS_WaterBoxV2/blob/master/README.md#34-%E5%96%AE%E5%85%83%E5%8A%9F%E8%83%BD%E6%B8%AC%E8%A9%A6) )
![](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577686559596_.png)

## **7.3 充電/開機**
- 如何充電：
    - 用Micro USB 接好 USB Charge後，再接上電源！！
    - [注意順序]先接電源再插USB，有可能再還沒插好前就會短路！！
    - LinkIt 7697 上的USB無法對電池充電，但可對所有模組供電；因此測試時建議關閉電路板左上方的電源開關。
- 開機畫面
    - OLED顯示畫面(基本上沒有缺字就可以了)
    - ArduinoIDE Serial port 設定：
        - Baud rate：9600
        - 有些人的IDE中文顯示會是亂碼(這是IDE產生的差異，建議使用最新版的IDE)
    - 開機後序列埠輸出內容如下圖1-圖5
![1. 開啟啟動畫面](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577686822118_.png)
![2. 說明畫面](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577686949803_.png)

![3. WiFi  連線成功畫面](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577686998174_.png)
![4. 分析時的系統訊息](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577690431962_.png)
![5. 設定模式時的系統訊息](https://paper-attachments.dropbox.com/s_3CC555A8CFF3D8CDA00E57144922BAEC389C3029AF20AB1D8E5FA1CA7DB99776_1577689987421_.png)

# **8. 上場域**
## **8.1 初步使用與暖身**
- 建立準備用的確認清單(Check List)，建議包含下列項目(?表示查核後數值)：
    - 水盒子電源確認(??.? V / 4.15V)
    - 水盒子感測項目校正/確認：
        - pH：4.0(??.?)/7.0(??.?)/10.0(??.?)
        - EC：0.0(?.?) / 1413(????)
        - Temp：28.5(??.?)
- 出發前的準備
    - 確認安裝日之前幾日天候狀況，下雨後溪水可能會在隔兩三天後暴漲
    - 除了設備之外，雨鞋、安裝器材、安全防護設備也應一併確認
- 建議先到預定安裝的場域完成場勘，場勘內容包括：
    - 拍攝安裝地點的上下游
    - 確認安裝位置的日照情況
    - 確認安裝位置是否為溪水上升範圍內
## **8.2 上場域前準備**
- 設備準備
    - 水盒子
    - Sensor
    - 固定用工具(矽利康/塑鋼土)
- 外部輔助項目
    - 筆電(確認有電)
    - 三用電表(確認有電)
    - 螺絲起子
    - 刷子(清除石頭上青苔)
    - 工安裝備
## **8.3 安裝**
- 避免下雨天安裝
- 注意安全
- 應戴工地安全帽及穿著反光背心
## **8.4 維護**
- 維護流程
    1. 檢查設備狀態：檢查設備是否還在
    2. 檢查現場狀態：確認安裝地點上下游是否安全
    3. 檢查電源狀態：確認是否還有電
- 現場維護時須留意的地方
    - 現場只做幾件事情：
        1. sensor清潔
        2. 電池電壓檢查/更換
        3. SD卡更換
        4. 用標準液確認sensor偏移狀況
    - 如現場氣候不佳，建議僅檢查外觀及是否有電即可
- 室內校正流程
    1. 清潔sensor上附著物質
    2. 偏移確認：放到標準液當中確認讀值並記錄校正前讀值
    3. 進行校正
    4. 校正後再次進行偏移確認，並記錄校正後讀值
## **8.5 後續分析(作者外出取材中)**
    1. 基本項目：
        * 趨勢圖
        * 各測項的變化的細節及代表意義
    2. 其他：作者外出取材中(?)


# **9. 入手前確認事項**
- Maker 套件，著重 DIY 與客製化，請有正確的預期


