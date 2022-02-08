## Notas para desenvolvimento SYNAPTIC (LINUX)
### Montando ambiente
 - instalar e configurar o docker e docker-compose.
 - clonar o projeto synaptic numa pasta dentro desta (na minha pasta o projeto tá numa pasta chamada **site**, peço que utilizem também, pois toda a config da pasta está vinculada a esse pathname).
	> git clone git@github.com:synapticgestao/synaptic.git site
 - criar os arquivos abaixo, a partir de seus exemplos no projeto:
	 > cp site/config/autoload/doctrine_orm.local.example.php site/config/autoload/doctrine_orm.local.php; 
	 
	> cp site/config/autoload/mongo.local.example.php site/config/autoload/mongo.local.php; 
- ajustar endereço ip do usuário, para o xdebug, no arquivo docker-compose.yml (colocar o seu ip onde está <seu_ip>, ps.: não colocar localhost, nem 127.0.0.1):
	- "host-docker-external:<seu_ip>"
- iniciar os containers.
	> docker-compose up -d
- rodar o compose no container:
	> docker exec -ti synaptic_site bash -c 'composer update'
- criar pastas temporárias e de dados no sistema.
	> mkdir site/data
	
	> mkdir site/public/fotos
	
	> mkdir -p site/module/Application/src/Application/Entity
- atribuir permissões:
	> sudo chmod -R 777 site/data site/public/fotos site/vendor/mpdf
### restaurando banco
- pegar a última versão do banco em formato zip e salvar nessa pasta.
- rodar o comando que estrai e carrega o banco (instalar o unzip).
	> rm synaptic_db.sql
	
	> unzip $(ls -Art *.zip | tail -n 1)
	
	> docker cp synaptic_db.sql synaptic_db:/tmp
	
	> docker exec -ti synaptic_db bash -c 'mysql -psynaptic < /tmp/synaptic_db.sql'
### Acessando sistema
- acessar o sistema no endereço: http://localhost:8084
### vscode
- configuração para conectar ao xdebug ... .vscode/launch.json (ps.: **isso já vai no repositório, apenas para informação**):

        {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Listen for XDebug",
                "type": "php",
                "request": "launch",
                "port": 9003,
                "pathMappings": {
                  "/var/www/html": "${workspaceRoot}"
                }
            }
        ]
    }
