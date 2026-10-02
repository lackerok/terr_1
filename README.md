# Задание 0
<img width="918" height="682" alt="image" src="https://github.com/user-attachments/assets/47b46f08-2723-4e94-ab50-52e3ccf35c55" />
<img width="688" height="99" alt="image" src="https://github.com/user-attachments/assets/9913130b-7bc3-47a9-acf6-72a16ed3ddd5" />

# Задание 1

Согласно .gitignore, личную и секретную информацию разрешено сохранять в файле personal.auto.tfvars. Этот файл добавлен в исключения git, поэтому не попадёт в репозиторий, а суффикс .auto.tfvars позволяет терраформу загружать секреты автоматически без дополнительных флагов

<img width="470" height="348" alt="image" src="https://github.com/user-attachments/assets/6ae3f2a0-72b6-44d3-9a43-9c02367f4e03" />
RtUEOOB4xPUQVCwF - пароль
Что исправлено: добавлено имя "nginx" у docker_image, имя контейнера "1nginx" изменено на "nginx", убран фейковый ресурс FAKE, регистр resulT заменён на строчный result)
<img width="872" height="75" alt="image" src="https://github.com/user-attachments/assets/c938c181-42ae-4bdd-8e6b-50e48050833d" />

<img width="872" height="91" alt="image" src="https://github.com/user-attachments/assets/f372f582-0cdd-4748-bbf4-1c2776024b4e" />

опасность: флаг полностью пропускает шаг ручного подтверждения изменений. При изменении ключевых параметров Terraform не может обновить ресурс на лету и сначала полностью уничтожает его, а затем создает заново. На проде случайный запуск с -auto-approve может безвозвратно стереть базу данных или остановить работающие сервисы пользователей
он нужен для автоматизации в CI/CD пайплайнах, где скрипты деплоя исполняются фоновыми роботами-раннерами без участия человека

<img width="918" height="682" alt="image" src="https://github.com/user-attachments/assets/93e7b623-1397-4ee2-a191-2496933173d8" />

nginx не удалился из-за "keep_locally = true". Именно это значение указывает Terraform сохранить образ на диске хоста при уничтожении инфраструктуры

<img width="1069" height="408" alt="image" src="https://github.com/user-attachments/assets/ec95eb3a-2d2f-450b-8151-152206d7396f" />


Код:

```
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
    }
  }
  required_version = "~>1.12.0" /*Многострочный комментарий.
 Требуемая версия terraform */
}
provider "docker" {}

#однострочный комментарий

resource "random_password" "random_string" {
  length      = 16
  special     = false
  min_upper   = 1
  min_lower   = 1
  min_numeric = 1
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id

  name  = "hello_world"
  ports {
    internal = 80
    external = 8000
  }
}
```
