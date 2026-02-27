#Linux #WSL #PHP #Apache #DevEnv

Шпаргалка по установке и настройке стека для работы с PHP внутри Linux/WSL.

---
## 1. Установка ядра

```bash
sudo apt update && sudo apt upgrade
sudo apt install apache2
sudo apt install php libapache2-mod-php
sudo systemctl restart apache2
```

- *Стандартная корневая директория:*`/var/www/html`
- *Конфиги Apache лежат в:* `/etc/apache2/` (apache2.conf - глобалки).
## 2. Дополнительные зависимости

```bash
sudo apt install php-pgsql
sudo systemctl restart apache2
```
- (к примеру для работы с PostgreSQL)
## 3. Смена корневой директории #ПЕРЕДЕЛАТЬ 

Часто `/var/www/html` неудобен из-за прав доступа. Переносим корень в `~/localhost`:

```bash
# Создаем папку для проектов
mkdir ~/localhost

# Указываем новый DocumentRoot
echo "DocumentRoot /home/$USER/localhost" | sudo tee /etc/apache2/sites-available/000-default.conf 

# Даем Apache права на чтение из нашей домашней директории
echo "
<Directory /home/$USER/localhost/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
        Allow from all
</Directory>
" | sudo tee -a /etc/apache2/apache2.conf

# Выдаем права и создаем тестовый файл
chmod 755 $HOME
echo "<?php echo 'Hello World!'; ?>" > ~/localhost/index.php
sudo systemctl restart apache2
```

> **Примечание:** Если используется Firewall, не забыть прокинуть порты: `sudo ufw allow 'Apache Full' && sudo ufw enable`