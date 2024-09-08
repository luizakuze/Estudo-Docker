# Docker Machine

Possibilidade de executar com apenas um comando o projeto com Docker.

1. Instalação 

    Do Docker Machine:

    ```sh
    curl -L https://github.com/docker/machine/releases/download/v0.12.0/docker-machine-`uname -s`-`uname -m` -o /tmp/docker-machine
    chmod +x /tmp/docker-machine
    sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
    ```

    > Objetivo: Fazer com que o Docker Machine instale o Docker Host utilizando o VirtualBox
    
    > `sudo apt-get install virtualbox`

2. Instalação de um novo Docker Host
     
    ```sh
    sudo docker-machine create --driver virtualbox linuxtips
    ```
    - `docker-machine create` Cria um novo Docker host.
    - `--driver virtualbox`  Utiliza o VirtualBox como driver para criar a VM.
    -  `linuxtips` Nome da VM que será criada.

3. Verificando a Instalação:

    ```sh
    docker-machine ls
    docker-machine env linuxtips
    ```
 

4. Acessando o Ambiente do Host:

    ```sh
    eval $(docker-machine env linuxtips)
    ```
 

5. Iniciando um Container em Execução:

    ```sh
    docker run -d --name meu-container nginx
    ```
 

6. Verificando o IP do Host:

    ```sh
    docker-machine ip linuxtips
    ``` 

7. Conectando-se ao Host via SSH:

    ```sh
    docker-machine ssh linuxtips
    ```

    - `docker-machine ssh linuxtips`  Conecta-se ao host Docker via SSH.

8. Removendo a Máquina Docker:

    ```sh
    docker-machine rm linuxtips
    ```
 