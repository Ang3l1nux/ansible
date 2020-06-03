<img style="width: 100%; height: 300px;" src="img/head.png">

# Installing
> $ pip install ansible <br />
> sudo yum install ansible <br />
> sudo apt-get install ansible <br />

Whats diference between pip install and manager package?

The most common and preferrd way of instalation 

<img style="width: 20%; height: 200px; align=right" src="img/logo2.png">

# Caracteristicas 
<ul>
    <li class="itens">Ansible usa yaml</li>
    <li class="itens">multiplataforma</li>
    <li class="itens">Playbook</li>
    <li class="itens">Modulos</li>          
    <li class="itens">Roles</li>
    <li class="itens">AD Hoc commands</li>
    <li class="itens">Inventory</li>
</ul>

<h3 style="color: red; "><ins>Inventory</ins></h3>
<p>É uma coleção de hosts (nodes) e pode trabalhar com hosts e grupos (static ou dynamic</p>

<p>Static Inventory</p>

```
10.42.0.2
10.43.12.120
10.42.10.20
10.43.40.9
```
<br />

``` 
[control]
control ansible_host=10.42.0.2

[web]
node-1 ansible_host=10.42.0.6
node-2 ansible_host=10.42.0.7
node-2 ansible_host=10.42.0.8

[haproxy]
haproxy ansible_host=10.42.0.100

[all:vars]
ansible_user=vagrant
ansible_ssh_private_key_file=~/.vagrant.d/insecure_private_key
```

<h3 style="color: red; "><ins>Files</ins></h3>
<ul>
    <li class="itens">group_vars</li>
    <li class="itens">hosts</li>
    <li class="itens">site.yml</li>
    <li class="itens">ansible.cfg</li>
</ul>

<h3 style="color: red"><ins>AD Hoc commands</ins></h3>

> ansible all -m ping <br />
> ansible web -m commad -a "uptime" <br />
> ansible localhost -m setup <br />

<p>Teste</p>

> ansible all -i hosts -u vagrant -m ping <br /> 
> ansible all -i hosts -u vagrant -m setup <br />
> ansible webserver -i hosts -u vagrant -m yum -a "name=httpd state=present" -b <br />

<h3 style="color: blue"><ins>HandsOn AD Hoc commands</ins></h3>

all=todos os hostsa -i=quem é o arquivo de hosts -u=usuario -m modulo.  
o modulo "ping" apenas faz um ping nos hosts para testar a comunicacao  
o modulo "setup" faz um ansible_facts, coleta informações dos servers, util para condicionais  
o modulo "yum" instala pacotes no redhat e derivados  
-a=arguments que no caso é name=o nome do pacote, state=? [present], [absent] e o -b para utilizar sudo  

# Playbook and Variables:

<h3 style="color: blue"><ins>Variables Precedence (ordem de execução das variaveis) sixteen</ins></h3>
  
1- Extra vars  
2- Task vars (only for the task)  
3- Block vars (only for tasks in the block)  
4- Role and include vars  
5- Play vars_files  
6- Play vars_prompt  
7- Play vars  
8- set_facts  
9- registered vars  
10- host facts  
11- Playbook host_vars   
12- Playbook group_vars    
13- Inventory host_vars   
14- Inventory group_vars   
15- Inventory vars  
16- Role defaults  


<h3 style="color: red"><ins>Variables</ins></h3>

File: A directory should exist  
Yum: A package should be installed  
Service: A service should be running  
Template: Render as config file from a template  
get_url: Fetch an archive file from a URL 
git: clone a source code repository 

<h3 style="color: red"><ins>Tasks</ins></h3>

Nome + Modulo + Argumento  

```
tasks:
  - name: add cache dir
    file: 
      path: /opt/cache
      state: directory

  - name: install nginx
    yum: 
      name: nginx
      state: latest

  - name: restart nginx
    service:
      name: nginx
      state: restarted
```

<h3 style="color: red"><ins>Handler Tasks?</ins></h3>

> São listas de tasks (tarefas), não muito diferentes das tasks regulares que são 
referenciadas por um nome globalmente exclusivo e são notificadas pelos notificadores.
Se não for notificada não será executada.

```
handlers:
  - name: restart nginx
    service:
      name: nginx
      state: restarted
```

<h3 style="color: red"><ins>Para notifica-lo:</ins></h3>

```
tasks:
  - name: install nginx
    yum: 
      name: nginx
      state: latest
    notify: restart nginx   
```

<h3 style="color: red"><ins>Playbooks</ins></h3>

tasks  
templates = .j2  
--- = opcional  (indica o inicio)  
changed_when: false  
when  
<a href="redhat/site.yml">exemplo playbook</a>  
Para executar:  

>ansible-playbook -i hosts site.yml  

https://symfonycasts.com/screencast/ansible/idempotency-changed-when   
https://www.middlewareinventory.com/blog/ansible-changed_when-and-failed_when-examples/   
https://github.com/ansible/ansible/issues/14430   

# Ansible Roles

<h3 style="color: blue"><ins>Roles</ins></h3>

Roles are a package of closely related Ansible content the can be shared more  
easily than plays alone.

<h3 style="color: blue"><ins>Project with embedded Roles</ins></h3>

```
site.yml
role/
   common/
     files/
     templates/
     tasks/
     handlers/
     vars/
     defaults/
     meta/
    apache/
      files/
      templates/
      tasks/
      handlers/
      vars/
      defaults/
      meta/
```
<h3 style="color: blue"><ins>Playbook with roles</ins></h3>

```
# site.yml
---
- hosts: web
  roles:
     - common
     - apache
```

<h3 style="color: blue"><ins>Ansible Galaxy</ins></h3>

http://galaxy.ansible.com   
https://jinja.palletsprojects.com/en/2.11.x/   

Creating the Roles Structure with Ansible-Galaxy (and demonstration)
ansible-galaxy init --help

mkdir roles 
cd roles
ansible-galaxy init common
ansible-galaxy init apache

<h3 style="color: blue"><ins>Breaking an Existing Playbook into a Role (and demonstration)</ins></h3>

>touch /common/tasks/ntp.yml   
>touch /common/tasks/selinux.yml  
>ls -l /common/tasks/  
>ntp.yml  
>selinux.yml  
>main.yml  

vim roles/common/tasks/selinux.yml

```
---
- name: Install python bindigs for SELinux 
  yum: name={{tem}} state:present 
  with_items:
  - libselinux-python
  - libsemanage-python 
   
- name: test to see if Selinux s running
  command: getenforce 
  register: sestatus 
  changed_when: false
```

vim roles/common/tasks/ntp.yml

```
---
- name: install ntp
  yum: name=ntp state=present

- name: configure ntp file
  template: src=ntp.conf.j2 dest=/etc/ntp.conf
  notify: start ntp 

- name: start ntp
  service: name=ntpd state=started      
```

vim roles/common/handlers/main.yml
```
---
- name: restart ntp
  service: name=ntpd state=restarted
```

vim roles/common/templates/ntp.conf.j2
```
driftfile /var/lib/ntp/drift

restrict 127.0.0.1
restrict -6 ::1

server {{ ntpserver }}

includefile /etc/ntp/crypto/pw

keys /etc/ntp/keys
```

group_vars/all  
<a href="redhat/group_vars/all">file</a>

vim roles/common/tasks/main.yml 
```
---
- name: Install epel repo # nome para a tarefa 
  yum: name=epel-release state=present

- include: selinux.yml
- include: ntp.yml   
```

vim roles/apache/tasks/main.yml
```
- name: Install Apache 
  yum: name=httpd state=present 

- name: create sites directories 
  file: path={{item}} state=diretory
  with_items: "{{apacje_dirs}}"

- name: copy an index.html
  template: src=index.html.j2 dest={{apache_docroot}}/index.html

- name: copy httpd conf
  template: src=httpd.conf-{{ansible_os_family}}.j2 dest={{apache_config}}
  notify: restart apache 

- name: Start Apache 
  service: name=httpd state=started enabled=yes
```

vim roles/apache/template/index.html.j2  
<a href="redhat/roles/apache/templates/index.html.j2">index.html.j2</a>

vim roles/webserver/template/httpd.conf-RedHat.j2  
<a href="redhat/roles/apache/templates/httpd.conf-RedHat.j2">httpd.conf-RedHat.j2</a> 

vim roles/apache/handlers/main.yml 
```
---
- name: restart apache 
  service: name=httpd state=restarted
```

<h3 style="color: blue"><ins>Create New Role</ins></h3>

>cd /roles  
>ansible-galaxy init web  
>vim /roles/web/tasks/main.yml

```
---
- name: install git
  yum: name=git state=present

- name: checkout lameapp
  git: repo=https://github.com/jsmatin/lameapp.git version="{{lameapp_version|string}}"

- name: set permissions on the app
  file: name=/var/www/lameapp/lame.py mode=0755

- name: add apache config 
  copy: src=lameapp.conf dest={{sites_available}}
  notify: restart apache 

- name: link app config 
  file: src="{{sites_available}}/lameapp.conf" dest={{sites_enabled}}/lameapp.conf state=link

- meta: flush_handlers

- name: check for proper response 
  uri:
    url:= http://localhost/lame
    return_content= yes
  register: result  
  until: '"Hello Moon" in result.content'
```

vim /roles/web/files/lameapp.conf  
<a href="redhat/roles/web/files/lameapp.conf">lameapp.conf</a> 

vim /roles/web/handlers/main.yml  
```   
---
- name: restart apache 
  service: name=httpd state=restarted 
```

<h3 style="color: blue"><ins>Finale</ins></h3>

Ansible Tower:  
https://www.ansible.com/products/tower  

Ansible AWX:  
https://github.com/ansible/awx  
https://www.youtube.com/watch?v=ZatqBgn_Wic  
https://dataunique.com.br/blog/instalando-o-awx-para-gerenciamento-de-playbooks-ansible/  
https://medium.com/@alvarobacelar/simplificando-o-awx-1-6-156237ed7a22  

RBACK: ?  

Git:    
https://github.com/michelleperz
https://github.com/jsmartin/lameapp

