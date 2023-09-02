<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [概念](#%E6%A6%82%E5%BF%B5)
- [安装Istio](#%E5%AE%89%E8%A3%85istio)
- [卸载Istio](#%E5%8D%B8%E8%BD%BDistio)
- [流量管理](#%E6%B5%81%E9%87%8F%E7%AE%A1%E7%90%86)
  - [配置请求路由](#%E9%85%8D%E7%BD%AE%E8%AF%B7%E6%B1%82%E8%B7%AF%E7%94%B1)
  - [故障注入](#%E6%95%85%E9%9A%9C%E6%B3%A8%E5%85%A5)
  - [流量转移](#%E6%B5%81%E9%87%8F%E8%BD%AC%E7%A7%BB)
  - [TCP 流量转移](#tcp-%E6%B5%81%E9%87%8F%E8%BD%AC%E7%A7%BB)
  - [设置请求超时](#%E8%AE%BE%E7%BD%AE%E8%AF%B7%E6%B1%82%E8%B6%85%E6%97%B6)
  - [熔断](#%E7%86%94%E6%96%AD)
  - [镜像](#%E9%95%9C%E5%83%8F)
  - [地域负载均衡 - *istio-proxy*](#%E5%9C%B0%E5%9F%9F%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1---istio-proxy)
  - [Ingress](#ingress)
  - [Egress](#egress)
- [安全](#%E5%AE%89%E5%85%A8)
  - [认证](#%E8%AE%A4%E8%AF%81)
  - [证书管理](#%E8%AF%81%E4%B9%A6%E7%AE%A1%E7%90%86)
  - [授权](#%E6%8E%88%E6%9D%83)
  - [TLS 配置](#tls-%E9%85%8D%E7%BD%AE)
- [策略执行](#%E7%AD%96%E7%95%A5%E6%89%A7%E8%A1%8C)
  - [使用 Envoy 启用速率限制](#%E4%BD%BF%E7%94%A8-envoy-%E5%90%AF%E7%94%A8%E9%80%9F%E7%8E%87%E9%99%90%E5%88%B6)
- [可观察性](#%E5%8F%AF%E8%A7%82%E5%AF%9F%E6%80%A7)
- [可扩展性](#%E5%8F%AF%E6%89%A9%E5%B1%95%E6%80%A7)
- [bookinfo](#bookinfo)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 概念

- 熔断器：超过一定量请求后，后面业务不在提供服务
- 超时
- 重试
-  A/B 测试：A/B测试是一种用于评估两个或多个版本的产品、网站、应用程序或营销策略的比较方法，旨在确定哪个版本在用户体验、转化率、收入或其他关键指标方面效果更好。
- 金丝雀发布（灰度发布）：最初只对一小部分用户或服务器进行发布，然后逐步扩大规模，以确保新版本没有严重问题。
- 基于流量百分比切分：

[istio官方文档](https://istio.io/latest/zh/docs/concepts/traffic-management/)

# 安装Istio

> 基于k8s环境
>
> 建议使用[Helm安装](https://istio.io/latest/zh/docs/setup/install/helm/)

**下载**

```bash
curl -L https://istio.io/downloadIstio | sh -
```

**配置环境变量**

```bash
cd istio-1.18.2
# samples/ 目录下的示例应用程序
# bin/ 目录下的 istioctl 客户端二进制文件。
mv bin/istioctl /usr/local/bin
```

**安装**

```bash

istioctl install --set profile=demo -y
# ✔ Istio core installed
# ✔ Istiod installed
# ✔ Egress gateways installed
# ✔ Ingress gateways installed
# ✔ Installation complete
```

- default：根据 IstioOperator API 的默认设置启动组件。 建议用于生产部署和 Multicluster Mesh 中的 Primary Cluster。
  您可以运行 istioctl profile dump 命令来查看默认设置。

- demo：这一配置具有适度的资源需求，旨在展示 Istio 的功能。 它适合运行 Bookinfo 应用程序和相关任务。 这是通过快速开始指导安装的配置。
- minimal：与默认配置文件相同，但只安装了控制平面组件。 它允许您使用 Separate Profile 配置控制平面和数据平面组件(例如 Gateway)。
- remote：配置 Multicluster Mesh 的 Remote Cluster。
- empty：不部署任何东西。可以作为自定义配置的基本配置文件。
- preview：预览文件包含的功能都是实验性。这是为了探索 Istio 的新功能。不确保稳定性、安全性和性能（使用风险需自负）。

**添加命名空间**

给命名空间添加标签，指示 Istio 在部署应用的时候，自动注入 Envoy 边车代理：

```BAS
kubectl label namespace default istio-injection=enabled
```

**安装组件**

```yaml
kubectl apply -f samples/addons
```

 **访问Kiali 仪表板**

```bash
istioctl dashboard kiali
```

# 卸载Istio

```bash
kubectl delete -f samples/addons
istioctl uninstall -y --purge
kubectl delete namespace istio-system
kubectl label namespace default istio-injection-
```

# 流量管理

## 配置请求路由
如何将请求动态路由到微服务的多个版本。 ***VirtualService***

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: productpage
spec:
  hosts:
  - productpage
  http:
  - route:
    - destination:
        host: productpage
        subset: v1
```

根据登录用户 来自名为 Jason 的用户的所有流量将被路由到服务 `reviews:v2`。

请注意，Istio 对用户身份没有任何特殊的内置机制。事实上，`productpage` 服务在所有到 `reviews` 服务的 HTTP 请求中都增加了一个自定义的 `end-user` 请求头，从而达到了本例子的效果。

```yaml
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
...
spec:
  hosts:
  - reviews
  http:
  - match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v1
```

## 故障注入
此任务说明如何注入故障并测试应用程序的弹性。

**延迟故障**

```yaml
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
spec:
  hosts:
  - ratings
  http:
  - fault:
      delay:
        fixedDelay: 7s
        percentage:
          value: 100
    match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: ratings
        subset: v1
  - route:
    - destination:
        host: ratings
        subset: v1
```

**abort 故障**

```yaml
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
spec:
  hosts:
  - ratings
  http:
  - fault:
      abort:
        httpStatus: 500
        percentage:
          value: 100
    match:
    - headers:
        end-user:
          exact: jason
    route:
    - destination:
        host: ratings
        subset: v1
  - route:
    - destination:
        host: ratings
        subset: v1
```



## 流量转移
展示如何将流量从旧版本迁移到新版本的服务。

```yaml
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
...
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
      weight: 50
    - destination:
        host: reviews
        subset: v3
      weight: 50
```

## TCP 流量转移
展示如何将一个服务的 TCP 流量从旧版本迁移到新版本。

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: tcp-echo
spec:
  hosts:
  - "*"
  gateways:
  - tcp-echo-gateway
  tcp:
  - match:
    - port: 31400
    route:
    - destination:
        host: tcp-echo
        port:
          number: 9000
        subset: v1
      weight: 80
    - destination:
        host: tcp-echo
        port:
          number: 9000
        subset: v2
      weight: 20
```

## 设置请求超时
本任务用于示范如何使用 Istio 在 Envoy 中设置请求超时。

```bash
$ kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v2
    timeout: 0.5s
EOF
```

## 熔断
本任务展示如何为连接、请求以及异常检测配置熔断。

```bash
$ kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: httpbin
spec:
  host: httpbin
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 1
      http:
        http1MaxPendingRequests: 1
        maxRequestsPerConnection: 1
    outlierDetection:
      consecutive5xxErrors: 1
      interval: 1s
      baseEjectionTime: 3m
      maxEjectionPercent: 100
EOF
```

> 您定义了 `maxConnections: 1` 和 `http1MaxPendingRequests: 1`。这些规则意味着，如果并发的连接和请求数超过一个，在 `istio-proxy` 进行进一步的请求和连接时，后续请求或连接将被阻止。
>
> `istio-proxy` 允许存在一些误差。

## 镜像
此任务演示了 Istio 的流量镜像/影子功能。

> 将v1的流量转义到v2

```bash
$ kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: httpbin
spec:
  hosts:
    - httpbin
  http:
  - route:
    - destination:
        host: httpbin
        subset: v1
      weight: 100
    mirror:
      host: httpbin
      subset: v2
    mirrorPercentage:
      value: 100.0
EOF
```

## 地域负载均衡 - *istio-proxy* 

[配置多集群访问](https://kubernetes.io/zh-cn/docs/tasks/access-application-cluster/configure-access-multiple-clusters/)

```bash
 # kubectl --context 指定集群
 kubectl --context="$CTX" apply -f sample.yaml;
```

本系列任务演示如何在 Istio 中配置地域负载均衡。

```yaml
$ kubectl --context="${CTX_PRIMARY}" apply -n sample -f - <<EOF
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: helloworld
spec:
  host: helloworld.sample.svc.cluster.local
  trafficPolicy:
    connectionPool:
      http:
        maxRequestsPerConnection: 1
    loadBalancer:
      simple: ROUND_ROBIN
      localityLbSetting:
        enabled: true
        failover:
          - from: region1
            to: region2
    outlierDetection:
      consecutive5xxErrors: 1
      interval: 1s
      baseEjectionTime: 1m
EOF
```

故障转移到 region1.zone2

```bash
kubectl --context="${CTX_R1_Z1}" exec \
  "$(kubectl get pod --context="${CTX_R1_Z1}" -n sample -l app=helloworld \
  -l version=region1.zone1 -o jsonpath='{.items[0].metadata.name}')" \
  -n sample -c istio-proxy -- curl -sSL -X POST 127.0.0.1:15000/drain_listeners
  
$ kubectl exec --context="${CTX_R1_Z1}" -n sample -c sleep \
  "$(kubectl get pod --context="${CTX_R1_Z1}" -n sample -l \
  app=sleep -o jsonpath='{.items[0].metadata.name}')" \
  -- curl -sSL helloworld.sample:5000/hello
Hello version: region1.zone2, instance: helloworld-region1.zone2-86f77cd7b-cpxhv
```

地域权重的分布

```yaml
$ kubectl --context="${CTX_PRIMARY}" apply -n sample -f - <<EOF
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: helloworld
spec:
  host: helloworld.sample.svc.cluster.local
  trafficPolicy:
    loadBalancer:
      localityLbSetting:
        enabled: true
        distribute:
        - from: region1/zone1/*
          to:
            "region1/zone1/*": 70
            "region1/zone2/*": 20
            "region3/zone4/*": 10
    outlierDetection:
      consecutive5xxErrors: 100
      interval: 1s
      baseEjectionTime: 1m
EOF
```

## Ingress

[详看](https://istio.io/latest/zh/docs/tasks/traffic-management/ingress/)

控制 Istio 服务网格的入口流量。Ingress `Gateway` 描述在网格边界运作的负载均衡器，用于接收传入的 HTTP/TCP 连接。 它会配置暴露的端口、协议等，但与 [Kubernetes Ingress 资源](https://kubernetes.io/zh-cn/docs/concepts/services-networking/ingress/)不同，

```yaml
$ kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: httpbin-gateway
spec:
  selector:
    istio: ingressgateway # use Istio default gateway implementation
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "httpbin.example.com"
EOF

---
$ kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: httpbin
spec:
  hosts:
  - "httpbin.example.com"
  gateways:
  - httpbin-gateway
  http:
  - match:
    - uri:
        prefix: /status
    - uri:
        prefix: /delay
    route:
    - destination:
        port:
          number: 8000
        host: httpbin
EOF
```

**安全网关**

```yaml
$ cat <<EOF | kubectl apply -f -
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: mygateway
spec:
  selector:
    istio: ingressgateway # use istio default ingress gateway
  servers:

  - port:
      number: 443
      name: https-httpbin
      protocol: HTTPS
    tls:
      mode: SIMPLE
      credentialName: httpbin-credential
    hosts:
    - httpbin.example.com
  - port:
      number: 443
      name: https-helloworld
      protocol: HTTPS
    tls:
      mode: SIMPLE
      credentialName: helloworld-credential
    hosts:
    - helloworld.example.com
EOF
```

##  Egress

https://istio.io/latest/zh/docs/tasks/traffic-management/egress/egress-control/

控制 Istio 服务网格的出口流量。访问外部服务

```bash
$ kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: httpbin-ext
spec:
  hosts:
  - httpbin.org
  ports:
  - number: 80
    name: http
    protocol: HTTP
  resolution: DNS
  location: MESH_EXTERNAL
EOF
```

# 安全

## 认证
管控网格服务间的双向 TLS 和终端用户的身份认证。

## 证书管理
管理 Istio 的证书。

## 授权
展示如何控制到 Istio 服务的访问。

## TLS 配置
在 Istio 中配置 TLS。

# 策略执行

演示策略执行特性。

##  [使用 Envoy 启用速率限制](https://istio.io/latest/zh/docs/tasks/policy-enforcement/rate-limit/)

Envoy 支持两种速率限制：全局和本地。全局速率使用全局 gRPC 速率限制服务为整个网格提供速率限制。 本地速率限制用于限制每个服务实例的请求速率。局部速率限制可以与全局速率限制一起使用，以减少负载全局速率限制服务。

```bash
# Copyright Istio Authors
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

##################################################################################################
# Redis service and deployment
# Ratelimit service and deployment

# Note: a configmap is needed to make the rate limit deployment work properly, for example:
#
#  apiVersion: v1
#  kind: ConfigMap
#  metadata:
#    name: ratelimit-config
#  data:
#    config.yaml: |
#      domain: echo-ratelimit
#      descriptors:
#        - key: PATH
#          value: "/"
#          rate_limit:
#            unit: minute
#            requests_per_unit: 1
#        - key: PATH
#          rate_limit:
#            unit: minute
#            requests_per_unit: 100
##################################################################################################
apiVersion: v1
kind: Service
metadata:
  name: redis
  labels:
    app: redis
spec:
  ports:
  - name: redis
    port: 6379
  selector:
    app: redis
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - image: redis:alpine
        imagePullPolicy: Always
        name: redis
        ports:
        - name: redis
          containerPort: 6379
      restartPolicy: Always
      serviceAccountName: ""
---
apiVersion: v1
kind: Service
metadata:
  name: ratelimit
  labels:
    app: ratelimit
spec:
  ports:
  - name: http-port
    port: 8080
    targetPort: 8080
    protocol: TCP
  - name: grpc-port
    port: 8081
    targetPort: 8081
    protocol: TCP
  - name: http-debug
    port: 6070
    targetPort: 6070
    protocol: TCP
  selector:
    app: ratelimit
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ratelimit
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ratelimit
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: ratelimit
    spec:
      containers:
      - image: envoyproxy/ratelimit:9d8d70a8 # 2022/08/16
        imagePullPolicy: Always
        name: ratelimit
        command: ["/bin/ratelimit"]
        env:
        - name: LOG_LEVEL
          value: debug
        - name: REDIS_SOCKET_TYPE
          value: tcp
        - name: REDIS_URL
          value: redis:6379
        - name: USE_STATSD
          value: "false"
        - name: RUNTIME_ROOT
          value: /data
        - name: RUNTIME_SUBDIRECTORY
          value: ratelimit
        - name: RUNTIME_WATCH_ROOT
          value: "false"
        - name: RUNTIME_IGNOREDOTFILES
          value: "true"
        - name: HOST
          value: "::"
        - name: GRPC_HOST
          value: "::"
        ports:
        - containerPort: 8080
        - containerPort: 8081
        - containerPort: 6070
        volumeMounts:
        - name: config-volume
          mountPath: /data/ratelimit/config
      volumes:
      - name: config-volume
        configMap:
          name: ratelimit-config
```

本地速率

```yaml
$ kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
metadata:
  name: filter-local-ratelimit-svc
  namespace: istio-system
spec:
  workloadSelector:
    labels:
      app: productpage
  configPatches:
    - applyTo: HTTP_FILTER
      match:
        context: SIDECAR_INBOUND
        listener:
          filterChain:
            filter:
              name: "envoy.filters.network.http_connection_manager"
      patch:
        operation: INSERT_BEFORE
        value:
          name: envoy.filters.http.local_ratelimit
          typed_config:
            "@type": type.googleapis.com/udpa.type.v1.TypedStruct
            type_url: type.googleapis.com/envoy.extensions.filters.http.local_ratelimit.v3.LocalRateLimit
            value:
              stat_prefix: http_local_rate_limiter
              token_bucket:
                max_tokens: 10
                tokens_per_fill: 10
                fill_interval: 60s
              filter_enabled:
                runtime_key: local_rate_limit_enabled
                default_value:
                  numerator: 100
                  denominator: HUNDRED
              filter_enforced:
                runtime_key: local_rate_limit_enforced
                default_value:
                  numerator: 100
                  denominator: HUNDRED
              response_headers_to_add:
                - append: false
                  header:
                    key: x-local-rate-limit
                    value: 'true'
EOF
```

此任务将展示如何配置 Istio 来动态地限制服务的流量。

# 可观察性

演示如何从网格收集遥测信息。

[指标](https://istio.io/latest/zh/docs/tasks/observability/metrics/)

演示 Istio 中指标的收集和查询。

[日志](https://istio.io/latest/zh/docs/tasks/observability/logs/)

演示 Istio 网格日志的配置、收集和处理。

[分布式追踪](https://istio.io/latest/zh/docs/tasks/observability/distributed-tracing/)

该任务展示了如何为启用了 Istio 支持的应用进行追踪。

[网格可视化](https://istio.io/latest/zh/docs/tasks/observability/kiali/)

此任务向您展示如何在 Istio 网格中可视化服务。

[远程访问遥测插件](https://istio.io/latest/zh/docs/tasks/observability/gateways/)

此任务向您展示如何配置从外部访问 Istio 遥测插件。

[Telemetry API](https://istio.io/latest/zh/docs/tasks/observability/telemetry/)

本任务向您演示如何配置 Telemetry API。

# 可扩展性



# bookinfo

部署

```yaml
# Copyright Istio Authors
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

##################################################################################################
# This file defines the services, service accounts, and deployments for the Bookinfo sample.
#
# To apply all 4 Bookinfo services, their corresponding service accounts, and deployments:
#
#   kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
#
# Alternatively, you can deploy any resource separately:
#
#   kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml -l service=reviews # reviews Service
#   kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml -l account=reviews # reviews ServiceAccount
#   kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml -l app=reviews,version=v3 # reviews-v3 Deployment
##################################################################################################

##################################################################################################
# Details service
##################################################################################################
apiVersion: v1
kind: Service
metadata:
  name: details
  labels:
    app: details
    service: details
spec:
  ports:
  - port: 9080
    name: http
  selector:
    app: details
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: bookinfo-details
  labels:
    account: details
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: details-v1
  labels:
    app: details
    version: v1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: details
      version: v1
  template:
    metadata:
      labels:
        app: details
        version: v1
    spec:
      serviceAccountName: bookinfo-details
      containers:
      - name: details
        image: docker.io/istio/examples-bookinfo-details-v1:1.17.0
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 9080
        securityContext:
          runAsUser: 1000
---
##################################################################################################
# Ratings service
##################################################################################################
apiVersion: v1
kind: Service
metadata:
  name: ratings
  labels:
    app: ratings
    service: ratings
spec:
  ports:
  - port: 9080
    name: http
  selector:
    app: ratings
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: bookinfo-ratings
  labels:
    account: ratings
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ratings-v1
  labels:
    app: ratings
    version: v1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ratings
      version: v1
  template:
    metadata:
      labels:
        app: ratings
        version: v1
    spec:
      serviceAccountName: bookinfo-ratings
      containers:
      - name: ratings
        image: docker.io/istio/examples-bookinfo-ratings-v1:1.17.0
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 9080
        securityContext:
          runAsUser: 1000
---
##################################################################################################
# Reviews service
##################################################################################################
apiVersion: v1
kind: Service
metadata:
  name: reviews
  labels:
    app: reviews
    service: reviews
spec:
  ports:
  - port: 9080
    name: http
  selector:
    app: reviews
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: bookinfo-reviews
  labels:
    account: reviews
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reviews-v1
  labels:
    app: reviews
    version: v1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: reviews
      version: v1
  template:
    metadata:
      labels:
        app: reviews
        version: v1
    spec:
      serviceAccountName: bookinfo-reviews
      containers:
      - name: reviews
        image: docker.io/istio/examples-bookinfo-reviews-v1:1.17.0
        imagePullPolicy: IfNotPresent
        env:
        - name: LOG_DIR
          value: "/tmp/logs"
        ports:
        - containerPort: 9080
        volumeMounts:
        - name: tmp
          mountPath: /tmp
        - name: wlp-output
          mountPath: /opt/ibm/wlp/output
        securityContext:
          runAsUser: 1000
      volumes:
      - name: wlp-output
        emptyDir: {}
      - name: tmp
        emptyDir: {}
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reviews-v2
  labels:
    app: reviews
    version: v2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: reviews
      version: v2
  template:
    metadata:
      labels:
        app: reviews
        version: v2
    spec:
      serviceAccountName: bookinfo-reviews
      containers:
      - name: reviews
        image: docker.io/istio/examples-bookinfo-reviews-v2:1.17.0
        imagePullPolicy: IfNotPresent
        env:
        - name: LOG_DIR
          value: "/tmp/logs"
        ports:
        - containerPort: 9080
        volumeMounts:
        - name: tmp
          mountPath: /tmp
        - name: wlp-output
          mountPath: /opt/ibm/wlp/output
        securityContext:
          runAsUser: 1000
      volumes:
      - name: wlp-output
        emptyDir: {}
      - name: tmp
        emptyDir: {}
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reviews-v3
  labels:
    app: reviews
    version: v3
spec:
  replicas: 1
  selector:
    matchLabels:
      app: reviews
      version: v3
  template:
    metadata:
      labels:
        app: reviews
        version: v3
    spec:
      serviceAccountName: bookinfo-reviews
      containers:
      - name: reviews
        image: docker.io/istio/examples-bookinfo-reviews-v3:1.17.0
        imagePullPolicy: IfNotPresent
        env:
        - name: LOG_DIR
          value: "/tmp/logs"
        ports:
        - containerPort: 9080
        volumeMounts:
        - name: tmp
          mountPath: /tmp
        - name: wlp-output
          mountPath: /opt/ibm/wlp/output
        securityContext:
          runAsUser: 1000
      volumes:
      - name: wlp-output
        emptyDir: {}
      - name: tmp
        emptyDir: {}
---
##################################################################################################
# Productpage services
##################################################################################################
apiVersion: v1
kind: Service
metadata:
  name: productpage
  labels:
    app: productpage
    service: productpage
spec:
  ports:
  - port: 9080
    name: http
  selector:
    app: productpage
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: bookinfo-productpage
  labels:
    account: productpage
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: productpage-v1
  labels:
    app: productpage
    version: v1
spec:
  replicas: 1
  selector:
    matchLabels:
      app: productpage
      version: v1
  template:
    metadata:
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/port: "9080"
        prometheus.io/path: "/metrics"
      labels:
        app: productpage
        version: v1
    spec:
      serviceAccountName: bookinfo-productpage
      containers:
      - name: productpage
        image: docker.io/istio/examples-bookinfo-productpage-v1:1.17.0
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 9080
        volumeMounts:
        - name: tmp
          mountPath: /tmp
        securityContext:
          runAsUser: 1000
      volumes:
      - name: tmp
        emptyDir: {}
---
```

网关

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: bookinfo-gateway
spec:
  # The selector matches the ingress gateway pod labels.
  # If you installed Istio using Helm following the standard documentation, this would be "istio=ingress"
  selector:
    istio: ingressgateway # use istio default controller
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*"
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: bookinfo
spec:
  hosts:
  - "*"
  gateways:
  - bookinfo-gateway
  http:
  - match:
    - uri:
        exact: /productpage
    - uri:
        prefix: /static
    - uri:
        exact: /login
    - uri:
        exact: /logout
    - uri:
        prefix: /api/v1/products
    route:
    - destination:
        host: productpage
        port:
          number: 9080
```

