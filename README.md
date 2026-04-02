# redir-tools
Samba AD Default Container Redirection Tools

```
[root@dc tmp]# redircmp 
Использование: redircmp <OU-path> [--ldb <path>] [--create] [--help]

Описание:
    Переназначает контейнер для новых учетных записей компьютеров в Samba AD.
    Создает файл бэкапа, пригодный для восстановления через ldbmodify.

Параметры:
    OU-path                 Путь к подразделению без корня домена
    --ldb <path>            Путь к базе данных sam.ldb (по умолчанию: /var/lib/samba/private/sam.ldb)
    --create                Создать указанные OU, если они не существуют
    --help                  Показать эту справку
```

```
[root@dc tmp]# redirusr 
Использование: redirusr <OU-path> [--ldb <path>] [--create] [--help]

Описание:
    Переназначает контейнер для новых учетных записей ПОЛЬЗОВАТЕЛЕЙ в Samba AD.
    Создает файл бэкапа, пригодный для восстановления через ldbmodify.

Параметры:
    OU-path                 Путь к подразделению без корня домена
                            Пример: OU=Staff,OU=Corporate
    --ldb <path>            Путь к базе данных sam.ldb (по умолчанию: /var/lib/samba/private/sam.ldb)
    --create                Создать указанные OU, если они не существуют
    --help                  Показать эту справку

Примеры:
    redirusr "OU=Users,OU=Office" --create
```
