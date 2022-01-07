1. Переменная c будет иметь значение a+b, т.к. в данном случае a и b - это не значения переменных, а просто символы.

Переменная d будет иметь значение 1+2, т.к. здесь есть ссылка на значение переменных a и b, но отсутствует приведение к числовому типу данных, и + в данном случае не операция сложения, а просто символ.

Переменная e будет иметь значение 3, т.к значения переменных приведены к числовому типу данных и применена опарация сложения.


2. В скрипте пропущена закрывающая круглая скобка в строке оператора while и отсутствует условие выхода из бесконечного цикла, например, седьмой строкой можно вставить else break.


3. Скрипт:
```
vagrant@vagrant:~$ cat ./sh.sh
#!/usr/bin/env bash
hosts=(192.168.0.1 173.194.222.113 87.250.250.24)
for i in {1..5}
do
date >>hosts.log
    for h in ${hosts[@]}
    do
        curl -Is --connect-timeout 3 $h:80 >/dev/null
        if (($?==0))
        then
         status=started
        else
         status=error
        fi
        echo "    check" $h status=$status >>hosts.log
    done
done
```
4. Скрипт:
```
vagrant@vagrant:~$ cat ./sh.sh
#!/usr/bin/env bash
hosts=(192.168.0.1 173.194.222.113 87.250.250.24)
while ((1==1))
do
    for h in ${hosts[@]}
    do
        curl -Is --connect-timeout 3 $h:80 >/dev/null
        if (($?!=0))
        then
            echo $h status=error >>hosts.log
            exit
        fi
   done
done
```