# Домашнее задание к занятию «Введение в Terraform»

## Подготовка к выполнению заданий.

Установлена версия Terraform:

<img width="521" height="69" alt="image" src="https://github.com/user-attachments/assets/73e98d43-87eb-488b-bd92-d49675490a66" />

Скачан git репозиторий:

<img width="756" height="245" alt="image" src="https://github.com/user-attachments/assets/74b4b7c5-cdf6-4595-9485-870badc8823c" />

Установлен docker:

<img width="510" height="446" alt="image" src="https://github.com/user-attachments/assets/ca31ef2f-8588-49cf-8d79-1b4e6d2b92c8" />

## Задание 1
### 1. Скачать все необходимые зависимости.
Все действия делаю через VS CODE.
```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> terraform init 
Initializing the backend...
Initializing provider plugins...
- Finding latest version of kreuzwerker/docker...
- Finding latest version of hashicorp/random...
- Installing kreuzwerker/docker v4.5.0...
- Installed kreuzwerker/docker v4.5.0 (unauthenticated)
- Installing hashicorp/random v3.9.0...
- Installed hashicorp/random v3.9.0 (unauthenticated)
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

╷
│ Warning: Incomplete lock file information for providers
│ 
│ Due to your customized provider installation methods, Terraform was forced to calculate lock file
│ checksums locally for the following providers:
│   - hashicorp/random
│   - kreuzwerker/docker
│ 
│ The current .terraform.lock.hcl file only includes checksums for windows_amd64, so Terraform running on
│ another platform will fail to install these providers.
│ 
│ To calculate additional checksums for another platform, run:
│   terraform providers lock -platform=linux_amd64
│ (where linux_amd64 is the platform to generate)
╵
Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> terraform providers

Providers required by configuration:
.
├── provider[registry.terraform.io/kreuzwerker/docker]
└── provider[registry.terraform.io/hashicorp/random]

PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src>  
```

### 2 Изучите файл .gitignore.
Изучив файл .gitignore личную и секретную информацию допустимо хранить в файле: personal.auto.tfvars
Этот файл исключен из контроля версий, и не должен попадать в репозиторий.|
### 3 Выполните код проекта.
Сначала проверим что он хочет применить:
```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # random_password.random_string will be created
  + resource "random_password" "random_string" {
      + bcrypt_hash = (sensitive value)
      + id          = (known after apply)
      + length      = 16
      + lower       = true
      + min_lower   = 1
      + min_numeric = 1
      + min_special = 0
      + min_upper   = 1
      + number      = true
      + numeric     = true
      + result      = (sensitive value)
      + special     = false
      + upper       = true
    }

Plan: 1 to add, 0 to change, 0 to destroy.

───────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these
actions if you run "terraform apply" now.
```

