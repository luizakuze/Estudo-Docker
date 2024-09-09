# Criando e Gerenciando Imagens


Duas formas: Montar uma distribuição praticamente do zero utilizando somente instruções através do dockerfile e outra forma é realizar modificações em uma imagem já existente e salvando em uma imagem nova

## I. Dockerfile do zero 


1. Montar o Dockerfile

    ```docker
    FROM debian 

    RUN apt-get update && apt-get install -y apache2 && apt-get clean
    ENV APACHE_LOCK_DIR ="var/lock"
    ENV APACHE_PID_FILE="var/run/apache2.pid"
    ENV APACHE_RUN_USER="wwww-data"
    ENV APACHE_RUN_FROUP="wwww-data"
    ENV APACHE_LOG_DIR="var/log/apache2"

    LABEL description="Webserver"

    VOLUME /var/www/html/
    EXPOSE 80 
    ```

    - `FROM` indica a imagem a servir como base. Precisa ser a primeira linha do dockerfile.
    - `RUN` lista de comandos a serrem executados na criação da imagem
    - `ENV` define as variáveis de ambiente
    - `LABEL` adiciona _metadata_ à imagem, como descrição e versão
    - `VOLUME` define um volume a ser montado no container

2. Construir da imagem
    É necessário estar no diretório em que está o dockerfile
    ```bash
    docker build . # sem tag
    docker build -t linuxtips/apache:1.0 . # com tag
    ```
 
3. Executar um container utilizando a imagem como base
    ```bash
    docker container run -it linuxtips/apache:1.0
    ```
     ---
    Nesse exemplo, adicionalmente, para subir o processo...

    ```bash
    ps -ef # verificando se o apache está em execução
    /etc/init.d/apache2 start # inicializando apache
    ss -atn # exibir informações sobre a rede TCP do sistema
    ip addr show eth0 # informações da rede para a interface de rede eth0 (contém ip do container)
    curl <ip-container> # requisição http para o endereço ip do container
    ```
    ---

    Porém... Foi necessário entrar no container para subir o processo do Apache.  Todo container deve executar seu processo em primeiro plano sozinho. A alteração do dockerfile é a seguinte:

    ```DOCKER
    FROM debian 

    RUN apt-get update && apt-get install -y apache2 && apt-get clean
    ENV APACHE_LOCK_DIR ="var/lock"
    ENV APACHE_PID_FILE="var/run/apache2.pid"
    ENV APACHE_RUN_USER="wwww-data"
    ENV APACHE_RUN_FROUP="wwww-data"
    ENV APACHE_LOG_DIR="var/log/apache2"

    LABEL description="Webserver"

    VOLUME /var/www/html/
    EXPOSE 80 

    # MUDANÇA:
    ENTRYPOINT ["/usr/sbin/apachect1"]
    CMD ["-D", "FOREGROUND"]
    ```


    - `CMD` executa um comando. A diferença do `RUN`, é que o RUN é utilizado no momento que a imagem é construída. O `CMD` irá fazê-lo somente quando o container for iniciado.
    - `ENTRYPOINT` permite que você configura um container para rodar um executável. Quando o executável for finalizado, o container também é finalizado.

    > O `CMD` somente aceita parâmetros do `ENTRYPOINT` quando utilizados em conjunto. 

    >`/usr/sbin/apachect1` é um comando  e `"-D", "FOREGROUND"` são os parâmetros.
    > No shell, por exemplo, a execução ficaria: `/usr/sbin/apachect1 -D FOREGROUND`



### Alguns comandos diferentes do dockerfile...
- `ADD` copia novos arquivos e diretórios ao filesystem do container e também extrair arquivos tar ou fazer download de uma URL.
- `COPY` copia novos arquivo s e diretórios ao filesystem do container.
- `EXPOSE` informa qual porta o container estará ouvindo
- `MAINTAINER` autor da imagem
- `USER` indica qual usuário será utilizado na imagem (default = root)
- `VOLUME` permite criação de um ponto de montagem no container
- `WORKDIR` responsável por mudar o diretório "/" (raiz) para o diretório especificado nele.

---

### Multi-stage

Possibilita a criação de um _pipeline_ no dockerfile, podendo até mesmo conter duas entradas `FROM`

Útil quando se quer compilar a aplicação em um container e exeutá-la, mas não instalando uma quantidade enorme de pacotes necessários pelos códigos em algumas linguagens, como Java ou Golang.


Dockerfile utilizado:
```docker
FROM golang

WORKDIR /app
ADD . /app
RUN go build -o goapp
ENTRYPOINT ./goapp
```
- Utilizando imagem golang para criação da imagem do container, pois já configura o ambiente

Aplicação em Go:
```golang
package main
import "fmt"

func main() {
    fmt.Println("GIROPOPS STRIGUS GIRUS - LINUXTIPS");
}
```

Comandos: 
```bash
docker build -t goapp:1.0 . 
docker image ls | grep goapp
docker run -it goapp:1.0 .
```



Refazendo o código do Dockerfile, melhorando ele:
- Duas entradas `FROM`. Cada entrada dessa define o início de um bloco.
```docker
FROM golang as construindo 

WORKDIR /src 
ADD . /src 
RUN go build -o goapp

FROM apline:3.1

WORKDIR /app
COPY --from=construindo /src/goapp /app
ENTRYPOINT [ "./goapp" ]
```
#### Primeiro bloco:
- `FROM golang as construindo`  apelidando primeiro bloco de "construindo", utilizando a imagem do goland para criação da imagem de container
- `WORKDIR /src` diretório de trabalho, quando o container iniciar estaremos nesse diretório.
- `ADD . /src` adicionando a app dentro do diretório `/src`
- `RUN go build -o goapp` executa o build da aplicação golang
#### Segundo bloco
- `FROM apline:3.1` iniciando segundo bloco, utilizando imagem do alpine para criação da imagem de container
- `WORKDIR /app` definindo outro diretório de trabalho
- `COPY --from=construindo /src/goapp /app` copia o bloco chamado "construindo" que está em /srsc/goapp para /app, ou seja, copiando o binário que foi compilado no bloco anterior para esse.
- `ENTRYPOINT  ./goapp` executa app
- Utilizar o Alpine para criação da imagem do container é um prática comum para reduzir o tamanho das imagens. Documentação: `https://hub.docker.com/_/alpine`.

Construindo, rodando e verificando:
```
docker build -t goapp_multistage:1.0 .
docker run -it goapp_multistage:1.0 
docker image ls | grep goapp 
```

A imagem sem multistage precisa de muitos pacotes para que o build da app aconteça. Na imagme com multistage, também são utilizados esses pacotes para o build, mas eles são descartados e apenas seu binário é copiado para o segundo bloco, onde utilizamos somente a imagem do alpine.

---

## II. Costumizando uma imagem base
1. Criando e xecutando um container com uma imagem base
    ```
    docker container run -it debian:8 /bin/bash
    ``` 

2. Fazendo algumas alterações no container
    ``` 
    apt-get update && apt-get install -y apache2 && apt-get clean
    ``` 
    Instalado o `apache2`, dsaindo do container e ainda deixando ele em execução `Ctrl + p + q`

3. Criando a imagem com alterações
    ```bash
    docker container ls 
    docker commit -m "meu container" container-id # docker commit cria imagem
    docker image ls
    $ docker tag image-id linuxtips/apache_2:1.0
    ```

4. Iniciando container utilizando a imagem com alterações
    ``` 
    docker container run -it linuxtips/apache_2:1.0 /bin/bash
    ```