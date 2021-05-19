前言：
k8s中通过Jenkins部署微服务，本文只介绍发布相关注意事项，常见错误以及Jenkinsfile文件，其他如gitlab、Harbor安装，可查看我其他文章。
微服务注册中心采用Eureka，部署后一般不会改动，直接手动部署，不放在cicd中；cicd只发布各个微服务的deployment，service/ingress也不经常改动，直接在阿里ACK控制台中手动安装，或新建Jenkins job发布；扩缩容也通过阿里控制台完成，自建k8s，单独新建job来实现，不推荐写复杂的逻辑判断，将扩缩容、更新发布放在一个Jenkins 任务重实现。

## 1. jenkinsfile配置注意事项
1. 构建镜像的tag通过环境变量BUILD_NUMBER来命名；同时yaml中定义两个变量，镜像tag同理通过BUILD_NUMBER，yaml中的副本数replicas的值，通过parameter获取，传递进来；
2. 第三步部署时，有引用到第二部定义的配置文件变量 admin.kubeconfig，此处要注意，当yaml在工作目录时，才能引用，如果yaml文件是在子文件夹，会提示找不到文件。
3. Jenkinsfile文件见jenkins-pipeline-ms-k8s-helm.txt。

## 2. 关于变量
1. step中sh ""shell里面定义变量，并引用该变量时，需要加上“\$”声明作为shell变量来调用，例如service_name=\${service}-service，cd **\${service_name}**，shell和groovy变量调用方式相同，加上会在运行时获得变量值，作为shell变量；不加的话，会当做groovy变量调用，因为找不到会提示“groovy.lang.MissingPropertyException: No such property: service_yaml for class: groovy.lang.Binding”的错误。
2. ```sed -i "s/<BUILD_NUMBER>/${BUILD_NUMBER}/" "\${service_yaml}"```和```sed -i "s/<REPLICACOUNT>/${ReplicaCount}/" "\${service_yaml}"```，同理service_yaml需要加上“\”，另外sed命令引用变量获取文件名，需要加上双引号；

## 3. 关于更新发布
1.  仅当 Deployment Pod 模板（即 .spec.template）发生改变时，例如模板的标签或容器镜像被更新， 才会触发 Deployment 上线。 其他更新（如对 Deployment 执行扩缩容的操作）不会触发上线动作；因此当代码更新时才通过此发布更新；如果只是扩缩容，可以通过控制台操作伸缩，如下图所示：
![image](https://img-blog.csdnimg.cn/img_convert/eb934e01eae2fa46bc7da4faed294d27.png)
如果是自建K8S集群，只需要专门新建一个扩缩容的k8s，Jenkins job即可。