Далее применяем:
```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # random_password.random_string will be created
  + resource "random_password" "random_string" {
      + bcrypt_hash = (sensitive value)
      + id          = (known after apply)
      + length      = 16
      + lower       = true
      + min_lower   = 1
      + min_numeric = 1
      + min_special = 0
      + min_upper   = 1
      + number      = true
      + numeric     = true
      + result      = (sensitive value)
      + special     = false
      + upper       = true
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

random_password.random_string: Creating...
random_password.random_string: Creation complete after 0s [id=none]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

Секретное содержимое в файле tfstate:

```
Ключ: result
Значение: JcIakflD72ZSnWQU
```
### 4 Расскоментировал блок с 22 - 37 (именно тут было закомментировано) и выполнив terraform validate возникает ошибка:

<img width="751" height="356" alt="image" src="https://github.com/user-attachments/assets/01568208-f19b-45f2-b0ea-be42ef61e16a" />

Исходя из ошибки, можно сделать два вывода:
  У docker_image отсутствует локальное имя.
  Имя 1nginx начинается с цифры и поэтому недопустимо.
Исправляем. Добавляем локальное имя: "docker_image" "nginx"
И меняем название: "docker_container" "nginx"
Пробуем заново terraform validate:
<img width="760" height="192" alt="image" src="https://github.com/user-attachments/assets/8f5a992e-6609-478f-a9eb-3eff85ac22bd" />
Видим опять ошибку, специально она заложена для задания.
если посмотреть на 30-ую строчку можно заметить слово FAKE которой точно не должно быть. Также неверное имя атрибута 'resulT' вместо 'result'
Исправляем и пробуем заново:
<img width="601" height="79" alt="image" src="https://github.com/user-attachments/assets/068ca6bd-edc7-41f9-94ae-f8923405ca81" />
Всё хорошо запустилось!

Каждый блок должен содержать тип ресурса и его локальное имя!

### 5 Запускам код, показываем выводы.

Вводим в терминале 3 команды: 
```
terraform fmt
terraform validate
terraform plan
```
Проверяем, чтобы все хорошо было и запускаем:
```
terraform apply
```
Исправленный фрагмент кода:

<img width="703" height="344" alt="image" src="https://github.com/user-attachments/assets/27076c00-f634-4ebf-ba2b-4877ca602b53" />

И сам вывод docker ps:

<img width="808" height="357" alt="image" src="https://github.com/user-attachments/assets/67224da0-3084-4dd8-9475-e80a303d150f" />

### 6 Заменяем имя docker-контейнера в блоке кода.

Ключ -auto-approve автоматически подтверждает выполнение изменений и убирает запрос на ввод 'yes'.
Опасность заключается в том, что Terraform сразу применит запланированные изменения. Если в конфигурации допущена ошибка, он может без дополнительного подтверждения изменить или удалить важные ресурсы.
Этот ключ полезен при автоматическом запуске Terraform в скриптах и CI/CD, где нет человеческой руки, который мог бы вручную подтвердить выполнение команды.

Вывод команды docker ps:

<img width="802" height="448" alt="image" src="https://github.com/user-attachments/assets/c8678063-c8e3-4947-8c5d-a9689937f8c9" />

### 7 уничтожение созданных ресурсов с помощью Terraform.
Уничтожаем командой terraform destroy  и жмём yes чтобы подвердить.
Проверяем через terraform state list, docker ps, docker ps -a что всё удалено.
```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> terraform state list
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
Содержимое файла tfstate:

<img width="545" height="242" alt="image" src="https://github.com/user-attachments/assets/732dcf09-6226-436b-a38a-472c85d5fece" />

### 8
Образ nginx:latest не был удалён потому, что у нас в main.tf находится строчка: keep_locally = true, которая запрещает удалять образ и он остается локально.
Из-за этого при выполнении terraform destroy он удалил контейнер и убрал ресурс образа из своего state-файла, но сам образ остался в локальном хранилище Docker.

Проверка того, что образ действительно не удалён командой destroy:

<img width="784" height="111" alt="image" src="https://github.com/user-attachments/assets/90a0addc-c635-465f-9ff8-0c5b4f64fe20" />



## Задача 2*

### 1-2 пункт

Установил полный стек докера, вот компактный вывод:

<img width="677" height="236" alt="image" src="https://github.com/user-attachments/assets/6af16192-f3d9-4ac4-8799-5ae309dadbeb" />

### 3. Исходя из документации [тут](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs) можно увидеть как подключить remote docker contex к нашей рабочей станции.
В разделе Remote Hosts указано, что провайдер может подключаться к удалённому Docker Host по SSH. 
На рабочей станции был настроен SSH-профиль 'yc-vm', содержащий
адрес ВМ, имя пользователя и путь к приватному SSH-ключу.

Для удалённой ВМ был создан Docker context:

