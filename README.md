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
<p>all=todos os hostsa -i=quem é o arquivo de hosts -u=usuario -m modulo</p>
<p>o modulo "ping" apenas faz um ping nos hosts para testar a comunicacao</p>
<p>o modulo "setup" faz um ansible_facts, coleta informações dos servers, util para condicionais</p>
<p> o modulo "yum" instala pacotes no redhat e derivados</p>
<p>-a=arguments que no caso é name=o nome do pacote, state=? [present], [absent] e o -b para utilizar sudo</p>
<p></p>

