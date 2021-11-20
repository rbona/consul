no arquivo "docker-compose.yaml" está a configuração para disparar 3 servidores com o consul

Para iniciar um "agent" do consul que vai servir para monitoramento deve ser seguido os passos abaixo:
1. abrir um terminal
2. docker exec -it <nome do container> sh
3. disparar o "ifconfig" para descobrir o IP do container
4. executar o comando:
   => mkdir /etc/consul.d
   => mkdir /var/lib/consul
   => consul agent -server -bootstrap-expect=<qtd servidores esperado> -node=<nome do container> -bind=<ip do container> -data-dir=/var/lib/consul -config-dir=/etc/consul.d

Para cada servidor deve ser executada a sequencia acima

Para verificar os membros de um distrito deve ser feito
1. abrir um terminal
2. docker exec -it <nome do container> sh
3. consul member

Para incluir os agentes em um mesmo distrito, basta em um dos terminais dispara o comando abaixo
=> consul join <ip do container>

Para gerar uma chave de criptografia. 
1. Acessar o terminal do container
2. Executar o comando: consul keygen
3. Copiar a chave para o item "encrypt" no json do container em servers 
Se um dos componentes do cluster não estiver criptografado, enquanto os outros estiverem, esse cluster sem criptografia não será conectado no cluster
Deve ser utilizada a mesma chave para todos os agents

Para acessar a user interface do consul, deve ser incluido no comando de inicialização do agent o seguinte:
-ui -client=0.0.0.0
Ou incluir no json de configuração dos servidores.
No final deve ser compartilhada a porta 8500 do container
Para acessar a UI basta acessar a url localhost:8500/ui
