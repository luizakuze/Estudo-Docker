# Gerenciando a rede dos containers

## Alguns parâmetros do `docker run`

- `--dns` indica servidor dns (dns=converte o nome do site em um IP que o navegador possa carregar o site)
- --hostname indica um hostname (nome do dispositivo na rede)
- --link cria um link entr eos containers, sem a necessidade de saber o ip do outro
- --net permite configurar o modo de rede que você usa o container. 4 opções, em que a mais uitlizada é a --net-host, em que o container utiliza a rede do host para se comunicar  enão cria um stack de rede (organiza a comunicação em camadas) para o container.
- --expose expõe a porta do container apenas
- --publish expõe a porta do container e a do host
- --default-gateway determina a rota padrão
- --mac-address determina um MAC addres ( id único do dispositivo na rede local)

## Fazer com que a porta do container responda na porta do host
Se estiver nas configurações padrão, o Apache2 do container estará respondendo na porta 80/TCP .

Vamos fazer com que a porta 8080 do host responda pela porta 80 do container, ou seja, quando alguém bater na porta 8080 do host, a requisição será encaminhada para porta 80 do container.

1. Rodando o container
    ```bash
    docker container run -it -p 8080:80 linuxtips/apache:1.0 /bin/bash
    ps -ef
    /etc/init.d/apache2 start 
    ps -ef
    ip addr
    curl container-id:80 # curl com destino ao ip do container na porta 80
    curl ip-host:8080 # curl com destino ao ip do host na porta 8080
    ```

    - `-p 8080:80` `8080` é a porta do host e `80` é a do container. Significa que toda requisição que chegar na porta `8080` deverá ser encaminhada para porta `80`.

    Como isso acontece: Utiliza o módulo `netfilter` do Linux, que disponibiliza o `iptables`. as regras de NAT configuradas permitem o DNAT da porta 8080 do host para a 80 do container.
    
    
    ```
    sudo iptables -L -n
    sudo iptables -L -n -t nat
    ```