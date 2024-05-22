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
