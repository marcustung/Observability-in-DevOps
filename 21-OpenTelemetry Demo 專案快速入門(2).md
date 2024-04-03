# Day21 - OpenTelemetry : Demo 專案快速入門 (2/2)

![https://ithelp.ithome.com.tw/upload/images/20231006/20162577GUrNoNL0rJ.jpg](https://ithelp.ithome.com.tw/upload/images/20231006/20162577GUrNoNL0rJ.jpg)
> OpenTememetry Demo 專案是如何做到的呢 ?

## Telemetry Data Flow : 遙測數據蒐集流程
![https://ithelp.ithome.com.tw/upload/images/20231006/20162577OLiotJkyj6.png](https://ithelp.ithome.com.tw/upload/images/20231006/20162577OLiotJkyj6.png)

上圖是 OTel 範例專案的數據流程圖，從圖中可以得知重要元件有 OTel Demo Application、OTel Collector、Prometheus、Jaeger 及 Grafana。以下就針對這些元件做簡單說明
 
### **OpenTelemetry Demo**

OTel Demo是由 10 多種不同的程式語言開發的微服務架構應用程式，在 Day18 的介紹中有提到蒐集遙測數據方式有**自動**跟**手動**兩種，以 .NET 開發的 Cart Service 為例，其是使用自動(automatic)採集，再將蒐集好的資料使用 HTTP、grpc Protocol 傳遞到 OTel collector。

在 .NET 專案 [Program.cs](https://github.com/open-telemetry/opentelemetry-demo/blob/main/src/cartservice/src/Program.cs) 可以在程式碼中可以看到使用 `AddOpenTelemetry()` 方法後加上 `AddAspNetCoreInstrumentation` 與 `AddRedisInstrumentation` 自動蒐集 dotnet 應用程式與 Redis 的遙測資料
```
Action<ResourceBuilder> appResourceBuilder =
    resource => resource
        .AddDetector(new ContainerResourceDetector());

builder.Services.AddOpenTelemetry()
    .ConfigureResource(appResourceBuilder)
    .WithTracing(tracerBuilder => tracerBuilder
        .AddRedisInstrumentation(
            cartStore.GetConnection(),
            options => options.SetVerboseDatabaseStatements = true)
        .AddAspNetCoreInstrumentation()
        .AddGrpcClientInstrumentation()
        .AddHttpClientInstrumentation()
        .AddOtlpExporter());

```

#### **OTel Collector**
- Rceivers : 
    - 透過兩個不同的端點接收數據進行監聽 
    - HTTP : http://localhost:4318 
    - gRPC : grpc://localhost:4317 
- Processors 
- Exporters 
    - OTel 使用兩種方式導出數據
    -  OTLP HTTP Exporter 將數據發送到 http://localhost:9090/api/v1/otlp
    -  OTLP Exporter grpc 數據發送到 Jaeger

設定檔 : otelcol-config.yml、[otelcol-observability.yml](https://github.com/open-telemetry/opentelemetry-demo/blob/main/src/otelcollector/otelcol-observability.yml)
```
# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0

exporters:
  otlp:
    endpoint: "jaeger:4317"
    tls:
      insecure: true
  otlp/logs:
    endpoint: "dataprepper:21892"
    tls:
      insecure: true
  otlphttp/prometheus:
    endpoint: "http://prometheus:9090/api/v1/otlp"
    tls:
      insecure: true

service:
  pipelines:
    traces:
      exporters: [otlp, logging, spanmetrics]
    metrics:
      exporters: [otlphttp/prometheus, logging]
    logs:
      exporters: [otlp/logs, logging]

```

OTel Collector 詳細配置設定介紹請參考 : [官網](https://github.com/open-telemetry/opentelemetry-collector/blob/main/exporter/README.md)

#### **Prometheus**
* 收集器使用OTLP協議將數據發送到 Prometheus。
* 存儲在其時間序列數據庫 (TSDB) 中。
* 可以通過瀏覽器訪問 http://localhost:9090 開啟 Prometheus UI 進行操作。

#### **Jaeger**
* Jaeger 收集器在 grpc://jaeger:4317 上監聽遙測數據。
* 數據存在 Jaeger DB 中。
* 可以通過瀏覽器訪問 http://localhost:16686 開啟 Jaeger UI 進行操作。

#### **Grafana**
* 資料來源 
    * Prometheus : http://localhost:9090/api
    * Jaeger : http://localhost:16686/api
* 可以通過瀏覽器訪問 http://localhost:3000/dashboard 開啟 Grafana UI 進行操作。


---


## Manual Instrumentation
> 當需要自定義 Span 屬性時，該如何設定呢 ? 

在 [Cart Service](https://github.com/open-telemetry/opentelemetry-demo/blob/main/src/cartservice/src/services/CartService.cs) 中有幾個自定義屬性
| Name | Type | Description | 
| -------- | -------- | -------- | 
| app.cart.items.count	     | number     | Number of unique items in cart | 
| app.product.id | string | Product ID for cart item | 
| app.product.quantity | string | Quantity for cart item | 

在 .NET 中我們可以使用 `Activity` 類別，設定其 tag 進行自定義屬性設定，將重要資訊透過設定在既有的請求上，將屬性一直往後傳遞下去

例如 : `cart.items.count`

```
var activity = Activity.Current;
activity?.SetTag("app.user.id", request.UserId);
activity?.AddEvent(new("Fetch cart"));

var cart = await _cartStore.GetCartAsync(request.UserId);
var totalCart = 0;
foreach (var item in cart.Items)
{
    totalCart += item.Quantity;
}
activity?.SetTag("app.cart.items.count", totalCart);
```

例如 : 清空購物車時，紀錄 UserId。當遇到異常的狀況時亦可將錯誤資訊記錄下來，如下所示

```
public override async Task<Empty> EmptyCart(EmptyCartRequest request, ServerCallContext context)
    {
        var activity = Activity.Current;
        activity?.SetTag("app.user.id", request.UserId);
        activity?.AddEvent(new("Empty cart"));

        try
        {
            if (await _featureFlagHelper.GenerateCartError())
            {
                await _badCartStore.EmptyCartAsync(request.UserId);
            }
            else
            {
                await _cartStore.EmptyCartAsync(request.UserId);
            }
        }
        catch (RpcException ex)
        {
            Activity.Current?.RecordException(ex);
            Activity.Current?.SetStatus(ActivityStatusCode.Error, ex.Message);
            throw;
        }

        return Empty;
    }
```

關於資料蒐集的重要觀念之前已介紹過，可以參考下列章節回顧
- [Day14 - 可觀測性管道：解析現代數據收集與分析](https://ithelp.ithome.com.tw/articles/10330480)
- [Day18 - OpenTelemetry : 核心元件大解密 (1/2) ](https://ithelp.ithome.com.tw/articles/10333458)
- [Day19 - OpenTelemetry : 核心元件大解密 (2/2)](https://ithelp.ithome.com.tw/articles/10333461)

最後關於 Demo 範例專案內容可以參考官網 [OpenTelemetry Document 文件](https://opentelemetry.io/docs/demo/)，內有更豐富的資源提供參考，詳細說明就整理到這邊不在追述。


## 參考連結
[OpenTelemetry in Action](https://medium.com/@magstherdev/opentelemetry-in-action-fc61263c852)

[OpenTelemetry Demo Documentation](https://opentelemetry.io/docs/demo/)











