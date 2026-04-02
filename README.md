# redir-tools
Samba AD Default Container Redirection Tools

## Общий вид
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
## Использование:

```
[root@dc tmp]#  redircmp "OU=MyComputers,OU=Corporate"
[2026-04-02 20:27:26] === Начало переназначения контейнера для учетных записей компьютеров ===
[2026-04-02 20:27:27] Параметры:
[2026-04-02 20:27:27]   OU путь: OU=MyComputers,OU=Corporate
[2026-04-02 20:27:27]   Корень домена: DC=test,DC=alt
[2026-04-02 20:27:27]   Полный DN OU: OU=MyComputers,OU=Corporate,DC=test,DC=alt
[2026-04-02 20:27:27] ✗ Ошибка: подразделение OU=MyComputers,OU=Corporate не существует. Используйте --create.

[root@dc tmp]#  redircmp "OU=MyComputers,OU=Corporate" --create
[2026-04-02 20:27:35] === Начало переназначения контейнера для учетных записей компьютеров ===
[2026-04-02 20:27:35] Параметры:
[2026-04-02 20:27:35]   OU путь: OU=MyComputers,OU=Corporate
[2026-04-02 20:27:35]   Корень домена: DC=test,DC=alt
[2026-04-02 20:27:35]   Полный DN OU: OU=MyComputers,OU=Corporate,DC=test,DC=alt
[2026-04-02 20:27:35] Подразделение OU=MyComputers,OU=Corporate не существует, создаем...
[2026-04-02 20:27:35]   Создание OU: OU=Corporate
[2026-04-02 20:27:35]   ✓ OU создан: OU=Corporate
[2026-04-02 20:27:35]   Создание OU: OU=MyComputers,OU=Corporate
[2026-04-02 20:27:35]   ✓ OU создан: OU=MyComputers,OU=Corporate
[2026-04-02 20:27:36] ✓ Бэкап (формат ldbmodify) создан: /var/lib/samba/backup/wellKnownObjects_restore_20260402_202735.ldif
[2026-04-02 20:27:36] Применение изменений в базе данных...
[2026-04-02 20:27:36] ✓ Успешно! Контейнер переназначен.
[2026-04-02 20:27:36] Для ОТКАТА выполните: ldbmodify -H "/var/lib/samba/private/sam.ldb" /var/lib/samba/backup/wellKnownObjects_restore_20260402_202735.ldif

[root@dc tmp]# ldapsearch -LL -Y GSSAPI -H ldap://dc.test.alt -b dc=test,dc=alt 'wellKnownObjects=*' wellKnownObjects -o ldif-wrap=no  | grep AA312825768811D1ADED00C04FD8D5CD
SASL/GSSAPI authentication started
SASL username: Administrator@TEST.ALT
SASL SSF: 256
SASL data security layer installed.
wellKnownObjects: B:32:AA312825768811D1ADED00C04FD8D5CD:OU=MyComputers,OU=Corporate,DC=test,DC=alt

[root@dc tmp]# ldbmodify -H "/var/lib/samba/private/sam.ldb" /var/lib/samba/backup/wellKnownObjects_restore_20260402_202735.ldif
Modified 1 records successfully

[root@dc tmp]#  ldapsearch -LL -Y GSSAPI -H ldap://dc.test.alt -b dc=test,dc=alt 'wellKnownObjects=*' wellKnownObjects -o ldif-wrap=no  | grep AA312825768811D1ADED00C04FD8D5CD
SASL/GSSAPI authentication started
SASL username: Administrator@TEST.ALT
SASL SSF: 256
SASL data security layer installed.
wellKnownObjects: B:32:AA312825768811D1ADED00C04FD8D5CD:CN=Computers,DC=test,DC=alt
```
