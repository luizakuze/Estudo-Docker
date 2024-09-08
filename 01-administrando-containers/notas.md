# Executando e administrando containers Docker

1. Cria e executa um container utilizando a imagem personalizada do _hello-world_.
   Criou o container, imprimiu a mensagem na tela e depois o container foi finalizado automaticamente (ou seja, executou sua tarefa).

    ```sh
    docker container run hello-world
    ```

    ```sh
    docker run hello-world
    ```

2. Com a imagem no _host_, visualizamos a imagem:

    ```sh
    docker image ls
    ```

3. Verificando containers em execução:

    ```sh
    docker container ls
    ```

    Flags: 
    - `-a` para verificar containers parados, em execução ou que foram finalizados.
    - `-it` para abrir terminal interativo.
    - `-d` para rodar o container em segundo plano (não tranca o terminal).

4. Listando containers:

    ```sh
    docker ps
    ```

5. Modo interativo:

    Dentro do container, utilizando um shell.

    ```sh
    docker run -it centos:7
    cat /etc/redhat-release
    ```

    Caso queira sair, mas manter o container em execução: `Ctrl + p + q`

    Acessando o container em execução novamente no shell:

    ```sh
    docker container attach container-id
    ```

6. Criando um container, porém não inicializando:

    ```sh
    docker container create -it ubuntu
    ```

    Para inicializar ele depois:

    ```sh
    docker container start container-id
    ```

7. Parar um container em execução:

    ```sh
    docker container stop container-id
    ```

8. Reiniciar um container (stop + start):

    ```sh
    docker container restart container-id
    ```

9. Pausar um container:

    ```sh
    docker container pause container-id
    ```

    Para despausar o container:

    ```sh
    docker container unpause container-id
    ```

10. Verificar consumo de CPU, memória e rede pelo container em tempo real:

    ```sh
    docker container stats container-id
    ```

    Para verificar todos os containers de uma vez:

    ```sh
    docker container stats
    ```

11. Verificar quais processos estão em execução em determinado container:

    ```sh
    docker container top container-id
    ```

12. Verificando os logs:

    ```sh
    docker container logs container-id
    ```

    Flag: `-f` mostrar logs atualizando saída no terminal (trava o terminal).

13. Removendo containers:

    ```sh
    docker container rm container-id
    ```

    Para remover um container que está em execução adicionar a flag `-f`:

    ```sh
    docker container rm -f container-id
    ```

```markdown
# Nota:
## Diferença do comando `stop` e `pause`

- Com `stop`, o container é completamente parado, força a parada dos processos. É necessário reiniciar com `start`.
- Com `pause`, os processos são apenas suspensos (parados temporariamente) e podem ser retomados (`unpause`).
```