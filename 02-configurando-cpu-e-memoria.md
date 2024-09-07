# Configurando CPU e memória para os meus containers

É importante limitar a utilização de recursos dos containers, pois serviços diferentes utilizam uma quantidade de memória diferente.

1. Especificando a quantidade de memória (antes de executar):
    ```
    docker container run -it --name teste debian
    ```
    Visualizando a quantidade de memória que está configurada para esse container
    ```
    docker container inspect teste | grep -i mem
    ```
    Valores `Memory` está zerado, já que nem existe nenhum limite estabelecido.

    Colocando uma restrição de 512MB de memória:
    ```
    docker container run -it -m 512M --name novo_container debian
    ```
    Podemos utilizar o comando docker container stats para verificar o limite

2. Especificando a quantidade de CPU (antes de executar):
    ```
    docker container run --cpus=0.5 --name teste1 nginx
    ```
    - Container pode utilizar no máximo 0,5 CPU, ou seja, metade de 1 core.

3. Alterando especificação (após execução):
    ```
    docker container update -m 256m --cpus=1 teste1 
    ```
