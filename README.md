# smallbigdata-container
## Проект по дисциплине Small Big Data
Проект включает в себя *Docker Image*, состоящий из ML-модели на `scikit-learn`, `Airflow` оркестрации, анализе данных при помощи `numpy`, `scipy`,
`pandas`, `matplotlib`, `seaborn`. Визуализация представлена в инстументе `Microsoft PowerBI`. \
Данные хранятся в базе данных `PostgreSQL` на сервисе [hoster.ru](hoster.ru).
### Роли в проекте:
- Руководитель проекта: Георгий Новожилов
- ML-модель: Кирилл Андронов
- Анализ данных: Никита Цап, Полина Моисеева
- BI-проектирование: Георгий Корнилов, Филипп Евменьев
- ETL: Иван Спиридонов
### Системная архитектура
![image](https://user-images.githubusercontent.com/24700915/233774688-0e906e7f-1f34-42d1-afea-309eb1b63ab6.png)

Презентация проекта доступна [<ins>по ссылке</ins>](https://docs.google.com/presentation/d/1aRgvSuflUph7w6Fd24xhOIt93v2giMF9/edit?usp=share_link&ouid=115651976754306796676&rtpof=true&sd=true)


### Рекомендуемые характеристики
- RAM: 4Гб (16Гб+ для первичного обучения модели)
- VRAM: 2Гб
- Место на жёстком диске: 2Гб
- Установленный Docker
- Linux Ubuntu 22.04 / LTS

## Шаги запуска
### 1. Скачаем docker image [<ins>по ссылке</ins>](https://drive.google.com/file/d/1xBgNpqfOBnvf_1ol5l3oxcQ0kOSnJz5v/view?usp=share_link)
Вес архива составляет 983Мб, что превышает лимит допустимой загрузки в github, поэтому архив с образом был выгружен в Google Disk.
### 2. Распакуем архив
Распакуем архив `smallbigdata-container.tar.gz` в `smallbigdata-container.tar` для дальнейшей загрузки в docker.
### 3. Загрузим docker image в docker
```bash
docker load < smallbigdata-container.tar
```
### 4. Запустим контейнер
```bash
docker run -d -p 8080:8080 smallbigdata-container
```
После запуска пропишем команду и узнаем название запущенного контейнера
```bash
docker container ls
```
В колонке `NAMES` имя контейнера `smallbigdata-container` - `crazy_jackson`:
![image](https://user-images.githubusercontent.com/24700915/233772686-20b43ab2-e7f1-455e-9335-360fa11aac4b.png)
### 5. Узнаем пароль к Apache Airflow
Пропишем bash команду:
```bash
docker exec -it container_name /bin/bash
```
(в моём случае `container_name = crazy_jackson`) <p>
Откроем текстовый файл с паролем к *standalone* версии Apache Airflow:
```bash
cat standalone_admin_password.txt
```
### 6. Откроем сервис Apacke Airflow
Перейдём в [localhost:8080](http://localhost:8080/) и вставим полученный пароль из пункта 5.
### 7. Отфильтруем Airflow DAGS
Изначально, в `Apache Airflow` включены Example DAGS. Для того, чтобы отображались DAGS из нашего проекта, отфильтруем их по тегам `fine_tuning` и `prediction`
![image](https://user-images.githubusercontent.com/24700915/233772907-08f7c771-4e4f-40c8-bdff-b762e75f982b.png)
### 8. Готово!

### Общая структура контейнера:
```
├── dags
│   ├── fine_tuning.py
│   ├── load_model.py
├── src
│   ├── predict.csv
│   ├── test.csv
│   ├── test_tuning.csv
├── params
│   ├── params.yaml
│   ├── requirements.txt
├── model
│   ├── log_tuning.txt
│   ├── model.pkl
│   ├── model_ft.pkl
│   ├── pred_log.txt
├── airflow.cfg
├── airflow.db
├── airflow-webserver.pid
├── requirements.txt
├── standalone_admin_password.txt
└── webserver_config.py
```
