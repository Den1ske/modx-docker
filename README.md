# Modx-docker

# Конфигурация

1. Переименовать .env_template файл в .env
2. Узнать последнюю версию MODx на сайте [MODx.com](https://modx.com/download), затем вписать последнюю версию в параметр MODX_VERSION в .env, также прописать SHA1 hash файла (`$ sha1sum modx.zip`)
3. Отредактировать default настройки
   
#  Установка
- `docker-compose up -d`

# Обновление 
- Запустить update.sh 

Сборка основана на базе [https://github.com/modxcms/docker-modx](https://github.com/modxcms/docker-modx)
