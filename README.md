# istio-demo
java springboot 工程，部署在istio环境中。

包含三个工程
- istio-product 产品服务
- istio-store 商店服务
- remote 公共依赖

# 打包
```
mvn package -DskipTests
```
执行mvn命令后会字段打包成镜像，如果有habor仓库，可以自行推送到仓库中。

# 部署
准备 k8s 和 istio环境，本文使用的 k8s v1.18.5 / istio 1.7

运行istio,设置自动注入
```
istioctl install --set profile=demo
kubectl label namespace default istio-injection=enabled
```

应用 istio.yaml 资源
```
kubectl apply -f istio.yaml
```
查看service
```
kubectl get svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP    57d
product      ClusterIP   10.101.176.11   <none>        8080/TCP   5m57s
store        ClusterIP   10.101.60.102   <none>        8080/TCP   5m57s
```
调用store的接口
```
curl 10.101.60.102:8080/list
```
可以看到返回结果，v1 和 v2 两个版本

```
product list1.0
product list2.0
product list1.0
product list1.0
product list1.0
product list2.0
```






