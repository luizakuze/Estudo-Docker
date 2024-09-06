# Executando e administrando containers Docker

1. Cria e executa um container utilizando a imagem personalizada do _hello-world_.
Criou o container, imprimiu a mensagem na tela e depois o container foi finalizado automaticamente. ( ou seja, executou sua tarefa).

    ```
    docker container run hello-world
    ```

    ```
    docker run hello-world
    ```

2. Com a imagem no _host_, visualizamos a imagem:

    ```
    docker image ls
    ```

3. Verificando containers em execução
    ```
    docker container ls
    ```

    flags:  -a para verificar containers parados, em execuçaõ ou que foram finalizados. -it para abrir terminal interativo e -d para rodar o container em segundo plano (não tranca o terminal).

4. Listando containers
    ```
    docker ps
    ```
 
5. Modo interativo

    Dentro do container, utilizando um shell.
    ```
    docker run -it centos:7
    cat /etc/redhat-release
    ```
    Caso queira sair, mas manter o container em execução: ``Ctrl + p + q``

    Acessando o container em execução novamente no shell: 
    ```
    docker container attach container-id
    ```

6. Criando um container, porém não inicializando:

    ```
    docker container create -it ubuntu
    ```

    Para inicializar ele depois:
    ```
    docker container start container-id
    ```

7. Parar um container em execução:
    ```
    docker container stop container-id
    ```
8. Reiniciar um container (stop+start)
    ```
    docker container restart container-id
    ```
9. Pausar um container
    ```
    docker container pause container-id
    ```
    Para despausar o container:
    ```
    docker container unpause container-if
    ```

10. Verificar consumo de CPU, memória e rede pelo container em tempo real:
    ```
    docker container stats container-id
    ```
    Para verificar todos os containers de uma vez: ``docker container stats container-id``

11. Verificar quais processos estão em execução em determinado container
    ```
    docker container top container-id
    ```
12. Verificando os logs
    ```
    docker container logs container-id
    ```
    flag: -f mostrar logs atualizando saída no terminal (trava o terminal)

13. Removendo containers.
    ```
    docker container rm container-id
    ```
    Para remover um container que está em execução adicionar a flag `-f`