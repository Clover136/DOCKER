# DOCKER  

## Задание 1  
__Цель задания:__  
  1. Установить виртуальную машину на ALT Linux  
  2. Установить Docker  
  3. Запустить контейнер "Hello World"  
  4. Удалить контейнер  

### Выполнение задания
Для начала обновляем пакеты:  
```
apt-get update
```
Затем устанавливаем Docker:  
```
apt-get install -y docker-engine
```
Запускаем Docker:  
```
systemctl enbale --now docker
```
И скачиваем нужный контейнер:  
```
docker pull hello-world
```
Смотрим список контейнеров:    
```
docker images
```
Затем запускаем 'Hello-Wolrd':  
```
docker run hello-world
```
Останавливаем контейнер `Hello-World`:  
```
docker rm -v *ID контейнера*
```
Удаляем контейнер:  
```
docker rmi *ID контейнера*
```

---

# Nginx в Docker  

## Задание 2  
__Цель задания:__  
  1. На базе образа NGINX последней версии создать катомный образ с измененным файлом index.html. В файле должен быть заготовок H1 с вашей ФИО  
  2. Запустить контейнер на порту 80  

### Выполнение задания  
Обновляем пакеты:  
```
apt-get update
```
Далее загружаем `Nginx` контейнер в Docker:  
```
docker pull nginx
```
Запускаем контейнер на 80 порте:  
```
docker run --rm -d --anme nginx -v /data/app:/var/www/html -p 0.0.0.0:80:80 nginx
```
Проверяем запущенные контейнеры:  
```
docker ps
```
