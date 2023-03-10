# Настройка центрального сервера для сбора логов.

## Описание домашнего задания:
1. В `vagrant` поднимаем 2 машины `web` и `elk`.
2. На web поднимаем `nginx`.
3. На `log` настраиваем центральный лог сервер на системе `ELK`.

## Описание реализации:

### Краткое описание [Vagrantfile](https://github.com/shulgazavr/logs/blob/main/Vagrantfile):
- Описание параметров создаваемых серверов:
```
servers = [
    { :hostname => 'elk', :ips => '192.168.31.201', :ram => 5120, :cpuz => 2 },
    { :hostname => 'web', :ips => '192.168.31.202', :ram => 762, :cpuz => 1 },
]
```

- Настройка параметров сети:
```
            node.vm.network :public_network, ip: machine[:ips], bridge: "wlp58s0"
```

> Примечание: ip-адреса и имя интерфейса выбраны исходя из особенности окружения.

- Опционально можно положить свой публичный ключ `id_rsa.pub` в каталог с `Vagrantfile` и раскомментировать строки для доступа к серверам по ssh:
```
...
#            node.vm.provision "file", source: "id_rsa.pub", destination: "/home/vagrant/id_rsa.pub"
...
#                sudo cat id_rsa.pub >> /root/.ssh/authorized_keys
#                rm id_rsa.pub
...
```
### Краткое описание `playbook`-файлов (основное функциональное решение):
1. Копирование `rpm`-пакетов на сервер (используемая версия стэка: 7.17.3).
2. Установка.
3. Копирование конфигурационных файлов.
4. Запуск сервиса и добавление его в автозагрузку.

> Примечание: установочные пакеты необходимы поместить в директорию playbooks/files/rpms. [Скачать](https://drive.google.com/drive/folders/1ahS8U-gZpy6cqaeI6SEnlgQED1gm4EM5?usp=sharing)
### Итог:
В результате выполнения команды `vagrant up`, полуается централизованное место хранения лог-файлов 'nginx'.


![image](https://user-images.githubusercontent.com/105816449/221717011-63a9db92-f449-46ee-9902-3699357890e5.png)
