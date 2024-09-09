# Docker Compose

- Referência: `https://blog.4linux.com.br/docker-compose-explicado/`

Para realizar a execução de um contêiner basta executar o comando docker container run, porém, e se for necessário executar vários contêineres de uma única vez e cada um com um propósito distinto?  

Seria muito trabalhoso executar diversas vezes o mesmo comando e ainda incluir os parâmetros necessários, além da criação da rede e volumes, se necessário.

É aí que entra o Docker Compose, para facilitar o gerenciamento de multi-contêineres principalmente em ambientes de desenvolvimento.


- `docker-compose up`: cria e inicia os contêineres;
- `docker-compose build`: realiza apenas a etapa de build das imagens que serão utilizadas;
- `docker-compose logs`: visualiza os logs dos contêineres;
- `docker-compose restart`: reinicia os contêineres;
- `docker-compose ps`: lista os contêineres;
- `docker-compose scale`: permite aumentar o número de réplicas de um contêiner;
- `docker-compose start`: inicia os contêineres;
- `docker-compose stop`: paralisa os contêineres;
- `docker-compose down`: paralisa e remove todos os contêineres e seus componentes como rede, imagem e volume.


1. Iniciar todos os serviços com as configurações do docker compose

    ```
    docker-compose up -d
    ```
    > Para o docker compose do diretório `12-docker-compose/artigo/Composes/2` faz com que inicie um serviço WordPress e um serviço d ebanco de dados MySQL.

2. Removendo containers e recursos criados
    ```
    docker-compose down
    ```