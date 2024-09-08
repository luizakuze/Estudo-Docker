# Compartilhando Imagens

## Imagens no Docker Hub
Ao pegar a imagens diretamente do Docker Hub, testamos ela antes de colocada ela diretamente em produção. Podemos testar ela com alguns comandos:


```bash
docker image inspect debian
docker history linuxtips/apache:1.0 # camadas da iamgem
```

o site `ImageLayers` faz a mesma coisa que o `docker history`, mas não é preciso baixar a imagem.

Um dos componentes do Docker Hub, é o `registry`. Ele é responsável pelo repositório d eimagens do Docker Hub. 

1. Login para uso do Docker Hub
    ```
    docker login
    ```
    Para especificar outro registry em vez do Docker Hub:
    ```
    docker login registry.nome_registry.com
    ```

2. Compartilhando imagem (push)
    A iamgem deve ter o seguitne padrão: `linuxtips/apache:1.0`, onde `linustips` é o nome do usuário do Docker Hub, `apache` é o nome da imagem e `1.0` é a versão.

    Upload da imagem da máquina local para o registry do Docker Hub:
    ```
    docker push linuxtips:apache:1.0
    ```

3. Visualizando imagem pela linha de comando
    ```
    docker search luizakuze/apache:1.0
    ```

4. Trazendo imagem para máquina local (pull)
    ```bash
    docker container ls | grep luizakuze/apache:1.0  
    docker container stop container-id 
    docker image rm -f luizakuze/apache:1.0 # removendo imagem local
    docker pull luizakuze/apache:1.0 # trazendo imagem
    docker image ls
    ```


---

## Criando um Registry Local

Muitas empresas não gostam de manter seus dados na nuvem em serviços de terceiros, a alternativa é criar um registry local para as pessoas da empresa. O projeto está armazenado em `https://github.com/distribution/distribution`

### Utilizando `Docker Distribution`
Vai guardar e distribuir imagens assim como o Docker Hub, mas localmente.
1. Rodar o Docker Distribution como um container
    ```
    docker container run -dp 5000:5000 --restart=always --name registry registry:2
    ```
    Criou um container chamado `registry` que utiliza a imagem `registry:2` como base com a opção `--restart=always` (se ocorrer um problema com o container, vai reiniciar automaticamente). A porta de comunicação do container e a porta do host é `5000`

    Aqui o registry já está em execução
    ```
    docker container ls
    docker image ls
    ```

2. Testar o registry, fazendo push da imagem para ele
    É preciso incluir o endereço do novo registry em vez do nome de usuário
    ```
    docker tag luizakuze/apache:1.0 localhost:5000/apache
    docker push localhost:5000/apache
    ```

Esse é um registry local bem simples, na documentação oficial é possível visualizar novas funcionalidades.