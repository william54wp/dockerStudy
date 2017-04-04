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
1. 搭建 wordpress
由两部分组成： mariadb wordpress
    1. mariadb : [daoCloud 镜像仓库说明](https://dashboard.daocloud.io/packages/b58db9a4-a808-4611-aaf0-d01e0acc0c5b) `docker run --name db --env MYSQL_ROOT_PASSWORD=example -d mariadb`
    1. wordpress : [daoCloud 镜像说明](https://dashboard.daocloud.io/packages/88b8f1e2-477d-49dd-ba3e-3466bfc2a489)`docker run --name myWordPress --link db:mysql -p 8080:80 -d wordpress`
    
1. 搭建 GitLab
由三部分组成： postgresql redis gitlab
    
    1. sameersbn/postgresql:9.4-12 [GitInfo](https://github.com/sameersbn/docker-postgresql/tree/9.4-12) `docker run -d --name gitlab-postgresql --env 'DB_USER=gitlab' --env 'DB_PASS=password' sameersbn/postgresql:9.4-12`

    1. sameersbn/redis:latest [docker store](https://store.docker.com/community/images/sameersbn/redis) `docker run -d --name gitlab-redis sameersbn/redis:latest`
    
    1. sameersbn/gitlab:8.4.4 [docker store](https://store.docker.com/community/images/sameersbn/gitlab)

        `docker run \
        --name gitlab \
        --link gitlab-postgresql:postresql \
        --link gitlab-redis:redisio -p 10022:22 -p 10080:80 \
        --env 'GITLAB_PORT=10080' \
        --env 'GITLAB_SSH_PORT=10022' \
        --env 'GITLAB_SECRETS_DB_KEY_BASE=long-and-random-alpha-numeric-string' \
        sameersbn/gitlab:8.4.4`