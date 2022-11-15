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
7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.
```
➜  playbook git:(master) ✗ cat group_vars/el/examp.yml                  
$ANSIBLE_VAULT;1.1;AES256
63633735366131303236393436636266366566313862356339323231626235653938363139333332
3163376535633832343562663032313462323661393836310a346238633930386232613764386661
62373265333734646363306263636135396339316531613162363163633634386439633830653963
3761653262386435340a373736633962333364396536303532393333656162326637386536353630
62613434663436366433643731306266663835646234353264353431633936643337
➜  playbook git:(master) ✗ ansible-vault encrypt group_vars/deb/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
➜  playbook git:(master) ✗ cat group_vars/deb/examp.yml
$ANSIBLE_VAULT;1.1;AES256
62626331343636623133666565656532326231666161366539626665366163323362653532336539
6133386265656636343264656534306330326665303037620a353964393930633463663261373965
39396337356165376630646161623938646461663762323933383662376337656164326538303139
6163663233333664310a396534306235316361376661326234386166353264303562313536663365
39316431343065643462353732336530363964363138626463656431643066313333
➜  playbook git:(master) ✗ 
```
___________________________________________________
8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.
```
➜  playbook git:(master) ✗ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-password
Vault password: 
TASK [Print fact] ****************************************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
```
_____________________________________________________
9. Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.
```
ansible.netcommon.grpc         Provides a persistent connection using the gRPC protocol                                                                     
ansible.netcommon.httpapi      Use httpapi to run command on network appliances                                                                             
ansible.netcommon.libssh       Run tasks using libssh for ssh connection                                                                                    
ansible.netcommon.napalm       Provides persistent connection using NAPALM                                                                                  
ansible.netcommon.netconf      Provides a persistent connection using the netconf protocol                                                                  
ansible.netcommon.network_cli  Use network_cli to run command on network appliances                                                                         
ansible.netcommon.persistent   Use a persistent unix socket for connection                                                                                  
community.aws.aws_ssm          execute via AWS Systems Manager                                                                                              
community.docker.docker        Run tasks in docker containers                                                                                               
community.docker.docker_api    Run tasks in docker containers                                                                                               
community.docker.nsenter       execute on host running controller container                                                                                 
community.general.chroot       Interact with local chroot                                                                                                   
community.general.funcd        Use funcd to connect to target                                                                                               
community.general.iocage       Run tasks in iocage jails                                                                                                    
community.general.jail         Run tasks in jails                                                                                                           
community.general.lxc          Run tasks in lxc containers via lxc python library                                                                           
community.general.lxd          Run tasks in lxc containers via lxc CLI                                                                                      
community.general.qubes        Interact with an existing QubesOS AppVM                                                                                      
community.general.saltstack    Allow ansible to piggyback on salt minions                                                                                   
community.general.zone         Run tasks in a zone instance                                                                                                 
community.libvirt.libvirt_lxc  Run tasks in lxc containers via libvirt                                                                                      
community.libvirt.libvirt_qemu Run tasks on libvirt/qemu virtual machines                                                                                   
community.okd.oc               Execute tasks in pods running on OpenShift                                                                                   
community.vmware.vmware_tools  Execute tasks inside a VM via VMware Tools                                                                                   
community.zabbix.httpapi       Use httpapi to run command on network appliances                                                                             
containers.podman.buildah      Interact with an existing buildah container                                                                                  
containers.podman.podman       Interact with an existing podman container                                                                                   
kubernetes.core.kubectl        Execute tasks in pods running on Kubernetes                                                                                  
local                          execute on controller                                                                                                        
paramiko_ssh                   Run tasks via python ssh (paramiko)                                                                                          
psrp                           Run tasks over Microsoft PowerShell Remoting Protocol                                                                        
ssh                            connect via SSH client binary                                                                                                
winrm                          Run tasks over Microsoft's WinRM                                                                                             
(END)
```
_________________________________________________
10. В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.
```
➜  playbook git:(master) ✗ cat inventory/prod.yml 
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  loc:
    hosts:
      localhost:
        ansible_connection: local
```
_________________________________________________
11. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.
```
➜  playbook git:(master) ✗ ansible-playbook -i inventory/prod.yml site.yml --ask-vault-password
Vault password: 
TASK [Print fact] ****************************************************************************************************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}
ok: [localhost] => {
    "msg": "all default fact"
}
```
__________________________________________________
 
