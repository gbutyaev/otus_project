# Crawler CI/CD

## План
1. Подготовить образ c помощью Packer для поднятия ВМ в Terraform ( все на основе ubuntu:20.04)
```
packer build image_ubuntu.json
```   
![Скриншот 19-05-2022 185756](https://user-images.githubusercontent.com/76098648/169347329-6cb7b97e-b228-4fda-ac38-12b6f3a032e7.jpg)




## 1. Подготовка инфрасруктуры  
2. Для нашено проекта нам понадобятся 3 виртульный машины:
- Для GitLabCI ```project-gitlab```
- Для тестирования и сборки образов ```project-application```
- Для деплоя пирложения ```deploy-vm```  

```
terraform plan   

terraform apply -auto-approve   
```

![Скриншот 19-05-2022 191140](https://user-images.githubusercontent.com/76098648/169347885-c6026fb9-8aac-4ba3-8501-04d7b4107379.jpg)



### ВМ для Gitlab-CI   
 На ВМ `project-gitlab` разворачиваем `GitLab Omnibus` с помощью docker-compose

``` 
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'http://<YOUR-VM-IP>'
  ports:
    - '80:80'
    - '443:443'
    - '2222:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'
```
```
docker-compose up -d
```
```
**http://51.250.9.77/** 
```

### ВМ для Тестирования и билдов образов
Тестирование и сборка образов будет происходить в контейнерах. Добавив на ВМ ```project-application``` Dockerfile для сборки образов, файлы окружения, установим вокер и докер раннер с помощью готовых ansible ролей:   

```
ansible-galaxy install geerlingguy.docker
ansible-galaxy install riemers.gitlab-runner
```
```
ansible-playbook project -i inventory
```

### ВМ для деплоя приложения

В нашем проекте сборка и деплой разделены. Поэтому добавляем гитлаб-раннер на ВМ ```deploy-vm``, Dockerfile для сборки и файлы окру

## Тестирование, Билд и Деплой

Вся логика работы приложения заключена в GitLabCI и разделена на три стадии
1. Тестирование. Докер раннер выполняет ручные тесты из склоннированного репозитория на ВМ `project-application`
2. Билд. Докер раннер собирает образы нашего приложения - `ui` и `backend` и отправляет их `docker-hub`
3. Деплой. Гитлаб ( shell runner) вы




 


 
