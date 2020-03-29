# Руководство конфигурированию и установке программ на сервер Ubuntu

## Подключение по SSH
Для быстрого и удобного подключения SSH из под Windows установить любой клиент, например
[Bitwise SSH Client](https://www.bitvise.com/download-area)

## Конфигурирование SSH
Для обеспечения безопасности подключения рекомендуется использовать только подключение через приватному ключу, отключить аутентификацию при помощи пароля
#### Сгенерировать SSH ключ
```
ssh-keygen -t rsa -b 4096
/etc/ssh/sshd_config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.original
touch  ~/.ssh/authorized_keys
cat ~/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
service ssh restart
systemctl restart sshd
```
#### Отключить аутентификацию при помощи пароля
```
nano /etc/ssh/sshd_config
```
set parameter below to "no"
PasswordAuthentication no

## Установить Nginx
```
sudo apt-get update
sudo apt-get install nginx
```

## Установить Certbot
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt update
sudo apt-get install certbot python-certbot-nginx
```

## Установить и настроить UFW
Это firewall, нужно закрыть все порты, кроме 80 (http), 443 (https), 22 (ssh)
[help.ubuntu.ru](https://help.ubuntu.ru/wiki/%D1%80%D1%83%D0%BA%D0%BE%D0%B2%D0%BE%D0%B4%D1%81%D1%82%D0%B2%D0%BE_%D0%BF%D0%BE_ubuntu_server/%D0%B1%D0%B5%D0%B7%D0%BE%D0%BF%D0%B0%D1%81%D0%BD%D0%BE%D1%81%D1%82%D1%8C/firewall)
```
apt-get install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
ufw allow 22
ufw allow 80
ufw allow 443
ufw enable
sudo systemctl enable ufw
```

## Установить docker
[Docker for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
```
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

[Docker Compose](https://docs.docker.com/compose/install/)
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```