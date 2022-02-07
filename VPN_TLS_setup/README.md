# [RU] Инструкция по настройке wireguard через TLS

## Настраиваем обычный Wireguard VPN

### На стороне сервера!

**Шаг 1:** устанавливаем Wireguard management tools:
`sudo apt install wireguard` - Для APT систем

**Шаг 2:** Включаем IPv4 forwarding (если не сделали это ранее):

`sudo nano /etc/sysctl.d/99-sysctl.conf `

Раскомментируем строчку: `net.ipv4.ip_forward=1 `

Перезагружаем чтобы последнее вступило в силу:
`sudo systemctl reboot`


**Шаг 3.** Создаем с помощью генератора сразу конфиг. Сам генератор найти можно тут: https://www.wireguardconfig.com/

Меняем:
    `DNS` на 8.8.8.8.
    `Endpoint (Optional)` на IP адрес сервера.
    В строке `iptables -A FORWARD -i %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE` нужно заменить `eth0` на имя нашего интерфейса на сервере. Узнать его можно через команду `ip a`
    
**Шаг 4.** Копируем из скачанного архива файл `server.conf` на сервер:

`sudo nano /etc/wireguard/wg0.conf` (в видео вместо `wg0` было `wgtls`).

При желании можно еще раз убедиться, что мы правильно ввели имя интерфейса.

**Шаг 5.**

Включаем скопированный файл: `sudo systemctl enable --now wg-quick@wg0.service`.
Проверить что все ок можно командой `sudo systemctl status wg-quick@wg0.service`

### На стороне Клиента!

Копируем конфиг клиента из архива себе.


## Настраиваем TLS туннель поверх

*To be continued...*
