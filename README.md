# Semen
## 05_GCP01: cloud-bastion
#### Задание:
Самостоятельное задание
Исследовать способ подключения к someinternalhost в одну
команду из вашего рабочего устройства, проверить
работоспособность найденного решения и внести его в
README.md в вашем репозитории
Дополнительное задание:
Предложить вариант решения для подключения из консоли при
помощи команды вида ssh someinternalhost из локальной
консоли рабочего устройства, чтобы подключение выполнялось по
алиасу someinternalhost и внести его в README.md в вашем
репозитории

#### Решение:
``` ssh-keygen -t rsa -f ~/.ssh/appuser -C appuser -P ```  гененрирую пару ключей для пользователя appuser

`cat ~/.ssh/appuser.pub`  kопирую содержимое публичного  ключа и добавляю его в GKE в разделе метаданные, SSH ключи

`ps -C ssh-agent`  Удостоверяюсь, что запущен ssh-agent

`ssh-add -L`  подгружаю ключ


`nano ~/.ssh/config`  открывю для редактирования конфиг


`Host *`

`ForwardAgent yes`

`Host bastion`

`HostName 35.210.86.121` определяю внешний адрес хоста bastion

`User appuser` определяю пользователя по умолчанию для хоста bastion

`IdentityFile ~/.ssh/appuser` определяю id_rsa для хоста bastion

`Host someinternalhost`

`HostName 10.154.0.2`  определяю внутренний адрес хоста someinternalhost

`User appuser` определяю пользователя по умолчанию для хоста someinternalhost

`IdentityFile ~/.ssh/appuser` определяю id_rsa для хоста someinternalhost

`ProxyCommand ssh bastion` nc %h %p  опеределяю ssh проксирование через хост bastion`

`ssh bastion` захожу по имени хоста без указания  флага -A и имени пользователя

`ssh someinternalhost`  захожу по имени хоста с внутренним ip в одну команду 


#### Задание
1. Выполните задание про подключение через бастион хост.
2. Добавьте в ваш репозиторий Infra (ветка cloud-bastion):
файл setupvpn.sh
конфигурационный файл для подключения к VPN
(переименуйте *.ovpn в cloud-bastion.ovpn)
3. Опишите в README.md и получившуюся конфигурацию и данные
для подключения в следующем формате (важно для проверки!):

#### Решение:

bastion_IP = 35.210.86.121
someinternalhost_IP = 10.154.0.2
