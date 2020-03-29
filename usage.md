# Руководство по использованию программ на сервере Ubuntu

## Certbot
```
certbot --nginx -d example.com -d www.example.com
```
wildcard subdomains:
```
certbot certonly -d *.example.com --manual
```

## Nginx
```
edit /etc/nginx
nginx -s reload или systemctl restart nginx
```

Решить проблему "could not build server_names_hash, you should increase server_names_hash_bucket_size: 32"
Открыть /etc/nginx/nginx.conf, раскомментировать # server_names_hash_bucket_size 64;

Выпустить сертификат selfsigned для default server
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt

## Общее
Использование памяти в МБ
```
ps aux | awk '{print $6/1024 " MB\t\t" $11}' | sort -n
```
Дисковое пространство
```
du -h --max-depth=1 | sort -hr
```

## Архиватор tar (+bz2/gz)
Почему tar? Потому-что сохраняет linux разрешения файлов. Tar по умолчанию ничего не сжимает.
Для сжатия можно использовать gz или bz2
Создать архив (использование -v опционально, но удобно видеть что пакуешь)
```
tar -cvf file.tar path
```
Извлечь
```
tar -xvf file.tar
```
Добавить сжатие gz
```
tar -cvzf file.tar path
tar -xvzf file.tar
```
Добавить сжатие bz2
```
tar -cvjf file.tar path
tar -xvjf file.tar
```

## Docker

Использование ресурсов
```
docker stats --no-stream
docker system df
docker system df -v
```

Удалить неиспользуемые образы
```
docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
docker image prune
```

#### Docker Compose
```
docker-compose up -d
docker-compose restart {service_name}
docker-compose stop
```

#### Swarm
Включить/выключить режим swarm
```
docker swarm init
docker swarm leave
```

Запустить с переменными среды из .env
```
env $(cat .env | grep ^[A-Z] | xargs) docker stack deploy --with-registry-auth --compose-file docker-compose.yml garden
```



