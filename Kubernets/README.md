# Projeto de resolução do trabalho prático da matéria *Containers e Orquestração*

- [Escopo do projeto](#escopo-do-projeto)
- [Executando o projeto localmente](#escopo-do-projeto)
  - [Pré requisitos](#pré-requisitos)
  - [Passos](#passos)
- [Estrutura](#estrutura)
  - [Pasta Back](#back)
  - [Pasta DB](#db)
  - [Pasta Front](#front)
  - [Pasta Nginx](#nginx)
    

## Escopo do projeto.
Reimplementar a tarefa da unidade I Docker, utilizando os conceitos do Kubernetes.
- O Sistema deve funcionar acessando diretamente a porta do FrontEnd. Não é necessário ingress-controller.
  - Aceito port-forward para o front-end.
  - Aceito NodePort.
  - Aceito ingress.
- AutoScale com HPA deve ser implementado para o backend.
- As imagens devem estar no docker hub.

## Executando o projeto localmente.
### Pré requisitos.
- Para esse projeto foi utilizado o Docker Desktop.
  - Para SO [Windows](https://docs.docker.com/desktop/install/windows-install/)
  - Para SO [Linux](https://docs.docker.com/desktop/install/linux/)
  - Para SO [MAC](https://docs.docker.com/desktop/install/mac-install/)

### Passos:
- Executar o Docker Desktop.
- Acessar a raiz pasta descompactada, onde teremos a seguinte estrutura de pastas e arquivos:
  - back
    - deployment.yml
    - hpa-backend.yaml
    - service.yml
  - db
    - deployment.yml
    - pvc.yml
    - service.yml
  - front
     - deployment.yml
     - service.yml
  - nginx
     - deployment.yml
     - nginx-configmap.yml
     - service.yml
- Para cada pasta devemos executar o comando `kubectl apply -f <pasta>`. É aconselhave seguir a seguinte ordem:
  1. db `kubectl apply -f .\db\ `.
  2. back `kubectl apply -f .\back\ `.
  3. front `kubectl apply -f .\front\ `.
  4. nginx `kubectl apply -f .\nginx\ `.
- A cada execução devemos aguardar que os pods estejam disponíveis para executar o comando seguinte.
  - Podemos verificar o status dos pods através do comando `kubectl get pods`
  - Ao terminar de criar todos os pods devemos ter um log pareceido com esse da imagem abaixo:
![image](https://github.com/user-attachments/assets/1ad08c58-6fbf-4959-9bdb-f267ae99bdd6)
- Para acessar o pagina do front iremos utilizar o **port-forward** executando o seguinte comando: `kubectl port-forward service/frontend-service 80:3000`
- No navegador de preferência acessar a url http://localhost.

## Estrutura.
O projeto é composto por quatro pastas.
### back
> Dentro desta pasta temos os seguintes arquivos:
>
> - deployment.yml
>   - Responsável por realizar o deploy do backend, para esse deploy foi utilizado a imagem *my-backend:latest* que está disponivel no [meu repositório do docker hub](https://hub.docker.com/repository/docker/adilson90/my-backend/general).
> - hpa-backend.yaml
>   - Responsável por configurar o Auto Scaler dos pods do backend, onde no projeto definimos um mínimo de 2 pods e um máximo de 5.
> - service.yml
>   - Responsável por configurar o serviço do Backend, foi configurado o tipo ClusterIP e disponibilizado na porta 5000.

### db
> Dentro desta pasta temos os seguintes arquivos:
>
> - deployment.yml
>   - Responsável por realizar o deploy do banco de dados postgres, para esse deploy foi utilizado a imagem *postgres:latest*.
> - pvc.yml
>   - Responsável por configurar o volume persistente do banco de dados, foi definido um tamanho de 1GB para este volume.
> - service.yml
>   - Responsável por configurar o serviço do banco de dados, foi disponibilizado na porta 5432.

### front
> Dentro desta pasta temos os seguintes arquivos:
>
> - deployment.yml
>   - Responsável por realizar o deploy do frontend, para esse deploy foi utilizado a imagem *my-frontend:latest* que está disponivel no [meu repositório do docker hub](https://hub.docker.com/repository/docker/adilson90/my-frontend/general).
> - service.yml
>   - Responsável por configurar o serviço do frontend, foi configurado o tipo ClusterIP e disponibilizado na porta 3000.

### nginx
> Dentro desta pasta temos os seguintes arquivos:
>
> - deployment.yml
>   - Responsável por realizar o deploy do ngin, para esse deploy foi utilizado a imagem *nginx:latest*.
> - nginx-configmap.yml
>   - Responsável por configurar o proxy reverso e expor as urls dos serviços de front e back.
> - service.yml
>   - Responsável por configurar o serviço do nginx, foi configurado o tipo NodePort para que o front pudesse acessá-lo e disponibilizado internamente na porta 80 e externamente na porta 32000.