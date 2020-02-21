提前申请docker hub ID

# 步骤1：安装 Docker Desktop
1. 命令直接下载：$ brew cask install docker
2. 官网下载：https://hub.docker.com/editions/community/docker-ce-desktop-mac
3. 国内下载：http://mirrors.aliyun.com/docker-toolbox/mac/docker-for-mac/

# 步骤2：配置国内镜像
！[image](/Users/ybenny/Documents/未命名文件夹/mirror.png)

```
{
  "insecure-registries":[],
  "registry-mirrors": [
    "http://hub-mirror.c.163.com/",
    "https://docker.mirrors.ustc.edu.cn",
    "https://61c34qp8.mirror.aliyuncs.com"
  ],
  "experimental": false,
  "debug": true
}
```
点击“Apply&Restart”按钮

# 步骤3：下载 Kubernetes 镜像
1. 拉取github仓库
```
$ git clone https://github.com/maguowei/k8s-docker-for-mac.git
```
2. 拉取镜像
```
$ cd k8s-docker-for-mac/
$ ./load_images.sh
```
3. 查看获得的镜像
```
$ docker images
```
4. 验证是否安装成功
```
$ kubectl cluster-info
$ kubectl get nodes
```


# 步骤4、安装Kubernetes  dashboard
1. 部署 kubernetes-dashboard.yaml
```
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```
2. 开启本机访问代理
```
$ kubectl proxy
```
3. 通过浏览器访问 
> http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
4. 获取token
```
TOKEN=$(kubectl -n kube-system describe secret default| awk '$1=="token:"{print $2}')
kubectl config set-credentials docker-desktop --token="${TOKEN}"
echo $TOKEN
```
> eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZWZhdWx0LXRva2VuLXN2NHZwIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRlZmF1bHQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJmNWM1ZGI1MC1jZjMxLTRkMmYtOWRjOS0zMGI5NDE0ZWJiODciLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZGVmYXVsdCJ9.eu75_e8-qmctu34dd-DB3GyYlZwCXfMqhfq5ohJ1rLTOU0dnrOHFjCfLpyyDSTtzB9YNhPYIG4t5hO3ucDywMncjwOuic2RhQqZT_FiU0UVvngy4BT_tbPdNHIRG07GQxsCDYsECynZnpWHBr8qaiRMRAV9gDFiRFjLlZT4csUs7vL65dhv1UZ3_U-tZ5jnJfTHrhPi907XhySETElJvYL-TuPcc3i4dosL7JIXFnGbBy5UIk-av3ll41VvcUjgWQZ931M40oUWdXWaJZlbp8A0Q4fKjYOk6wbLnJeUVu6ewGSinqM4-NZWLgY95qtOr7M27nyRs56_flQNHG0PKiA

