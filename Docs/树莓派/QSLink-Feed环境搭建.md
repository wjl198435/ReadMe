## 1.官网下载2020-02-13-raspbian-buster-lite.zip (453M)
   
## 2.配置wifi
```
/wpa_supplicant.conf 
country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
 
network={
ssid="HUAWEI-wang"
psk="wjl19840305"
key_mgmt=WPA-PSK
priority=1
}
```

## 3.ssh 登陆
   创建ssh 空文件
## 4.安装docker docker-compose
```
curl -sL get.docker.com|sh#######
sudo usermod -aG docker pi######

安装pip
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py 
sudo apt install -y libssl-dev python3-dev libffi-dev python-backports.ssl-match-hostname
 
sudo apt-get install python3-distutils
sudo python3 get-pip.py
pip --version

sudo pip install docker-compose

```

## pip 本地安装
```
 pip install aiohttp_cors
 pip install homeassistant
``` 
## Updating Docker
 To update docker you can simply run the install again.

 sudo /etc/init.d/docker stop
 curl -sL get.docker.com|sh
## Updating Docker Compose (docker-compose)
 sudo apt install --upgrade python python-pip libffi-dev python-backports.ssl-match-hostname
 sudo pip install --upgrade docker-compose 


## 创建docker-compose.yml
```
version: '3.7'
  services:
    homeassistant:
      container_name: homeassistant
      image: homeassistant/home-assistant:stable
      network_mode: "host"
      ports:
        - "8123:8123"
      volumes:
        - /opt/homeassistant:/config
        - /etc/localtime:/etc/localtime:ro
        - /etc/letsencrypt:/etc/letsencrypt:ro
      devices:
        - /dev/ttyACM0:/dev/ttyACM0:rwm
      restart: always
      healthcheck:
        test: ["CMD", "curl", "-f", "http://127.0.0.1:8123"]
        interval: 30s
        timeout: 10s
        retries: 6
```

## Create the following file for automating the service on startup
      /etc/systemd/system/home-assistant.service
```
[Unit]
Description=Home Assistant Service
Requires=docker.service
After=docker.service
[Service]
WorkingDirectory=/root
ExecStart=/usr/local/bin/docker-compose up
ExecStop=/usr/local/bin/docker-compose down
             
TimeoutStartSec=0
Restart=on-failure
StartLimitInterval
```
   
### Run this systemctl enable home-assistant.service
### Run this systemctl enable docker


## Raspberry Pi I2C配置
### 启动I2C 接口
sudo raspi-config

### 安装I2C 扫描工具

sudo apt-get install -y i2c-tools

