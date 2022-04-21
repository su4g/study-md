安装宝塔

yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh



ssl和配置

```json
#SSL-END
location /v2 
{
    proxy_pass http://127.0.0.1:18668;
    proxy_redirect off;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $http_host;
    proxy_read_timeout 300s;

}
```



v2ray安装

source <(curl -sL https://multi.netlify.app/v2ray.sh) –zh



修改时区同步时间

\cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

yum install ntp ntpdate -y

service ntpd stop

ntpdate us.pool.ntp.org

service ntpd start



停用防火墙

systemctl stop firewalld.service



配置v2ray

```json

  "log": {
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log",
    "loglevel": "info"
  },
  "policy": {
    "levels": {
      "0": {
        "uplinkOnly": 0,
        "downlinkOnly": 0,
        "connIdle": 150,
        "handshake": 4
      }
    }
  },
  "inbounds": [
    { 
      "listen": "127.0.0.1",
      "port": 18668,
      "protocol": "vmess",
      "settings": { 
        "clients": [
          {
            "alterId": 0,
            "level": 1,
            "id": "8a29394a-357d-11ec-9ced-560003a48d48"
          } 
        ]
      }, 
      "streamSettings": {
        "network": "ws", 
        "security": "auto",
        "wsSettings": {
          "path": "/v2",
          "headers": {
            "Host": "www.suag.top"
          }
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": [
          "0.0.0.0/8",
          "10.0.0.0/8",
          "100.64.0.0/10",
          "169.254.0.0/16",
          "172.16.0.0/12",
          "192.0.0.0/24",
          "192.0.2.0/24",
          "192.168.0.0/16",
          "198.18.0.0/15",
          "198.51.100.0/24",
          "203.0.113.0/24",
          "::1/128",
          "fc00::/7",
          "fe80::/10"
        ],
        "outboundTag": "blocked"
      }
    ]
  }
```



添加中转

https://www.cloudflare.com/zh-cn/



client

![image-20211026105548815](.\typora-user-images\image-20211026105548815.png)