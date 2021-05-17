# 微服务CICD注意事项

1. 微服务注册中心采用Eureka，部署后一般不会改动，直接手动部署，不放在cicd中；
2. service/ingress也不经常改动，直接在阿里ACK控制台中手动安装，或者单独部署Jenkins item发布；
3. cicd只发布各个微服务的deployment；
4. 根据获取到的build id来来作为镜像的tag；
5. ```sed -i "s/<BUILD_NUMBER>/${BUILD_NUMBER}/" "\${service_yaml}"```，注意后面的文件名从变量中获取，要加上""，另外"\"表示命令
