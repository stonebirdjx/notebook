# 主要优势

由指标名称和和键/值对标签标识的时间序列数据组成的多维数据模型。

强大的查询语言 PromQL。

不依赖分布式存储；单个服务节点具有自治能力。

时间序列数据是服务端通过 HTTP 协议主动拉取获得的。

也可以通过中间网关来推送时间序列数据。

可以通过静态配置文件或服务发现来获取监控目标。

支持多种类型的图表和仪表盘。



# 样本

样本由以下三部分组成：

- 指标（metric）：指标名称和描述当前样本特征的 labelsets；
- 时间戳（timestamp）：一个精确到毫秒的时间戳；
- 样本值（value）： 一个 folat64 的浮点型数据表示当前样本的值。

api_http_requests_total{method="POST", handler="/messages"}

# 编写一个简单的 Exporter

```go
package main

import (
"log"
"net/http"

"github.com/prometheus/client_golang/prometheus"
"github.com/prometheus/client_golang/prometheus/promhttp"
)

var (
//使用GaugeVec类型可以为监控指标设置标签，这里为监控指标增加一个标签"device"
speed = prometheus.NewGaugeVec(prometheus.GaugeOpts{
Name: "disk_available_bytes",
Help: "Disk space available in bytes",
}, []string{"device"})

tasksTotal = prometheus.NewCounter(prometheus.CounterOpts{
Name: "test_tasks_total",
Help: "Total number of test tasks",
})

taskDuration = prometheus.NewSummary(prometheus.SummaryOpts{
Name: "task_duration_seconds",
Help: "Duration of task in seconds",
//Summary类型的监控指标需要提供分位点
Objectives: map[float64]float64{0.5: 0.05, 0.9: 0.01, 0.99: 0.001},
})

cpuTemperature = prometheus.NewHistogram(prometheus.HistogramOpts{
Name: "cpu_temperature",
Help: "The temperature of cpu",
//Histogram类型的监控指标需要提供Bucket
Buckets: []float64{20, 50, 70, 80},
})
)

func init() {
//注册监控指标
prometheus.MustRegister(speed)
prometheus.MustRegister(tasksTotal)
prometheus.MustRegister(taskDuration)
prometheus.MustRegister(cpuTemperature)
}

func main() {
//模拟采集监控数据
fakeData()

//使用prometheus提供的promhttp.Handler()暴露监控样本数据
//prometheus默认从"/metrics"接口拉取监控样本数据
http.Handle("/metrics", promhttp.Handler())
log.Fatal(http.ListenAndServe(":10000", nil))
}

func fakeData() {
tasksTotal.Inc()
//设置该条样本数据的"device"标签值为"/dev/sda"
speed.With(prometheus.Labels{"device": "/dev/sda"}).Set(82115880)

taskDuration.Observe(10)
taskDuration.Observe(20)
taskDuration.Observe(30)
taskDuration.Observe(45)
taskDuration.Observe(56)
taskDuration.Observe(80)

cpuTemperature.Observe(30)
cpuTemperature.Observe(43)
cpuTemperature.Observe(56)
cpuTemperature.Observe(58)
cpuTemperature.Observe(65)
cpuTemperature.Observe(70)
}
```

