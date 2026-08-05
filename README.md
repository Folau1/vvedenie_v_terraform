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

