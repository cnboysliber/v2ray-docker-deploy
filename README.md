# V2Ray Docker 使用说明

### 准备工作（适用于新服务器）
1. 一个干净的服务器是缺少一些工具的，需要自己装一下，比如：wget、git、docker、docker-compose 这些（具体咋装软件如有需要再补充吧），安装好后，克隆本项目，再把 docker 服务开起来；
#### <a href="https://www.vultr.com/?ref=7573170" title="服务器申请">服务器申请</a>, 注册一个账号后即可购买，一个月最便宜(纽约的服务器)是3.5美元，可以多人共用，还是很划算的，如果想网络更快，可以购买新加坡或者其他亚洲地区的，最便宜是5美元，可以支付宝直接充值，服务器申请后可以随时销毁，实际是按小时计费的，所以不想用时可以销毁，用的时候再部署就可以了，很方便
<img width="1087" alt="image" src="https://user-images.githubusercontent.com/31284807/195549881-7e8a385c-fe54-4b50-a145-f6e73aed5732.png">
  
2. 准备一个域名，[免费域名](https://my.freenom.com/clientarea.php), 每次申请一个域名免费时间为12个月，可以到期后再重新申请一个就好，然后把 A 记录解析改为你服务器 ip。

3. 服务器免费证书安装，可以使用[acme.sh脚本](https://github.com/chengboliber/v2ray-docker-deploy/blob/master/TLS.md)

4. docker 安装：
```
curl -sSL https://get.daocloud.io/docker | sh
```
 如果要卸载老的版本：
  ```
  sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
  ```
 

### 服务端
1. 使用 [GGUID](https://www.guidgen.com/) 生成一个 ID 标识，然后打开 `./v2ray/config.json` 文件，修改[第 9 行](https://github.com/chengboliber/v2ray-docker-deploy/blob/master/v2ray/config.json#L9)的 ID 标识；

2. 看这个说明安装 [acme.sh](https://github.com/chengboliber/v2ray-docker-deploy/blob/master/TLS.md)，安装好后，按照说明申请证书（最省心的方法是执行这个命令：`acme.sh --issue -d yourdomain.com --standalone`，就会在 `~/.acme.sh/` 目录下生成好证书），把 `yourdomain.ctr 和 yourdomain.key` 这两个重命名为 `v2ray.ctr 和 v2ray.key`，接着把它们拷贝到 `./nginx/certs/` 目录下。

> 注意：如果没有生成 `.ctr` 文件，而是生成了 `.cer` 文件，请修改 `./nginx/conf.d/v2ray.conf` [第 22 行](https://github.com/chengboliber/v2ray-docker-deploy/blob/master/nginx/conf.d/v2ray.conf#L22) 的后缀。

3. 修改 `./nginx/conf.d/v2ray.conf` [第 8 行](https://github.com/chengboliber/v2ray-docker-deploy/blob/master/nginx/conf.d/v2ray.conf#L8) 的域名地址。

4. 启动临时服务测试一下：`docker-compose up`，如果测试没问题，把这服务关了，执行这个开启服务：`docker-compose up -d`


### 客户端
1. 从 [神一样的工具们](https://www.v2ray.com/awesome/tools.html) 下载当前环境的客户端；
2. 按下图设置客户端即可：
![](./images/client.png)


### 资源
GGUID 生成器：https://www.guidgen.com/  
免费域名：https://www.freenom.com/zh/freeandpaiddomains.html  
acme.sh：https://github.com/chengboliber/v2ray-docker-deploy/blob/master/TLS.md
