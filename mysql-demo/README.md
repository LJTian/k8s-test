# 介绍

这个是一个 k8s 方式部署的 mysql(mariadb) 10.11 版本

# 背景

我需要将其部署到 minikube 集群中，设计到了端口的转发，端口的转发目前使用  nginx 方式。

# 使用方式
## 1、创建 mysql 密码
```shell
kubectl create secret generic mysql-secret -n mysql-db --from-literal=password="passwd"
```

## 2、应用 yaml 文件
```shell
kubectl apply -f mysql-full.yaml
``` 

## 3、设置转发
```shell
sysctl -w net.ipv4.ip_forward=1
iptables -t nat -A PREROUTING -p tcp --dport 3306 -j DNAT --to-destination 192.168.49.2:30306
iptables -t nat -A POSTROUTING -d 192.168.49.2 -p tcp --dport 30306 -j MASQUERADE
```
