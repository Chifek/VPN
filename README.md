# VPN в каждый дом или как приручить Дракона


Ниже я расскажу о том, как заменить ваш VPN провайдер собственным сервером, развернутым на DigitalOcean с использованием WireGuard.


В чем главная проблема VPN провайдеров? Вы не знаете что они делают с вашими данными.
Очень мало VPN провайдеров прошли сторонний аудит и почти никто из них не открывает свой код.
Даже в случае открытого кода и пройденного аудита, для параноиков вопрос про то, что же происходит на стороне провайдера — остается открытым.


Решение достаточно простое — развернуть свою VPN ноду.



Я хочу сделать это просто

В сети существует достаточно много статей о том, как настроить WireGuard, вот некоторые из них:


https://www.wireguard.com/quickstart/
https://www.stavros.io/posts/how-to-configure-wireguard/
https://grh.am/2018/wireguard-setup-guide-for-ios/
https://www.digitalocean.com/community/tutorials/how-to-create-a-point-to-point-vpn-with-wireguard-on-ubuntu-16-04
https://www.linode.com/docs/networking/vpn/set-up-wireguard-vpn-on-ubuntu/

Но что, если я просто хочу установить WireGuard, без глубокого изучения документации?
Я просто хочу наиболее простым и быстрым способом развернуть VPN сервер и начать использовать его.


Все что мне нужно от инфраструктуры:


1 сервер
5–10 клиентов для меня и моих близких

Ниже — инструкция, как сделать это быстро и просто.


Создайте дроплет

Сначала вам нужно создать новый дроплет на DigitalOcean: https://www.digitalocean.com/docs/droplets/how-to/create/


Мне подходит самый простой c Ubuntu 18.04, который стоит 5$ в месяц.


Не забудьте добавить свой SSH ключ, чтобы иметь доступ к дроплету: https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/


Замечание. DigitalOcean — не единственно возможный вариант. Вы можете выбрать любой облачный сервис на ваш вкус.


Установите сервер WireGuard и создайте все необходимые конфигурации


Чтобы создать все необходимые конфигурации автоматически, вы можете использовать скрипт: wg-ubuntu-server-up.sh, который:


установит весь необходимый софт
настроит правила iptables и включит IPv4 forwarding
установит unbound в качестве dns resolver
создаст серверную конфигурацию и необходимое количество клиентских конфигураций
запустит WireGuard

Установите соединение с дроплетом через SSH и выполните следующие команды, чтобы скачать и запустить скрипт (используйте IP адрес вашего дроплета, вместо xxx.xxx.x.xx):


ssh root@xxx.xxx.x.xx

wget https://raw.githubusercontent.com/drew2a/wireguard/master/wg-ubuntu-server-up.sh
chmod +x ./wg-ubuntu-server-up.sh

sudo ./wg-ubuntu-server-up.sh


После выполнения скрипта, сервер WireGuard будет установлен, запущен и готов к работе с клиентами.


Признак корректного запуска WireGuard — после отработки скрипта, вы должны увидеть в консоли что-то похожее на:


interface: wg0
public key: +xxxEjj1qmxxxotq4OxxxfHPaxxxtre5xxxxOfxxw=
private key: (hidden)
listening port: 51820

peer: d1exxxLdCZcYxxxIQ0xxxxK/Wpx8G1N8xxvnUrxxxx=
allowed ips: 10.0.0.2/32

peer: fWExxxazRxxxUOxxxx4JKgUTxxo9LaxxxxOGWtxxK0w=
allowed ips: 10.0.0.3/32

...

peer: RbmxxxDxOoXMxxxcyate6xxxinIClxxDgRDxxxx0j0=
allowed ips: 10.0.0.10/32
