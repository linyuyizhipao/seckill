# seckill
> 秒杀系列

 ## 环境搭建
* http://laradock.io/documentation/ 这个laradock的docker镜像文件搭建环境还是挺好用的

### .   A.2）还没有PHP项目：
+ 使用 git clone https://github.com/laradock/laradock.git 那么变可按以下目录结构进行部署应用
+ laradock  （git拉下来的）
+ project-a  （自己后期拓展的应用）
+ project-b
### 在laradock中:   
>    cp env-example .env    
>初始化镜像即将搭建容器的配置文件，.env中的 APPLICATION 配置的路径为后面容器构建参考的根目录

> c. docker-compose up -d nginx mysql   
> 注意：所有Web服务器容器nginx，apache..依赖于php-fpm，这意味着如果您运行其中任何一个，它们将自动php-fpm为您启动容器，因此无需在up命令中明确指定它。

> 有兴趣可以自己去laradock里面看，有php开发所需要的很多容器，各容器的启动方式相近，若需则以次拓展即可

> 接下来我就当容器全部有了的情况下，这时我们可能还需要解决，更改nginx的配置的重启问题，php模块的增加编译安装问题
>> 我们git下来的其实就是一堆dockerfile，那么想要在外面更新容器配置并且生效，就需要在构建容器的是会就将外面的文件复制并覆盖容器在构建过程中的配置文件
>> laradock在每一个镜像文件中都预留的类似与 cp *.conf  *.conf 的命令，当然有些可能时作者处于安全考虑将注释了，但是如果我们需要自定义的话是可以通过此方法去实现的
>> 对于此处的 cp命令的修改虽然也是修改了dockerfile，但是只是为了容器配置文件是否在容器重启时是否覆盖容器里面的配置文件，并不是通过重构容器去控制配置更新的，只是一开始就让容器带有cp配置文件的功能，需要特别注意
>> 一次copy 后期配置文件更替便只需要不断重启容器

>php模块安装
>> 对于php的版本选择也是个份工作模式，与php-fpm模式
>> 这里的模块安装是通过变更dockerfile的内容，从而控制容器的，每一次的控制都是会将容器重构的，也就是通过重构容器的方式去自定义功能模块
>> 每一次的模块更新都将重构容器

> 虽然进入到容器中也能配置，但是不建议，不建议，重构容器就没了。

### 构建并运行
docker-compose up -d nginx mysql

### 进入容器（Workspace）
docker-compose exec workspace bash

###  您可以选择指定要重建的容器（而不是重建所有容器）：
docker-compose build {container-name}

###  查看日志文件  
>NGINX日志文件存储在logs/nginx目录中。
>docker-compose logs {container-name}

>但是，要查看所有其他容器（MySQL，PHP-FPM，...）的日志，您可以运行以下命令：
>docker-compose logs -f {container-name}
