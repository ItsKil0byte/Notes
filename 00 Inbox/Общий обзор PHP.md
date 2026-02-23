#ЯП #PHP

Source - https://metanit.com/php/tutorial/1.1.php

- Один из наиболее распространённых языков программирования для веб-сегмента.
- Имеет низкий порог вхождения.
- Обладает Си-подобным синтаксисом.

# Установка на Linux/WSL

```bash
sudo apt update
sudo apt upgrade

sudo apt install apache2
```

**Файлы конфигурации для Apache - `/etc/apache2/`:**
- apache2.conf - глобальные настройки.
- envvars - переменные окружения.
- magic - определение MIME типов.
- ports - TCP-порты для запуска.

- Стандартная корневая директория - `/var/www/html`.

```bash
sudo apt install php libapache2-mod-php
sudo systemctl restart apache2
```

**Установка доп зависимотей (к примеру для работы с PostgreSQL:**

```bash
sudo apt install php-pgsql
sudo systemctl restart apache2.service
```

**Если нужно настроить firewall:**

```bash
sudo ufw allow 'Apache Full'
sudo ufw enable
```

**Настройка домашней директории для Apache:**

#ПЕРЕДЕЛАТЬ

```bash
# [USER] - имя пользователя

mkdir ~/localhost
echo "DocumentRoot /home/[USER]/localhost" > /etc/apache2/sites-available/000-default.conf 

echo "
<Directory /home/[USER]/localhost/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
        Allow from all
</Directory>
" > /etc/apache2/apache2.conf

chmod 755 $HOME
echo "Hello World!" > ~/localhost/index.html
sudo systemctl restart apache2ls -la.service
```

