
### mongodb 安装和运行

[Install on Red Hat](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/)

1. 安装
    [Install MongoDB Community Edition on Red Hat or CentOS](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/#std-label-install-mdb-community-redhat-centos)
    ``` shell
    vim /etc/yum.repos.d/mongodb-org-7.0.repo
    ```
    ``` repo
    [mongodb-org-7.0]
    name=MongoDB Repository
    baseurl=https://repo.mongodb.org/yum/redhat/7/mongodb-org/7.0/x86_64/
    gpgcheck=1
    enabled=1
    gpgkey=https://www.mongodb.org/static/pgp/server-7.0.asc
    ```
    ``` shell
    sudo yum install -y mongodb-org
    ```



2. 运行
    [Run MongoDB Community Edition](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-red-hat/#run-mongodb-community-edition)
    默认目录：
    - /var/lib/mongo (the data directory)
    - /var/log/mongodb (the log directory)
    
    自定义：
    - 准备环境
        ``` shell
        mongodb_custom_path=~/programs/mongodb
        mkdir -p $mongodb_custom_path/lib/mongo
        mkdir -p $mongodb_custom_path/log/mongodb
        sudo chown -R mongod:mongod $mongodb_custom_path/lib/mongo
        sudo chown -R mongod:mongod $mongodb_custom_path/log/mongodb
        ```
    - 启动
        ``` shell
        mongod --dbpath $mongodb_custom_path/lib/mongo --logpath $mongodb_custom_path/log/mongodb --bind_ip 0.0.0.0 --fork
        # bind_ip 默认是127.0.0.1；使用0.0.0.0可以绑定到所有IPv4和IPv6的地址；可以使用bind_ip_all代替绑定所有IP。
        # --bind_ip arg：Comma separated list of ip addresses to listen on - localhost by default
        #  --bind_ip_all：Bind to all ip addresses

        # --fork：Fork server process
        
        # 默认配置文件（驼峰式配置）：vim /etc/mongod.conf
        # 例如：
        # Enter 0.0.0.0,:: to bind to all IPv4 and IPv6 addresses or, alternatively, use the net.bindIpAll setting.
        ```
3. 