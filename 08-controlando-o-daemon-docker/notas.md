# Controlando o _daemon_ do Docker

O _daemon_ do Docker é uma espécie de processo-pai que controla tudo, containers, imagens e etc.
Em SO multitaks, isto é, um sistema oepracional que executa "mais de uma tarefa por vez", um _daemon_ é um software que roda de forma independente em background e executa certas ações predefinidas em resposta a certos eventos.


1. Configurar outro intervalo de IP
    Configurar um outro `range`para serem utilizados pela `bridge "docker0"`e pelas outras interfaces de rede
    ```
    dockerd --bip 192.168.0.1/24
    ```
    Restringir o range  que o Docker utilizará para brigde docker0 e para subnet dos container
    ```
    dockerd --fixed-cidr 192.168.0.1/24
    ```
 