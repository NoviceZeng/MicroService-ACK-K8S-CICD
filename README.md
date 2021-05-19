## 1 jenkinsfile配置注意事项
1. 微服务注册中心采用Eureka，部署后一般不会改动，直接手动部署，不放在cicd中；cicd只发布各个微服务的deployment；
2. service/ingress也不经常改动，直接在阿里ACK控制台中手动安装，或者单独部署Jenkins item发布；

## 2. 关于变量
1. step中sh ""里面定义变量，并引用该变量时，需要加上“\$”来调用，例如service_name=\${service}-service，cd **\${service_name}**，这样
6.  仅当 Deployment Pod 模板（即 .spec.template）发生改变时，例如模板的标签或容器镜像被更新， 才会触发 Deployment 上线。 其他更新（如对 Deployment 执行扩缩容的操作）不会触发上线动作。
