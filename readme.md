# 学习Docker
## 搭建环境
### 安装 virtualbox , 下载 ubuntu Server ISO, git for windows , VisualStudio Code ,openssh for windows
1. 安装 code , git
1. 建立项目文件夹 dockerStudy
1. 初始化 dockerStudy git项目 测试同步
### 建立 Ubuntu Server 虚拟机
1. 配置为网桥模式
1. 启动系统
1. 测试SSH
1. 更新软件源
### 下载 Docker
1. 配置Docker软件源，安装 Docker
## Docker 学习
### Docker 系统搭建练习
1. 搭建 wordpress 由两部分组成： mariadb wordpress : 测试地址：[http://localhost:8080/](http://localhost:8080/) 
    1. mariadb : [ImageInfo](https://dashboard.daocloud.io/packages/b58db9a4-a808-4611-aaf0-d01e0acc0c5b)
        
            docker run --name db --env MYSQL_ROOT_PASSWORD=example -d mariadb

    1. wordpress : [ImageInfo](https://dashboard.daocloud.io/packages/88b8f1e2-477d-49dd-ba3e-3466bfc2a489)
        
            docker run --name myWordPress --link db:mysql -p 8080:80 -d wordpress
    
1. 搭建 GitLab 由三部分组成： postgresql redis gitlab 测试地址：[http://localhost:10080](http://localhost:10080)
    
    1. sameersbn/postgresql:9.4-12 [ImageInfo](https://github.com/sameersbn/docker-postgresql/tree/9.4-12)
    
            docker run --name gitlab-postgresql -d \
                --env 'DB_NAME=gitlabhq_production' \
                --env 'DB_USER=gitlab' --env 'DB_PASS=password' \
                --env 'DB_EXTENSION=pg_trgm' \
                sameersbn/postgresql:9.4-12</pre></code>

    1. sameersbn/redis:latest [ImageInfo](https://store.docker.com/community/images/sameersbn/redis)
        
            docker run --name gitlab-redis -d \
                sameersbn/redis:latest
    
    1. sameersbn/gitlab:8.4.4 [ImageInfo](https://store.docker.com/community/images/sameersbn/gitlab)
        
            docker run --name gitlab -d \
                --link gitlab-postgresql:postgresql --link gitlab-redis:redisio \
                --publish 10022:22 --publish 10080:80 \
                --env 'GITLAB_PORT=10080' --env 'GITLAB_SSH_PORT=10022' \
                --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' \
                --env 'GITLAB_SECRETS_SECRET_KEY_BASE=long-and-random-alpha-numeric-string' \
                --env 'GITLAB_SECRETS_OTP_KEY_BASE=long-and-random-alpha-numeric-string' \
                --volume /srv/docker/gitlab/gitlab:/home/git/data \
                sameersbn/gitlab:8.4.4
1. 搭建 redmine 服务 由两部分组成： postgresql,redmine 测试地址：[http://locahlost:10083](http://locahlost:10083)
    1. sameersbn/postgresql:9.4-12
            
            docker run --name=postgresql-redmine -d \
                --env='DB_NAME=redmine_production' \
                --env='DB_USER=redmine' --env='DB_PASS=password' \
                sameersbn/postgresql:9.4-12
            
    1. sameersbn/redmine:3.2.0-4
            
            docker run --name=redmine -d \
                --link=postgresql-redmine:postgresql --publish=10083:80 \
                --env='REDMINE_PORT=10083' \
                --volume=/srv/docker/redmine/redmine:/home/redmine/data \
                sameersbn/redmine:3.2.0-4
### docker study
1. 查询 Docker 版本号

        docker version

1. 查询镜像

        docker search [string]

1. 查询容器信息

        docker inspect

1. 下载镜像

        docker pull [imageName]

1. docker-compose

    1. 安装 docker-compose

            sudo apt-get install docker-compose

    1. 应用 docker-compose

                wordpress:
                image: wordpress
                links:
                - db:mysql
                ports:
                - 8080:80
                db:
                images:mariadb
                environment:
                MYSQL_ROOT_PASSWORD:example