# 使用Azure+SSR翻墙

## 先决条件

* 拥有Azure Global账户
* 已安装Azure Cli
* 已安装ShadowRocket客户端

## 步骤

1. 登陆Azure Cli

    ``` bash
    az login
    ```

2. abc

    ``` bash
    export SSR_GROUP=shadowsocks
    export SSR_NAME=shadowsocks1
    export SSR_PORT=8388
    ```

3. 使用Azure Cli创建Azure Container Instance作为ShadowRocket服务器。
    ``` bash
    # 新建资源组
    az group create -n $SSR_GROUP -l southeastasia
    # 启动Container Instance
    az container create -g shadowsocks --name $SSR_NAME \
        --image oddrationale/docker-shadowsocks \
        --ip-address public \
        --ports $SSR_PORT \
        --command-line "/usr/local/bin/ssserver -k password1"
    ```

4. 获取Container Instance的Endpoint Ip与端口

    ``` bash
    az container show -g $SSR_GROUP -n $SSR_NAME | grep '\("ip":\)\|\("port":\)'
    ```

5. 启动ShadowRocket客户端，并输入服务器Ip，端口已经密码

6. 关闭代理

7. 删除Azure Container Instance

    ``` bash
    az container delete -g $SSR_GROUP -n $SSR_NAME -y
    ```

## 相关资源

