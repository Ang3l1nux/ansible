zabbix-agent
=========

[![pipeline status](https://gitlab.com/Rivendel-Labs/ansible/roles/zabbix-agent/badges/master/pipeline.svg)](https://gitlab.com/Rivendel-Labs/ansible/roles/zabbix-agent/commits/master)

Agente de monitoramento do Zabbix, basicamente envia informações sobre o node/nó para o servidor Zabbix

Requerimentos
------------

Nenhum

Variáveis
--------------

Abaixo informacões sobre as variáveis, assim como seus valores padrão

    zabbix_agent_version: 3.2-1

Versão do agente a ser instalada, lembrando que é importante que este valor seja o mesmo tanto para o servidor Zabbix quanto para o agente.

    zabbix_agent_client_hostname: "{{ ansible_hostname }}"

Nome de exibição a ser usado para o zabbix-server, por padrão ele utiliza o hostname já definido no servidor.

    zabbix_agent_server_ip: 0.0.0.0

IP do servidor zabbix-server, é através desse parametro que o zabbix-agent irá se comunicar com o servidor.

    zabbix_agent_host_metadata: test

Grupo metadata que será cadastrado na execução do zabbix-agent, esta configuração permite que no primeiro contato do servidor com o zabbix-server seja possivel que ele já entre em um grupo de hosts.

    zabbix_agent_autoremove_enabled: true

Habilita a instalação do script de remoção automatica do host no zabbix-server via API.

    zabbix_agent_server_host: http://localhost/api_jsonrpc.php

URL do zabbix-server que será usada para as comunicações entre a API.

Dependências
------------

Nenhum

Playbook Exemplo
----------------

    - hosts: all
      vars:
        zabbix_agent_server_ip: 192.168.0.125
        zabbix_agent_host_metadata: linux_servers
        zabbix_agent_server_host: http://192.168.0.125/api_jsonrpc.php

      roles:
         - zabbix-agent

Licença
-------

BSD

Informações sobre o autor
------------------

Esta role foi desenvolvida por Diogo Lima (diogo.lima@rivendel.com.br) e refatorada por Thiago Freitas (thiago.freitas@rivendel.com.br) em 02/2018.
