# Требования:
1. ansible 2.1
2. python-lxml

# xenserver-update-downloader
Загрузка патчей для гипервизоров XenServer

## Возможности:
1. Загрузка патчей для любой версии гипервизоров XenServer
2. Отправка уведомлений email о загруженных обновлениях.

## Использование:
1. Внесите изменения в файл xenserver-update-downloader\vars\main.yml
 * Измините следующие парметры на свои:
   * ``` #mail_settings ```
 * Укажите для каких версии гипервизоров Вам нужны патчи, в виде укороченных имен патчей.
 
   ``` 
patch_name:
  - XS62ESP1
  - XS65ESP1
  - XS70E0 
   ```
   
### Обратите внимание, что данная роль запускается локально! И генерирует файлы для роли xenserver-update. 

# xenserver-update role
Установка патчей на гипервизор XenServer, посредством cli и ansible.

## Возможности:
1. Автоматическая установка патчей без перезапуска гипервизора/ов.
2. Автоматическая установка на N гипервизоров.

## Использование:
### Данная роль зависит от xenserver-update-downloader, сначала нужно запустить ее!
1. Внесите изменения в файл - xen-upd.yml
 * Измините следующие парметры на свои:
 
   ``` 
hosts: xen-example
remote_user: root 
   ```

2. Если Вам требуется перезагрузка или рестарт службы xe-tooltask-restart после установки патчей, привидите файл xen-upd.yml к следующему виду:

перезагрузка:
```
- hosts: xen-example
  gather_facts: False
  remote_user: root
  roles:
    - { role: xenserver-update, reboot: 1 }
```
xe-tooltask-restart:
```
- hosts: xen-example
  gather_facts: False
  remote_user: root
  roles:
    - { role: xenserver-update, toolstack: 1 }
```
