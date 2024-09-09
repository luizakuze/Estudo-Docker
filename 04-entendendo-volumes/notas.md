# Montando volumes

A função do volume é persistir os dados, eles são diretórios externos ao container.

## Método para quando se quer montar um diretório específico do host dentro do container. (Abordagem ruim para trabalhos em cluster)

1. Montando um volume - montar um diretório vezio do host dentro do container 
 

    ```
    mkdir /volume
    docker run -it --mount type=bind,src=/volume,target=/volume ubuntu

    df -h
    ls
    ```

    Parâmetro `--mount` indica o volume (que é /volume) e onde será montado.
    `-mount type=bind,source=/volume,target=/volume` significa que o docker montou esse diretório n ocontainer vazio.

2. Montando um container linkando ele com um diretório host que já tem um conteúdo
    Montando o diretório `/root/primeiro_container` do host dentro do container com o nome `/volume`.

    ```
    docker container run -it --mount type=bind,src=./root/primeiro_container,dst=/volume ubuntu
    df -h
    ```

    É possível deixar o volume no container apenas como read-only utilizando o parâmetro `ro`:

    ```
    docker container run -it --mount type=bind,src=./root/primeiro_container,dst=/volume,ro ubuntu
 
    ```

    E também, ao invés de montar um diretório do host, pode ser somente um arquivo do host
    ```
    docker container run -it --mount type=bind,src=./root/primeiro_container/dockerfile,dst=/volume ubuntu
    df -h
    ```

## Método de sempre criar volumes dentro de `/var/lib/docker/volumes/`

1. Comandos
    ```bash
    docker volume create giropops # criar 
    docker volume rm giropops # remover 
    docker volume inspect giropops # verificar detalhes
    docker volume prune # remover os volumes não utilizados
    
    ```

2. Criando umm volume em `/var/lib/docker/volumes/`
    ```
    docker volume create giropops  
    docker container run -it --mount type=volume,src=giropops,dst=/var/opa nginx 
    ```
    - `type=volume` indica que é um volume
    - `src=giropops` qual volume pretende montar
    - `dst=/var/opa` onde vai ser montado
### Alguns comandos...

1. Visualizar volumes
    ```
    docker volume ls
    ```

2. Obter informações do volume
    ```bash
    docker volume inspect giropops # todas as informações
    docker inspect -f {{.Mounts}} giropops # filtrando somente a localização do volume
    ```

## Criando e Montando um data-only container

    A função desse container será prover volumes para outros container.

1. Exemplo
    Container vai ser chamado de "dbdados", com volume chamado "/data", que guardará dados de um banco PostgreSQL
    ```
    docker container create -v /data --name dbdados centos
    ```
    O volume está vazio, podemos ver isso obtendo sua localização (docker inspect) e verificando o diretório (ls)

    Criando agora os containers que rodarão o PostgreSQL utilizando o volume "/data" do container "dbdados" para guardar dados.
    - `--volumes-from` para montar um container disponibilizado por outro container
    - `-e` informar as variáveis de ambiente para o container.
    (no exemplo são passadas variáveis de ambiente com o PostgreSQL)
    
    ```
    docker run -dp 5432:5432 --name pgsql1 --volumes-from dbdados \
    -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker \
    -e POSTGRESQL_DB=docker kamui/postgresql
    ```

    ```
    docker run -dp 5433:5432 --name pgsql2 --volumes-from dbdados \                                            
    -e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker \
    -e POSTGRESQL_DB=docker kamui/postgresql
    ```

    Aqui, tem dois containers com PostgreSQL em execução. 
    Para verificar se já algum dado no volume `"/data"` do container "dbdados", utilizar `docker inspect -f {{.Mount}} dbdados` e `ls` nesse path.

    O comando abaixo cria um novo container que monta o volume do container dbdados (localizado em "/data") e também monta um diretório do host no container no caminho "/backup":
    ```
    docker run -it --volumes-from dbdados -v $(pwd):/backup debian tar -cvf /backup/backup.tar /dat
    ``` 