```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> ssh yc-vm "docker ps"
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> docker context create yc-remote --docker "host=ssh://yc-vm" --description "Docker на облачной ВМ"
yc-remote
Successfully created context "yc-remote"
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> docker context ls
NAME              DESCRIPTION                               DOCKER ENDPOINT
default           Current DOCKER_HOST based configuration   npipe:////./pipe/docker_engine
desktop-linux *   Docker Desktop                            npipe:////./pipe/dockerDesktopLinuxEngine
yandex-vm         Яндекс Клауд                              ssh://yandex-vm
yc-remote         Docker на облачной ВМ                     ssh://yc-vm
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> docker --context yc-remote ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

В файле main.tf добавил блок:
```
provider "docker" {
  context = "yc-remote"
}
```

### запуск MySQL на удалённой ВМ

Были созданы два отдельных случайных пароля: для пользователя 'root' и для пользователя 'wordpress'.

```
resource "random_password" "mysql_root_password" {
  length      = 20
  special     = false
  min_upper   = 1
  min_lower   = 1
  min_numeric = 1
}

resource "random_password" "mysql_user_password" {
  length      = 16
  special     = false
  min_upper   = 1
  min_lower   = 1
  min_numeric = 1
}
```

Образ и контейнер был описан следующим образом:

```
resource "docker_image" "mysql" {
  name         = "mysql:8"
  keep_locally = true
}

resource "docker_container" "mysql" {
  name  = "mysql"
  image = docker_image.mysql.image_id

  env = [
    "MYSQL_ROOT_PASSWORD=${random_password.mysql_root_password.result}",
    "MYSQL_DATABASE=wordpress",
    "MYSQL_USER=wordpress",
    "MYSQL_PASSWORD=${random_password.mysql_user_password.result}",
    "MYSQL_ROOT_HOST=%"
  ]

  ports {
    internal = 3306
    external = 3306
    ip       = "127.0.0.1"
  }
}
```
Для создания ресурсов используем команду terraform apply.


Результат выполнения:

```
Apply complete! Resources: 4 added, 0 changed, 0 destroyed.
```

Терраформ сам создал пароли, скачал mysql и запустил контейнер.

Проверка запущенного контейнера:

```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> docker --context yc-remote ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                 NAMES
edadc75103f0   b3b90af2a655   "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes   127.0.0.1:3306->3306/tcp, 33060/tcp   mysql
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> 
```

Контейнер работает, всё выполнено на ВМ локально на ip адресе: 127.0.0.1:3306 

Посмотрев логи, можно увидеть, что была создана база данных 'wordpress', пользователь 'wordpress' и пользователю был предоставлен доступ к этой базе. 

Проверяем что mysql работает:

```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> docker --context yc-remote exec mysql sh -c 'mysqladmin ping -uroot -p"$MYSQL_ROOT_PASSWORD"'
mysqladmin: [Warning] Using a password on the command line interface can be insecure.
mysqld is alive
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> 
```

Ответ "mysqld is alive" подтверждает что база жива и работает.

Проверяем что база живёт только локально:
```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> ssh yc-vm "ss -lnt | grep 3306"              
LISTEN 0      4096       127.0.0.1:3306      0.0.0.0:*
```

### 6 Просто через mobaxterm подключился, зашёл внутрь контейнера чтобы посмотреть что в .env записано:

```
root@compute-vm-2-2-20-hdd-1785994098280:~/docker-test# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                                 NAMES
edadc75103f0   b3b90af2a655   "docker-entrypoint.s…"   2 hours ago   Up 2 hours   127.0.0.1:3306->3306/tcp, 33060/tcp   mysql
root@compute-vm-2-2-20-hdd-1785994098280:~/docker-test# docker exec -it mysql sh
sh-5.1# env | grep '^MYSQL_'
MYSQL_MAJOR=8.4
MYSQL_ROOT_PASSWORD=8L***********jZ
MYSQL_PASSWORD=fW***********p
MYSQL_USER=wordpress
MYSQL_VERSION=8.4.11-1.el9
MYSQL_ROOT_HOST=%
MYSQL_DATABASE=wordpress
MYSQL_SHELL_VERSION=8.4.10-1.el9
```

Пароли я скрыл специально звёздочками * 

## Задача 3 * 

Установил tofu: 

```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> tofu -version                                
OpenTofu v1.12.5  
on windows_amd64                                                
+ provider registry.terraform.io/hashicorp/random v3.9.0
+ provider registry.terraform.io/kreuzwerker/docker v4.5.0
```

И вот что показывает:

```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> tofu apply        
╷
│ Error: Inconsistent dependency lock file
│ 
│ The following dependency selections recorded in the lock file are inconsistent with the current
│ configuration:
│   - provider registry.opentofu.org/hashicorp/random: required by this configuration but no version is selected
│   - provider registry.opentofu.org/kreuzwerker/docker: required by this configuration but no version is selected
│ 
│ To update the locked dependency selections to match a changed configuration, run:
│   tofu init -upgrade

PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> tofu init -upgrade

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/random...
- Finding latest version of kreuzwerker/docker...
╷
│ Warning: Dependency lock file entries automatically updated
│ 
│ OpenTofu automatically rewrote some entries in your dependency lock file:
│   - registry.terraform.io/hashicorp/random => registry.opentofu.org/hashicorp/random
│ 
│ The version selections were preserved, but the hashes were not because the OpenTofu project's provider
│ releases are not byte-for-byte identical.
╵

╷
│ Error: Failed to resolve provider packages
│ 
│ Could not resolve provider kreuzwerker/docker: could not connect to registry.opentofu.org: failed to
│ request discovery document: 403 Forbidden
╵

╷
│ Error: Failed to resolve provider packages
│ 
│ Could not resolve provider hashicorp/random: could not connect to registry.opentofu.org: failed to
│ request discovery document: 403 Forbidden
╵
```
Пишет кучу ошибок. Проанализировав ошибки, то, что короткие адреса провайдеров были автоматически преобразованы в адреса Opentofu.
Поэтому в main.tf мы явно указываем адреса на Terraform Registry:

```
terraform {
  required_providers {
    docker = {
      source = "registry.terraform.io/kreuzwerker/docker"
    }

    random = {
      source = "registry.terraform.io/hashicorp/random"
    }
  }

  required_version = "~>1.12.0"
}
```

Также для OpenTofu был создан файл "%APPDATA%\tofu.rc" с настройкой зеркала провайдеров:

```
provider_installation {
  network_mirror {
    url     = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }

  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
```

Дальше план такой: удаляем все старые init и то что мы пытались делать с apply, и запускаем всё заново:


```
Remove-Item -Recurse -Force .\.terraform -ErrorAction SilentlyContinue
Remove-Item -Force .\.terraform.lock.hcl -ErrorAction SilentlyContinue
tofu init
```

Инициализация завершилась успешно:

```
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> tofu validate
Success! The configuration is valid.
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> tofu state list
docker_container.mysql
docker_image.mysql
random_password.mysql_root_password
random_password.mysql_user_password
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> tofu plan
random_password.mysql_root_password: Refreshing state... [id=none]
random_password.mysql_user_password: Refreshing state... [id=none]
docker_image.mysql: Refreshing state... [id=sha256:b3b90af2a6552ae30c266fdb7d5dd55f3afb72404bb78d37fe8a23eb857fd3fbmysql:8]
docker_container.mysql: Refreshing state... [id=edadc75103f0429bb23430aaab64800ea351a51242a80d320e61fa82b7eb0f79]

No changes. Your infrastructure matches the configuration.

OpenTofu has compared your real infrastructure against your configuration and found no differences, so no
changes are needed.
PS C:\Users\Александр\Documents\Project3\ter-homeworks\01\src> tofu apply -auto-approve
random_password.mysql_user_password: Refreshing state... [id=none]
random_password.mysql_root_password: Refreshing state... [id=none]
docker_image.mysql: Refreshing state... [id=sha256:b3b90af2a6552ae30c266fdb7d5dd55f3afb72404bb78d37fe8a23eb857fd3fbmysql:8]
docker_container.mysql: Refreshing state... [id=edadc75103f0429bb23430aaab64800ea351a51242a80d320e61fa82b7eb0f79]

No changes. Your infrastructure matches the configuration.

OpenTofu has compared your real infrastructure against your configuration and found no differences, so no
changes are needed.

Apply complete! Resources: 0 added, 0 changed, 0 destroyed.
```
