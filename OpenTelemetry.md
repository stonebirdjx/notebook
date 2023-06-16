<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [Tracer](#tracer)
- [Metrics](#metrics)
- [Exporters](#exporters)
  - [OTLP Exporter](#otlp-exporter)
  - [Jaeger Exporter](#jaeger-exporter)
  - [Prometheus Exporter](#prometheus-exporter)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

OpenTelemetry，也简称为 OTel，是一个供应商中立的开源可观察性框架，用于检测、生成、收集和导出遥测数据，例如traces（追踪）、metrics（指标）、logs（日志）。 作为一项行业标准，它受到许多供应商的原生支持。

**OpenTracing** 

提供了供应商中立的 API，用于将遥测数据发送到可观察性后端； 但是，它依赖于开发人员实现自己的库来满足规范。

**OpenCensus** 

提供了一组特定于语言的库，开发人员可以使用这些库来检测他们的代码并发送到他们支持的任何一个后端。

> 过去没有用于将数据发送到可观察性后端的标准化数据格式,为了拥有一个单一的标准，OpenCensus 和 OpenTracing 于 2019 年 5 月合并形成了 OpenTelemetry（简称 OTel）。作为 CNCF 孵化项目，OpenTelemetry 充分利用了两个世界的优点，然后是一些。



# Tracer

- 创建一个tracer
- 创建一个span 
  - 设置span 属性
  - 设置span 的events事件
  - 设置span状态
  - 记录错误

```go
package main

import (
	"context"
	"fmt"
	"log"

	"go.opentelemetry.io/otel"
	"go.opentelemetry.io/otel/attribute"
	"go.opentelemetry.io/otel/exporters/stdout"
	sdktrace "go.opentelemetry.io/otel/sdk/trace"
)

func main() {
	// Create and register the stdout exporter
	exporter, err := stdout.NewExporter(stdout.WithPrettyPrint())
	if err != nil {
		log.Fatalf("failed to initialize stdout export pipeline: %v", err)
	}
	tracerProvider := sdktrace.NewTracerProvider(sdktrace.WithBatcher(exporter))

	otel.SetTracerProvider(tracerProvider)

	tracer := otel.Tracer("example")

	// Start a span
	ctx, span := tracer.Start(context.Background(), "foo",
		otel.WithAttributes(attribute.String("key", "value")),
	)
	defer span.End()

	fmt.Println("Hello, OpenTelemetry!")
}
```

# Metrics

```go
package main

import (
	"context"
	"fmt"
	"log"
	"time"

	"go.opentelemetry.io/otel"
	"go.opentelemetry.io/otel/exporters/stdout"
	"go.opentelemetry.io/otel/metric"
	"go.opentelemetry.io/otel/metric/global"
	"go.opentelemetry.io/otel/trace"
	sdktrace "go.opentelemetry.io/otel/sdk/trace"
)

func initTracer() {

	exporter, err := stdout.NewExporter(stdout.WithPrettyPrint())
	if err != nil {
		log.Fatal(err)
	}

	// Create a new SDK tracer provider with the exporter
	// and a trace sampling strategy.
	tp := sdktrace.NewTracerProvider(
		sdktrace.WithBatcher(exporter),
		sdktrace.WithSampler(sdktrace.AlwaysSample()),
	)

	// Register the tracer provider as the global tracer provider.
	otel.SetTracerProvider(tp)
}

func initMeter() {
	exporter, err := stdout.NewExporter(stdout.WithPrettyPrint())
	if err != nil {
		log.Fatal(err)
	}

	pusher := metric.Must(*global.MeterProvider().Meter("example")).(*sdktrace.Exporter)
	pusher.RegisterExporter(exporter)
	global.SetMeterProvider(pusher.MeterProvider())
}

func main() {
	initTracer()
	initMeter()
	meter := global.Meter("example")
	counter := metric.Must(meter).NewInt64Counter("example_counter")
	histogram := metric.Must(meter).NewInt64Histogram(
		"example_histogram",
		metric.WithDescription("Example histogram"),
		metric.WithUnit("By"),
	)
	ctx := context.Background()
	for i := 0; i < 10; i++ {
		counter.Add(ctx, 1, metric.WithLabels(
			"label1", "value1",
			"label2", "value2",
		))

		histogram.Record(ctx, int64(i), metric.WithLabels(
			"label1", "value1",
			"label2", "value2",
		))

		time.Sleep(time.Second)
	}
	<-time.After(2 * time.Second)
}

```

# Exporters

## OTLP Exporter

```go
package main

import (
	"context"
	"log"
	"time"

	"go.opentelemetry.io/otel"
	"go.opentelemetry.io/otel/exporters/otlp"
	"go.opentelemetry.io/otel/metric"
	"go.opentelemetry.io/otel/metric/global"
	"go.opentelemetry.io/otel/metric/unit"
	"go.opentelemetry.io/otel/sdk/metric/controller/push"
	"go.opentelemetry.io/otel/sdk/resource"
	sdktrace "go.opentelemetry.io/otel/sdk/trace"
	"go.opentelemetry.io/otel/trace"
)

func main() {
	// Create the OTLP exporter
	exporter, err := otlp.NewExporter(
		otlp.WithInsecure(),
		otlp.WithAddress("localhost:4317"),
		otlp.WithGRPCDialOption(),
	)
	if err != nil {
		log.Fatalf("Failed to create exporter: %v", err)
	}
	defer exporter.Shutdown(context.Background())

	// Create the trace provider with the exporter
	// You can also add a batch span processor here if needed
	traceProvider := sdktrace.NewTracerProvider(
		sdktrace.WithBatcher(exporter),
		sdktrace.WithResource(resource.NewSchemaless(map[string]string{
			"service.name": "my-service",
		})),
	)
	defer traceProvider.Shutdown(context.Background())

	// Set the trace provider
	otel.SetTracerProvider(traceProvider)

	// Create a new tracer
	tracer := otel.Tracer("example-tracer")

	// Create a span
	ctx, span := tracer.Start(context.Background(), "example-span")
	defer span.End()

	// Create a new meter provider
	pusher := push.New(
		exporter,
		push.WithPeriod(10*time.Second),
	)
	meterProvider := pusher.Provider()

	// Register the meter provider globally
	global.SetMeterProvider(meterProvider)

	// Create a new meter
	meter := global.Meter("example-meter")

	// Create a counter instrument
	counter := metric.Must(meter).NewInt64Counter(
		"example-counter",
		metric.WithDescription("Counts the number of example events"),
	)

	// Record a counter measurement
	labels := []label.KeyValue{
		label.String("example-label", "example-value"),
	}
	counter.Add(ctx, 1, labels...)

	// Sleep for a bit to allow data to be exported
	time.Sleep(15 * time.Second)
}
```

## Jaeger Exporter

```go
package main

import (
    "context"
    "go.opentelemetry.io/otel"
    "go.opentelemetry.io/otel/exporters/jaeger"
    "go.opentelemetry.io/otel/sdk/trace"
    "go.opentelemetry.io/otel/trace"
)

func main() {
    // create a Jaeger exporter
    exp, err := jaeger.NewRawExporter(jaeger.WithCollectorEndpoint(jaeger.WithEndpoint("http://localhost:14268/api/traces")))
    if err != nil {
        panic(err)
    }
    defer exp.Shutdown(context.Background())

    // set global tracer provider
    tp := trace.NewTracerProvider(
        trace.WithBatcher(exp),
    )
    otel.SetTracerProvider(tp)

    // create a tracer
    tracer := otel.Tracer("example-tracer")

    // start a span
    ctx, span := tracer.Start(context.Background(), "example-span")
    defer span.End()

    // do some work
    for i := 0; i < 10; i++ {
        // create a child span
        childCtx, childSpan := tracer.Start(ctx, "child-span")
        defer childSpan.End()

        // do some work
        // ...

        // add an attribute to the child span
        childSpan.SetAttributes(
            trace.StringAttribute("key", "value"),
        )
    }
}
```

## Prometheus Exporter

```go
package main

import (
	"context"
	"log"
	"net/http"
	"time"

	"github.com/prometheus/client_golang/prometheus"
	"github.com/prometheus/client_golang/prometheus/promhttp"
	"go.opentelemetry.io/contrib/exporters/metric/prometheus/standard"
	"go.opentelemetry.io/otel/api/global"
	"go.opentelemetry.io/otel/api/metric"
	"go.opentelemetry.io/otel/api/metric/unit"
	controller "go.opentelemetry.io/otel/sdk/metric/controller/basic"
	exporter "go.opentelemetry.io/otel/sdk/metric/export"
	"go.opentelemetry.io/otel/sdk/metric/processor/basic"
	"go.opentelemetry.io/otel/sdk/resource"
)

func main() {
	// 设置全局的 Meter Provider
	// 如果要使用其他的 Meter Provider 实现，可以参考 SDK
	// https://github.com/open-telemetry/opentelemetry-go/tree/main/sdk/metric
	pusher, err := installPrometheusExporter()
	if err != nil {
		log.Fatalf("failed to install Prometheus exporter: %v", err)
	}
	defer pusher.Stop()

	// 创建一个 Meter 实例
	meter := global.Meter("example")

	// 定义一个 Counter 指标
	counter := metric.Must(meter).NewInt64Counter(
		"http_requests_total",
		metric.WithDescription("Total number of HTTP requests."),
	)

	// 创建一个资源实例
	res := resource.NewWithAttributes(
		attribute.String("service.name", "my-service"),
		attribute.String("telemetry.version", "v1"),
	)

	// 每隔 5 秒钟，发送指标数据到 Exporter
	for range time.Tick(time.Second * 5) {
		// 更新指标数据
		counter.Add(context.Background(), 1, label.String("path", "/foo"))

		// 获取当前的 Batch Processor
		batcher, err := pusher.Processor("example")
		if err != nil {
			log.Printf("failed to get batcher: %v", err)
			continue
		}

		// Sync 同步等待 Processor 处理完所有的数据
		_ = batcher.SynchronizedMove(export.CumulativeExportKindSelector())
	}
}

func installPrometheusExporter() (*controller.Controller, error) {
	// 创建 Prometheus Exporter
	exp, err := standard.NewExporter(
		standard.Config{
			Registerer: prometheus.DefaultRegisterer,
			Gatherer:   prometheus.DefaultGatherer,
		},
	)
	if err != nil {
		return nil, err
	}

	// 创建一个批量处理器
	batcher := basic.New(processor.NewFactory(), exp)

	// 创建一个控制器
	ctrl := controller.New(
		batcher,
		controller.WithPusher(exp),
		controller.WithCollectPeriod(2*time.Second),
	)

	// 启动控制器
	ctrl.Start(context.Background())

	// 注册 HTTP Handler，提供 /metrics 接口，用于获取 Prometheus 指标数据
	http.Handle("/metrics", promhttp.Handler())
	go func() {
		log.Fatal(http.ListenAndServe(":8888", nil))
	}()

	return ctrl, nil
}
```

