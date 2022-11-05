# Домашнее задание

Настроить виртуальную машину на Yandex Cloud при помощи Ansible
Цель:

В результате домашнего задания, вы научитесь создавать конфигурацию для автоматизации развёртывания при помощи Ansible и установите необходимый софт на свою виртуальную машину в Yandex.Cloud'е

Описание/Пошаговая инструкция выполнения домашнего задания:

    Установить себе на рабочий компьютер Ansible
    Создать playbook для установки и запуска nginx и postgres на виртуальной машине
    Конфигурацию playbook'а залить на гит и указать ссылку

# Создание образа на Ubuntu
./packer build image.json

смотри фото. Дальше созданный образ можно использовать при создании виртуальной машины либо через UI
либо командой yc compute instance create 

https://cloud.yandex.ru/docs/tutorials/infrastructure-management/packer-custom-image

# выполнение только ansible playbook

если на машине есть python, ssh и целевой host есть в, то выполнить playbook можно командой:

В этом случае нужно в файле cloned_ubuntu.ini указать логин и пароль

## запрашивающей пароль

ansible-playbook -i /mnt/virtual_box/cloned_ubuntu.ini /mnt/virtual_box/nginx_and_postgres.yml -kK

## указав пароль в параметрах команды

    ansible-playbook -i /mnt/virtual_box/cloned_ubuntu.ini /mnt/virtual_box/nginx_and_postgres.yml --extra-vars "ansible_sudo_pass=<user password>"

# Настройка
# Установка packer
https://developer.hashicorp.com/packer/downloads
https://cloud.yandex.ru/docs/tutorials/infrastructure-management/packer-quickstart#install-packer

## Сгенерировать ssh ключ:
 (опционально - если ещё не сделано)

 ssh-keygen -t rsa -b 2048

https://practicum.yandex.ru/trainer/ycloud/lesson/467fb1f2-7eb4-421c-a33c-117e1cf86b66/

Enter file in which to save the key (C:\Users\USER/.ssh/id_rsa):

# Настройка параметров в image.json
##  "token":     "<OAuth-токен>",
 как получить ya_cloud_token описано здесь
https://cloud.yandex.ru/docs/iam/concepts/authorization/oauth-token
если вы зарегистрировались в Yandex Cloud, то пройдите по ссылке
https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb

## "folder_id": "<идентификатор каталога>",
id папки в Yandex Cloud
С помощью Yandex CLI.
  yc resource-manager folder create --name project-1
где project-1 - это имя папки.

Если папка уже создана, то воспользуйтесь командой
  yc resource-manager folder list

после это если выполнить
  yc config set folder-id b1111111111111111111v --profile default 
где b1111111111111111111v - folder_id
то Yandex CLI будет считать эту папку текущей

## "subnet_id":           "<идентификатор подсети>",
Можно узнать либо с помощью команды 
    yc vpc subnet list
либо в облаке выбрав virtual private cloud

## playbook_file
нужно путь изменить на свой 

# Настройки для ручного запуска ansible playbook

sudo apt-get install ssh
apt-get install python
apt install python3.8-venv
pipx install ansible-review

## Для проверки ansible playbook
apt install ansible-lint

# Проверка ansible playbook
ansible-lint /mnt/virtual_box/check_url.yml

ansible-playbook --syntax-check /mnt/virtual_box/nginx_and_postgres.yml

# hello world для ansible-playbook
ansible-playbook -i /mnt/virtual_box/host.ini /mnt/virtual_box/check_url.yml --step
ansible-playbook --syntax-check /mnt/virtual_box/check_url.yml