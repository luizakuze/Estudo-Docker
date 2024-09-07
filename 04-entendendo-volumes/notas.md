# Montando volumes

A função do volume é persistir os dados, eles são diretórios externos ao container.

1. Montando um volume
 
    Método para quando se quer montar um diretório específico do host dentro do container. (Abordagem ruim para trabalhos em cluster)

    ```
    mkdir /volume
    docker run -it --mount type=bind,source=$(pwd)/volume,target=/volume ubuntu

    df -h
    ls
    ```

    Parâmetro `--mount` indica o volume que é /volume, onde será montado.
    `-mount type=bind,source=$(pwd)/volume,target=/volume` significa que o docker montou esse diretório n ocontainer vazio.

    
