# Projeto de resolução do trabalho pratico da máteria *Containers e Orquestração*
## Escopo do projeto
Utilizando o jogo demo disponível em [https://github.com/fams/guess_game](https://github.com/fams/guess_game), deverá implementar uma estrutura com Docker Compose que englobe os seguintes serviços:
- Um container para o backend em Python (Flask).
- Um container para o banco de dados Postgres.
- Um container NGINX atuando como proxy reverso e servindo as páginas do frontend React.

## Executando o projeto localmente
### Pré requisitos
- Ter o [GIT ](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) instalado localmete
- Pare esse projeto foi utilizado o Docker Desktop
  - Para SO [Windows](https://docs.docker.com/desktop/install/windows-install/)
  - Para SO [Linux](https://docs.docker.com/desktop/install/linux/)
  - Para SO [MAC](https://docs.docker.com/desktop/install/mac-install/)

 ### Passos:
 - Executar o Docker Desktop
 - Clonar este projeto
 - Acessar a raiz do projeto recem clonado, onde teremos o arquivo `docker-compose.yml`
 - Exectar o comando `docker compose up`
 - Aguardar até as imagens e os containers estejam disponiveis.
   - Podemos verificar o status atraves do menu containers do Docker Desktop
   - Será listado todos os containers que foram criados e o status de cada um
- No navegador de preferencia acessar a url http://localhost

## Estrutura
O projeto é composto por quatro arquivos.
### Dockerfile.Backend
Utilizamos o Dockerfile.Backend para criarmos disponibilizarmos o backend da aplicação(Flask), foi utilizado uma imagen [phyton](https://hub.docker.com/layers/library/python/3.10-slim/images/sha256-e7d6ea327beabaeeb064aa3334a56ab0d99ca52e335abcbacf6b1af3fe543def?context=explore).

Para facilitar a alteração versão da imagem phyton foi definida uma variavel *versionPhyton*, onde a mesma foi declarada e pode ser alterada no arquivo `docker-compose.yml`.

Outra medida que foi tomada para fácilitar a manutenção do projeto foi a abordagem de clonar o projeto [https://github.com/fams/guess_game](https://github.com/fams/guess_game) `RUN git clone https://github.com/fams/guess_game.git ./guess_game `.
Com essa aboordagem a aplicação **guess_game** poderá receber alterações sem a necessidade de alterarmos o presente projeto.

### Dockerfile.Frontend
Utilizamos o Dockerfile.Frontend para criarmos e disponibilizarmos o frontend da aplicação, foi utilizado uma imagem [node](https://hub.docker.com/layers/library/node/20.10.0-alpine3.18/images/sha256-d016f19a31ac259d78dc870b4c78132cf9e52e89339ff319bdd9999912818f4a?context=explore)

Para facilitar a alteração versão da imagem node foi definida uma variavel *versionNode*, onde a mesma foi declarada e pode ser alterada no arquivo `docker-compose.yml`.

Outra medida que foi tomada para fácilitar a manutenção do projeto foi a abordagem de clonar o projeto [https://github.com/fams/guess_game](https://github.com/fams/guess_game) `RUN git clone https://github.com/fams/guess_game.git ./guess_game `.
Com essa aboordagem a aplicação **guess_game** poderá receber alterações sem a necessidade de alterarmos o presente projeto.

### nginx.conf
Utilizamos o nginx.conf para defiinr um servidor NGINX com duas regras de proxy uma para o serviço de frontend e outra para o serviço de backend, onde seram encaminhadas as requisições para diferentes containers com base no caminho da URL.

### docker-compose.yml
Utilizamos o docker-compose.yml para declaramos os containers, foram declarados quatro serviços db, back, front e nginx. Além dos serviços tambem foi declarado um volume.

Dealhes dos serviços:
**db**:
- container_name: Define o nome do container.
- restart: Define qual vai ser a estrategia de reinicialização do container.
- image: Define qual vai ser a imagem utilizada.
- environment: Define algumas variaveis necessarias para configurar o banco de dados.
- volumes: Define um volume persistente que não sera perdido ao reiniciar o container.
  
**back**: 
- restart: Define qual vai ser a estrategia de reinicialização do container.
- build: Define qual vai ser a estrategia para realizar o build, neste o caso Dockerfile.Backend. Além disso foi definido uma variavel para ser utilizada na sexcução do arquivo Dockerfile.
- environment: Define algumas variaveis necessarias para configurar o banco de dados.
- depends_on: Define a dependencia de um outro serviço para que o serviço atual seja executado.
- deploy: Através da propriedade *replicas* foi definido um numero de replicas daquele container, com isso teremos um cenário para testar o balanceamento de carga







