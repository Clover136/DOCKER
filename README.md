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
  Запустить контейнер "NGINX" с мапингом порта 80. Сервер NGINX должен быть доступен с внешних устройств  

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

---

## Задание 3  
__Цель задания:__  
  1. На базе образа NGINX последней версии создать катомный образ с измененным файлом index.html. В файле должен быть заготовок h1 с вашей ФИО  
  2. Запустить контейнер на порту 80

### Выполнение задания  
Сначало запускаем интерактивную оболочку в контейнере Docker с помощью docker exec с флагами -i и -t:  
```
docker exec -it nginx /bin/bash
```
Пока мы можем только просматривать файлы, поэтому чтобы начать их редактировать нужно будет заново скачать программу, которой мы пользуемся для редака файлов, а именно `nano` или `vim`:  
```
apt-get update
apt-get install -y nano
```
Далее необходимо открыть файл index.html:  
```
nano /usr/share/nginx/html/index.html
```
Далее меняем по желанию.  
![Снимок](https://github.com/Clover136/DOCKER/assets/148867684/2f5b232c-7555-4227-9214-cb6cc9fc0c46)


---

## Задание 4  
__Цель задания:__  
  1. Создать образ для приложения раюотающего на фреймворке FastApi  
  2. Запустить контейнер на порту 80  

### Выполнение задания
Для начала обратимся к Python для создания нового окружения, которое мы назовём `docker`:  
```
pyton3 -m venv docker
```
Активируем его:  
```
source docker/bin/activate
```
Далее создаем новую директорию, под названием `fastapi` и заходим в неё:  
```
mkdir /fastapi
cd /fastapi
```
В этой директории создаем файл `requirements.txt`, куда мы запишем зависимости:  
```
nano requirements.txt
```
И заполняем его:  
```
fastapi>=0.68.0,<0.69.0
pydantic>=1.8.0,<2.0.0
uvicorn>=0.15.0,<0.16.0
```
И устанавливаем зависимости при помощи `pip`:  
```
pip install -r requirements.txt
```
В той же директории создаем папку `app`, переходим в нее и создаем 2 файла: `__init__.py` и `main.py`.(Файл `__init__.py` оставляем пустым):  
```
mkdir /fastapi/app
cd /fastapi/app
nano /fastapi/app/__init__.py (сохраняем и выходим сочетаниями клавиш ctrl+s, ctrl+x)
nano /fastapi/app/main.py
```
Заполняем файл `main.py`:  
```
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```
Далее в директории `/fastapi` создаём файл `Dockerfile`:  
```
nano /fastapi/Dockerfile
```
Заполняем файл ``Dockerfile`. (На всякий случай между строками можно установить решетки):  
```
FROM python:3.9
WORKDIR /code 
COPY ./requirements.txt /code/requirements.txt 
RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt 
COPY ./app /code/app 
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
```
Не выходя из директории `/fastapi`, создаём в ней образ приложения `FastAPI`:  
```
docker build -t fastapi /fastapi
```
После прошедшей загрузки запускаем сам контейнер:  
```
docker run -d --name fastapi -p 80:80 fastapi
```
Чтобы проверить работу контейнера мы должны перейти по ссылке: http://IP_адрес_вашей_машины. Там мы увивдим текст на подобии:  
```
{"item_id": 5, "q": "somequery"}
```
Также нужно ввести и следующую ссылку:  
```
http://IP_адрес_вашей_машины/docs
```

---










