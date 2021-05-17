# 微服务CICD注意事项

1. 微服务注册中心采用Eureka，部署后一般不会改动，直接手动部署，不放在cicd中；
2. service/ingress也不经常改动，直接在阿里ACK控制台中手动安装，或者单独部署Jenkins item发布；
3. cicd只发布各个微服务的deployment；
