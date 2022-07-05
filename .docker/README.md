### Habilitando o xdebug

Rode o comando:

``hostname -I | cut -d ' ' -f1``

Copie o IP e altere o arquivo **./docker/xdebug.ini** em **xdebug.client_host**.

### Adicionar o host

Altere o arquivo **/etc/hosts** e adicione o IP:

```127.0.0.1    api.mastertax.local```