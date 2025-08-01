---
title: "桌面級氣象站：樹梅派4B × BME680"
description: "<del>因為想讓樹莓派做一些像是樹莓派會做的事（？</del>"
date: 2024-03-29
updated: 2024-03-29
slug: "rpi4b_bme680"
taxonomies:
  tags: [zh_TW, Raspberry Pi, IoT, BME680, Sensor, TSDB, InfluxDB, Dashboard, Grafana, Prometheus]
---

<!-- # 桌面級氣象站：樹梅派 × BME680 -->

因為想讓樹莓派做一些像是樹莓派會做的事（？，
所以買了一顆 BME680 感測器來玩玩。

## 硬體選擇

### 溫濕度感測器

溫濕度感測器的選擇有很多，常見的有 DHT11、DHT22，以及 BMP180、BME280 等，
其中的功能、感測項目、範圍和精度都不相同，電路接法也各不相同，看下面的參考。

DHT11、DHT22、BMP180 僅支援溫度（Temperature）和溼度（Humidity），BME280 另外支援了氣壓（Pressure），BME680 除了上述的功能，還外加了感測揮發性有機化合物（VOC）。
更多的功能、更高的精度也就意味著更高的價格。
<del>但畢竟是難得買個玩具，當然是選可玩性高的。</del>

至於電路，好在為了便於開發，電子材料行上賣的通常都有打包好了的模組，用杜邦線插到 樹莓派＼Arduino 上的 GPIO 就可以直接使用。

BME680 是 BOSCH 出品的感測器，對就是那個做家電品牌。
老師說過做硬體第一步就是去查 Datasheet。
至於打包好的模組，模組製造商也會提供相應的 Datasheet，包含了電路的接法和一些開發資料。
電路理所當然會改變感測器的輸入輸出，所以以模組的文檔為準。

參考：

