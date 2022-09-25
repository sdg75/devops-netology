# Домашнее задание к занятию "8.1. Введение в Ansible"

## Основная часть.

1. Попробуйте запустить playbook на окружении из test.yml,
зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.

Ответ: 12

2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение
и поменяйте его на 'all default fact'.

Ответ:
```
vagrant@vagrant:~/netology/devops-netology/ansible/1$ cat group_vars/all/examp.yml
---
  some_fact: "all default fact"
```

3. Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.

Выполнено.

4. Проведите запуск playbook на окружении из prod.yml.
Зафиксируйте полученные значения some_fact для каждого из managed host.

Ответ:
```
vagrant@vagrant:~/netology/devops-netology/ansible/1$ ansible-playbook -i inventory/prod.yml site.yml 

PLAY [Print os facts] **************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] ********************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP *************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
5. Добавьте факты в group_vars каждой из групп хостов так,
чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.

Ответ:
```
vagrant@vagrant:~/netology/devops-netology/ansible/1$ cat group_vars/deb/examp.yml ;echo ""
---
  some_fact: "deb default fact"
vagrant@vagrant:~/netology/devops-netology/ansible/1$ cat group_vars/el/examp.yml ;echo ""
---
  some_fact: "el default fact"
```
6. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.

Ответ:
```
vagrant@vagrant:~/netology/devops-netology/ansible/1$ ansible-playbook -i inventory/prod.yml site.yml 

PLAY [Print os facts] **************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] ********************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.

Ответ:
```
vagrant@vagrant:~/netology/devops-netology/ansible/1$ ansible-vault encrypt group_vars/deb/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
vagrant@vagrant:~/netology/devops-netology/ansible/1$ ansible-vault encrypt group_vars/el/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
```
8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль.
Убедитесь в работоспособности.

Ответ:
```
vagrant@vagrant:~/netology/devops-netology/ansible/1$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password: 

PLAY [Print os facts] **************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ********************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

9. Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.

Ответ:

нужен "local"

10. В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.

Ответ:
```
vagrant@vagrant:~/netology/devops-netology/ansible/1$ cat inventory/prod.yml ; echo ""
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_connection: local
```
11. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль.
Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.

Ответ:

Запустил без создания отдельного group_vars -> получил для local из all
```html
vagrant@vagrant:~/netology/devops-netology/ansible/1$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password: 

PLAY [Print os facts] **************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [localhost]
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] ********************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```


Добавил отдельный group-vars для local
```
vagrant@vagrant:~/netology/devops-netology/ansible/1$ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password: 

PLAY [Print os facts] **************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
ok: [localhost]
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] ********************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [localhost] => {
    "msg": "local default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

12. Заполните README.md ответами на вопросы. Сделайте git push в ветку master.
В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md

Ответ: 