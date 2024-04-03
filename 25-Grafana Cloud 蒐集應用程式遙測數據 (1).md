# Day25 - Grafana Cloud : 蒐集應用程式遙測數據 (1/2)

![https://ithelp.ithome.com.tw/upload/images/20231011/201625770eLySRC8g8.jpg](https://ithelp.ithome.com.tw/upload/images/20231011/201625770eLySRC8g8.jpg)

接下來透過程式直接看如何將資料送到 Grafana Cloud，在開始之前來看遙測數據要如何傳送，根據官方的建議架構如下
![https://ithelp.ithome.com.tw/upload/images/20231011/20162577pBFF2SIMJl.png](https://ithelp.ithome.com.tw/upload/images/20231011/20162577pBFF2SIMJl.png)

應用程式使用 OpenTelemetry SDKs 自動蒐集方式將 traces, metrics 和 logs 資訊透過 OTLP 協定送到 Grafana Agent，Agent 再將資料送到 Grafana Cloud。

Grafana Agent 是本地安裝的代理，具有遠端寫入的功能，可以將指標、日誌及追蹤數據發送到後端儲存，例如 Mimir、Loki 和 Tempo。以下用 .NET 作為範例。

## 建立 .NET 範例專案
我們透過範例程式來整合 OpenTelemetry，在 .NET 中有內建 Web API 範例程式可以做為 Demo 程式不用自己寫，在開始我們透過下列指令建立 .NET 應用程式
> dotnet new webapi

上述指令將在資料夾底下建立 .NET 範例應用程式，該範例程式會包含 Swagger UI 及 WeatherForecast WebAPI。

## 範例程式加上 OpenTelemetry SDKs 
需要透過設定 OpenTelemetry for .NET 的 SDKs 自動蒐集應用程式的遙測數據資料，在 .NET 是透過 nuget 套件管理中心進行下載動作，要使用 openTelemetry 需要安裝下列套件
```
<PackageReference Include="OpenTelemetry.Exporter.Console" Version="1.6.0" />
<PackageReference Include="OpenTelemetry.Exporter.OpenTelemetryProtocol" Version="1.6.0" />
<PackageReference Include="OpenTelemetry.Instrumentation.AspNetCore" Version="1.5.0-beta.1" />
<PackageReference Include="OpenTelemetry.Instrumentation.Runtime" Version="1.5.0" />
```

套件安裝完成後，需要在啟動程式 `program.cs` 加上下列程式碼設定 openTelemetry

```
using OpenTelemetry.Metrics;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;

builder.Services.AddOpenTelemetry()
    .ConfigureResource(res => res.AddService(
        serviceName: builder.Environment.ApplicationName,
        serviceVersion: Assembly.GetEntryAssembly()?.GetName().Version?.ToString(),
        serviceInstanceId: Environment.MachineName)
    .AddAttributes(new Dictionary<string, object>
    {
        { "deployment.environment", builder.Environment.EnvironmentName }
    }))
    .WithTracing(tracing => tracing.AddAspNetCoreInstrumentation()
    .AddConsoleExporter());
```
上述程式碼重點
* AddOpenTelemetry : 使用 openTelemetry sdks 並設定其 servcice Name 、環境與版本資訊
* AddAspNetCoreInstrumentation : 自動蒐集 .NET 應用程式 **Trace** 數據資訊，包含傳入的 http 請求
* ConsoleExporter : 將追蹤輸出到控制台上

執行專案 dotnet run 或是在 Visual Studio IDE 按下 F5，在瀏覽器輸入 `https://localhotst:5000/weather` ，即可看到畫面上出現發送請求的相關遙測數據

```
 Activity.TraceId:          5d8094a157d928778b5fbe5dc85126eb
 Activity.SpanId:           732fb5979e3905b3
 Activity.TraceFlags:           Recorded
 Activity.ActivitySourceName: OpenTelemetry.Instrumentation.AspNetCore
 Activity.DisplayName: /weather
 Activity.Kind:        Server
 Activity.StartTime:   2023-10-10T15:25:04.1451306Z
 Activity.Duration:    00:00:00.0871028
 Activity.Tags:
     net.host.name: localhost
     net.host.port: 5000
     http.method: GET
     http.scheme: http
     http.target: /weather
     http.url: http://localhost:5000/weather
     http.flavor: 1.1
     http.user_agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36
     http.status_code: 200
 Resource associated with Activity:
     service.name: dotnet6-example
     service.instance.id: 2363b8ac-1e0f-44aa-9531-bf6a402b0d34

 info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
       Request starting HTTP/1.1 GET http://localhost:5000/favicon.ico - -
 Activity.TraceId:          26b7f657aaaffd67b0ea144c1d516a7b
 Activity.SpanId:           c164340727e4b0e7
 Activity.TraceFlags:           Recorded
 Activity.ActivitySourceName: OpenTelemetry.Instrumentation.AspNetCore
 info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
       Request finished HTTP/1.1 GET http://localhost:5000/favicon.ico - - - 404 0 - 0.5362ms
```

## 透過 OpenTelemetry 蒐集 Metrics 數據
在啟動程式 `program.cs` 加上下列程式碼設定 `WithMetrics`

```
.WithMetrics(metrics => metrics.AddAspNetCoreInstrumentation()
        .AddRuntimeInstrumentation()
        .AddConsoleExporter());
```

看起來內容與 trace 很像，主要功能如下
* 從 dotnet core 應用程式收集 metrics 資訊
* 應用程式運行時蒐集 runtime 資訊
* 蒐集後透過 console 顯示 

透過 IDE 或是 Command 執行 `dotnet run`，即可看到 trace 與 metrics 數據。
