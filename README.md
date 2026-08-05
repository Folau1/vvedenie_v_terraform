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
