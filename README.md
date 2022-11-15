# Подготовка к выполнению
______________________________________
1. Установите ansible версии 2.10 или выше.
2. Создайте свой собственный публичный репозиторий на github с произвольным именем.
3. Скачайте playbook из репозитория с домашним заданием и перенесите его в свой репозиторий.<br/>
__________________________________________
# Основная часть

_________________________________________
1. Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.<br>
```
➜  playbook git:(master) ansible-playbook -i inventory/test.yml site.yml

TASK [Print fact] *******************************************************************
ok: [localhost] => {
    "msg": 12
}
```
__________________________________________
2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.
```
TASK [Print fact] ******************************************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}
```
___________________________________________
3. Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.
```
➜  playbook git:(master) ✗ docker ps
CONTAINER ID   IMAGE      COMMAND          CREATED          STATUS          PORTS     NAMES
ae0ff81b81d7   ubuntu     "sleep 666666"   9 seconds ago    Up 6 seconds              ubuntu
44d83dbf1c19   centos:7   "sleep 666666"   52 seconds ago   Up 49 seconds             cenros7
```
_________________________________________________
4. Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host
```
➜  playbook git:(master) ✗ ansible-playbook -i inventory/prod.yml site.yml
TASK [Print fact] ****************************************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}
```
______________________________________________
5. Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.
```
➜  playbook git:(master) ✗ ansible-playbook -i inventory/prod.yml site.yml
TASK [Print fact] ****************************************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
```
________________________________________________
6. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.
```
➜  playbook git:(master) ✗ ansible-playbook -i inventory/prod.yml site.yml
TASK [Print fact] ****************************************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
```
________________________________________________
 
