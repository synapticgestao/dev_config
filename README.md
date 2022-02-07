# Notas para desenvolvimento SYNAPTIC
## Montando ambiente
 - instalar e configurar o docker e docker-compose.
 - clonar o projeto synaptic numa pasta dentro desta (na minha pasta o projeto tá numa pasta chamada **site**, peço que utilizem também, pois toda a config da pasta está vinculada a esse pathname).
	> git clone git@github.com:synapticgestao/synaptic.git site
 - criar os arquivos abaixo, a partir de seus exemplos no projeto:
	 - site/config/autoload/doctrine_orm.local.php
	- site/config/autoload/mongo.local.php
- ajustar endereço ip do usuário, para o xdebug, no arquivo docker-compose.yml (colocar o seu ip onde está <seu_ip>, ps.: não colocar localhost, nem 127.0.0.1):
	- "host-docker-external:<seu_ip>"
- iniciar os containers.
	> docker-compose up -d
- rodar o compose no container
	> docker exec -ti synaptic_site bash -c 'composer update'
- criar pastas temporárias e de dados.
	> mkdir data
	> mkdir public/fotos
	> mkdir -p module/Application/src/Application/Entity
- atribuir permissões:
	> chmod -R 777 data public/fotos vendor/mpdf
## restaurando banco
- pegar a última versão do banco em formato zip e salvar nessa pasta.
- rodar o comando que estrai e carrega o banco (instalar o unzip).
	> rm synaptic_db.sql
	> unzip $(ls -Art *.zip | tail -n 1)
	> docker cp synaptic_db.sql synaptic_db:/tmp
	> docker exec -ti synaptic_db bash -c "mysql -psynaptic < /tmp/synaptic_db.sql"
## Acessando sistema
- acessar o sistema no endereço: http://localhost:8084