- Random nerd tutorials: [DHT11 vs DHT22 vs LM35 vs DS18B20 vs BME280 vs BMP180](https://randomnerdtutorials.com/dht11-vs-dht22-vs-lm35-vs-ds18b20-vs-bme280-vs-bmp180/)
- Bosch Sensortec: [Gas Sensor BME680](https://www.bosch-sensortec.com/products/environmental-sensors/gas-sensors/bme680/)
- Waveshare Wiki: [BME680 Environmental Sensor](https://www.waveshare.net/wiki/BME680_Environmental_Sensor)

### 樹梅派4B

<del>樹梅派沒什麼好介紹的。</del>樹梅派上有一條又粗又長的 GPIO 接口，只在大學的時候做過相關的專案<del>（作業）</del>，有了自己的樹梅派之後也是把它拿來當成 Headless 的 Linux 電腦來使用，總想能有一天能自己玩玩 IoT。

要注意的是，在這個專案中使用到了 Docker，而一些 Docker container 會需要使用 64-bit 的作業系統，包括 64-bit Kernel 和 64-bit Userspace。我第一次知道 Kernel 和 Userspace 的架構可以不同...
然而，不知道為何我的 Raspberry Pi OS 是 64-bit Kernel + 32-bit Userspace，然後就不得不重灌了...

作業系統的架構可以通過以下指令檢查：

- The kernel architecture: `uname -m`.
- The userspace architecture: `getconf LONG_BIT`.
- APT/DPKG architecture: `dpkg --print-architecture`

參考：

- LinuxServer.io: [A Farewell To Arm(hf)](https://www.linuxserver.io/blog/a-farewell-to-arm-hf)

## 專案總覽

- 感測器：BME680
- 開發平台：Raspberry Pi
- 開發環境：Python
- 資料庫：InfluxDB
- 儀錶板：Grafana

這個專案只是最基本的把東西串接起來，比什麼什麼課程的期末專題廢得多。

## 硬體組裝（實體層）

首先是選擇通訊協定，我使用的 BME680 模組支援 I2C 和 SPI 實體層通訊協定，使用不同的協定電路接法不同，如何選擇實體層通訊協定可以看下面的參考。
這裡選擇 I2C，絕對不是因為我們使用的範例程式僅支援 I2C。


接線之前記得關機斷電，關機之前記得先打開 I2C 和 SPI 的支援，使用指令：

```bash
sudo raspi-config
```

在 `Interfacing Options` 中啟用 `I2C` 或 `SPI`。檢查對應的 kernel modules 是否被成功載入：

```bash
lsmod | grep "i2c\|spi"
```

之後接線之前記得關機斷電，這很重要所以說兩遍。

把感測器和樹梅派接起來沒有什麼難度，就和 Pen pineapple apple pen 一樣，只要照著感測器的 Datasheet 插到樹梅派的 GPIO 上即可。

要注意的是，樹莓派 4B 官方外殼沒給 GPIO 留洞，畢竟我買的時候也沒考慮真的要玩 GPIO，接好感測器後上蓋基本上是沒法蓋回去的，走線還會卡到官方散熱模組。

參考：

- [嵌入式系統中 I2C 和 SPI 通訊協定比較](https://www.eagletek.com.tw/post/i2c-spi-protocols)

## 安裝 BME680 驅動程式

專案的平臺使用 樹莓派上標準的 Python，使用 Package pimoroni/bme680-python。

由於有些作業系統的程式是依賴 Python 的，所以在 Linux 上安裝 Python 的 Packages 時，不能直接使用 `pip` 安裝，通常會建議在專案資料夾下使用虛擬環境 `virtualenv`。方法略。

```bash
pip install bme680
```

驅動程式碼中提供了範例使用程式，其中的 `read-all.py` 用於讀取所有參數，我們直接拿來用。

參考：

- GitHub: [pimoroni/bme680-python](https://github.com/pimoroni/bme680-python)

## 建立時序資料庫：InfluxDB

InfluxDB 是一個時序資料庫（TSDB），顧名思義就是把資料按時間存儲。

我們使用開源版本的 InfluxDB OSS v2。
InfluxDB 的選擇將會在附錄章節 InfluxDB v. Prometheus 討論。

InfluxDB OSS v2 可以通過 Docker 安裝。（註：範例中的地址 `127.0.0.1:8086` 為了安全僅限本機，若要開放，可以使用任意 Web 伺服器做反向代理，同時可以考慮啟用 HTTPS。）

```yaml
services:
  influxdb:
    container_name: influxdb
    image: influxdb:2
    restart: unless-stopped
    ports:
      - 127.0.0.1:8086:8086
    volumes:
        - "./data:/var/lib/influxdb2"
        - "./config:/etc/influxdb2"
```

安裝完成後，使用瀏覽器訪問 Container 開放的 IP:Port 會看到 WebUI，做一些基本設定，例如建立賬號、建立 `bucket`、建立 `token`。
取得 `token`、`org` 和 `bucket` 之後，拿個小本本記下來。

在 Python 中，可以直接使用官方的 Package 訪問資料庫。

```bash
pip install influxdb-client
```

參考：

- InfluxDB OSS v2 Documentation: [Install InfluxDB](https://docs.influxdata.com/influxdb/v2/install/)
- InfluxDB Cloud (TSM) Documentation: [Use the InfluxDB Python client library](https://docs.influxdata.com/influxdb/cloud/api-guide/client-libraries/python/)

## 寫一點程式

程式有兩個任務，按指定頻率的抓取感測值，然後放進資料庫。

我們可以直接修改上面提及的範例程式 `read-all.py`，畢竟是造好的輪子。
範例程式將抓取所有感測值，並將感測值印在標準輸出上。

### 擷取感測值

上文提到，BME680 還外加了感測揮發性有機化合物（VOC），VOC 感測器需要預熱。

在範例程式中，下面的代碼啟用 VOC 感測，刪掉即關閉：

```python
sensor.set_gas_status(bme680.ENABLE_GAS_MEAS)
```

若啟用了 VOC 感測，當感測器準備好的時候 `sensor.data.heat_stable == True` 即可得到 VOC 值 `sensor.data.gas_resistance`。

範例程式：

```Python
# BME680 初始化
sensor = bme680.BME680(bme680.I2C_ADDR_PRIMARY)

# 讀取 BME680 資料
if not sensor.get_sensor_data():
    continue
temperature: float = sensor.data.temperature
pressure: float = sensor.data.pressure
humidity: float = sensor.data.humidity
print(f"{temperature:.2f} °C, {pressure:.2f} hPa, {humidity:.2f} %RH")
```

### 寫入資料庫

範例程式：

```Python
# InfluxDB 客戶端初始化
client = InfluxDBClient(url=url, token=token, org=org)

# InfluxDB 寫入 API 初始化
write_api = client.write_api(write_options=SYNCHRONOUS)

# 構建 InfluxDB 數據點
data_point = Point("bme680") \
    .field("temperature", temperature) \
    .field("pressure", pressure) \
    .field("humidity", humidity)

# 寫入 InfluxDB
write_api.write(bucket=bucket, record=data_point)
```

此時已成功的把資料寫進 InfluxDB 中，在 WebUI 界面中可以看到資料的更新。
InfluxDB 內建資料視覺化，已經夠用了。
<del>但只用它不夠潮，</del>要潮到出水，我們會用到 Grafana。

## 建立儀錶板：Grafana

Grafana 是一個儀錶板，用於資料視覺的呈現。Gragana 一樣可以通過 Docker 安裝。（註：同樣的，範例中的地址 `127.0.0.1:3000` 為了安全僅限本機，若要開放，可以使用任意 Web 伺服器做反向代理，同時修改 docker compose 的環境變數 `GF_SERVER_ROOT_URL`，同時可以考慮啟用 HTTPS。）

```yaml
version: "3.8"
services:
  grafana:
    image: grafana/grafana-oss
    container_name: grafana
    restart: unless-stopped
    ports:
      - 127.0.0.1:3000:3000
    environment:
      - GF_SERVER_ROOT_URL=http://127.0.0.1:3000/
      - GF_DASHBOARDS_MIN_REFRESH_INTERVAL=1s
    network_mode: host
    volumes:
      - grafana-storage:/var/lib/grafana
volumes:
  grafana-storage: {}
```

Grafana 中內建 InfluxDB 的支援，在資料來源中可直接選擇 InfluxDB，按說明設定即可。

Grafana 預設的更新間隔較長，修改 docker compose 中的環境變數 `GF_DASHBOARDS_MIN_REFRESH_INTERVAL` 調整最小間隔（範例中為 1 秒）。 

至此，可以在 Grafana 的儀表板中「實時」看到 BME680 感測器的結果。

參考：

- SDpower @ iT 邦幫忙: 制霸IoT 30Day！系列 第 16 篇 [圖表（二）](https://ithelp.ithome.com.tw/articles/10223604)

## Appendix

### InfluxDB v. Prometheus

InfluxDB 是時序資料庫，時序資料 簡單來說就是 時間 對應 資料；
而 Prometheus 更專注於監視和告警。
簡單來說，他們關鍵的差異在於 Push（推）和 Pull（拉）模型。

- InfluxDB 使用 Push model，程式 將 資料 __推__ 到資料庫中。
- Prometheus 使用 Pull model，資料 由 Prometheus __拉__ 到資料庫中。

因為是由 Prometheus 主動向程式（Exporter） 抓取資料，Exporter 將資料準備成特定的格式 metric；
若 Exporter 要直接對接 Prometheus，Exporter 必須實作一個 Web Server：
即是 Prometheus 向 Exporter 發送 HTTP 請求，Exporter 使用 HTTP 回應。

所以，資料的流程是：

- 若使用 InfluxDB：資料源 -> 程式 -> InfluxDB <- Grafana；
- 若使用 Prometheus：資料源 -> Exporter <- Prometheus <- Grafana。
