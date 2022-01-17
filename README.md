# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"


## Обязательная задача 1
Мы выгрузили JSON, который получили через API запрос к нашему сервису:
```json
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            }
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
```
  Нужно найти и исправить все ошибки, которые допускает наш сервис

В девятой строке не хватает кавычек:
```json
"ip" : "71.78.22.43"
```

## Обязательная задача 2
В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: `{ "имя сервиса" : "его IP"}`. Формат записи YAML по одному сервису: `- имя сервиса: его IP`. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

### Ваш скрипт:
```python
???
```

### Вывод скрипта при запуске при тестировании:
```
*** IP checking started ***
{'drive.google.com': '74.125.205.194', 'mail.google.com': '216.58.210.165', 'google.com': '216.58.210.174'}
[ERROR] google.com IP mistmatch: 216.58.210.142 216.58.210.174
[ERROR] mail.google.com IP mistmatch: 216.58.210.133 216.58.210.165
[ERROR] drive.google.com IP mistmatch: 173.194.222.194 173.194.220.194
```

### json-файл(ы), который(е) записал ваш скрипт:
```json
vagrant@vagrant:~$ cat ./google.com.json
{"google.com": "216.58.210.174"}
```
```json
vagrant@vagrant:~$ cat ./mail.google.com.json
{"mail.google.com": "216.58.210.165"}
```
```json
vagrant@vagrant:~$ cat ./drive.google.com.json
{"drive.google.com": "173.194.220.194"}
```

### yml-файл(ы), который(е) записал ваш скрипт:
```yaml
vagrant@vagrant:~$ cat ./google.com.yaml
---
- google.com: 216.58.210.174
```
```yaml
vagrant@vagrant:~$ cat ./mail.google.com.yaml
---
- mail.google.com: 216.58.210.165
```
```yaml
vagrant@vagrant:~$ cat ./drive.google.com.yaml
---
- drive.google.com: 173.194.220.194
```