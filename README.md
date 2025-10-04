## «Оркестрация группой Docker контейнеров на примере Docker Compose»

## **Задача 1: Создание кастомного образа nginx**

### Выполненные действия:
1. Скачан образ nginx:1.21.1
2. Создан Dockerfile с заменой индексной страницы
3. Собран образ с тегом 1.0.0
4. Загружен в Docker Hub репозиторий

### Результат:
Образ доступен по ссылке: https://hub.docker.com/r/sermsn/custom_nginx

![Задача 1](images\Docker_z1.png)

---

## **Задача 2: Запуск и управление контейнером**

### Выполненные действия:
1. Запущен контейнер с именем `MalyavkoSN-custom-nginx-t2`
2. Контейнер переименован в `custom-nginx-t2`
3. Выполнена комплексная команда проверки
4. Проверена доступность через curl

### Команды:
```bash
docker run -d --name MalyavkoSN-custom-nginx-t2 -p 127.0.0.1:8080:80 sermsn/custom_nginx:1.0.0
docker rename MalyavkoSN-custom-nginx-t2 custom-nginx-t2
curl http://127.0.0.1:8080
```

![Задача 2](images\Docker_z2_1.png)

![Задача 2](images\Docker_z2_2.png)
---

## **Задача 3: Работа с контейнером и диагностика**

### Выполненные действия:
1. Подключение к потокам контейнера через `docker attach`
2. Анализ остановки контейнера после Ctrl+C
3. Редактирование конфигурации nginx (порт 80 → 81)
4. Диагностика проблемы с пробросом портов
5. Удаление контейнера без остановки

### Проблема и решение:
Контейнер слушал порт 81 внутри, но наружу был опубликован порт 80, что вызывало несоответствие.

![Задача 3](images\Docker_z3_1.png)
![Задача 3](images\Docker_z3_2.png)
![Задача 3](images\Docker_z3_3.png)
![Задача 3](images\Docker_z3_4.png)
---

## **Задача 4: Работа с volumes**

### Выполненные действия:
1. Запущены контейнеры CentOS и Debian с общим volume
2. Создан файл из контейнера CentOS
3. Создан файл на хостовой машине
4. Проверена синхронизация файлов в контейнере Debian

### Команды:
```bash
docker run -d -v $(pwd):/data --name centos-container centos:7 tail -f /dev/null
docker run -d -v $(pwd):/data --name debian-container debian:11 tail -f /dev/null
docker exec centos-container touch /data/from-centos.txt
touch from-host.txt
docker exec debian-container ls -la /data
```

![Задача 4](images\Docker_z4.png)

---

## **Задача 5: Работа с Docker Compose и Portainer**

### Выполненные действия:
1. Создана структура Docker Compose файлов
2. Настроен Portainer с локальным registry
3. Загружен образ в локальное registry
4. Развёрнут stack через Web Editor в Portainer
5. Получен скриншот Inspect контейнера


### Compose файл:
```yaml
version: "3.8"
services:
  portainer:
    image: portainer/portainer-ce:latest
    command: -H unix:///var/run/docker.sock --no-analytics
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data
    restart: unless-stopped

  registry:
    image: registry:2
    ports:
      - "5000:5000"
```

![Задача 5](images\Docker_z5_1.png)
![Задача 5](images\Docker_z5_2.png)
![Задача 5](images\Docker_z5_3.png)
---

## **Итоги**

✅ Все задачи выполнены успешно  
✅ Освоена работа с Docker образами и контейнерами  
✅ Освоена оркестрация с Docker Compose  
✅ Освоена работа с Portainer для управления Docker  


**Образ в Docker Hub:** https://hub.docker.com/r/sermsn/custom_nginx  
**Локальное окружение:** Portainer на http://127.0.0.1:9000  
**Тестовое приложение:** http://127.0.0.1:9090