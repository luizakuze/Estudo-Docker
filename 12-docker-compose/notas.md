https://blog.4linux.com.br/docker-compose-explicado/

# Docker Compose

**Antes**: Cada tipo de container (aplicação, banco de dados e servidor web) era definido e gerenciado separadamente com seus próprios Dockerfiles.

**Agora**: Com o Docker Compose, você pode definir e gerenciar ambientes completos em um único arquivo de configuração no formato YAML (`docker-compose.yml`), facilitando a coordenação de múltiplos containers e serviços.

**Exemplo de Arquivo Docker Compose**:

```yaml
version: '3'  # versão do Docker Compose que está sendo utilizada

services:  # início da definição dos serviços que serão utilizados
  web:  # nome do serviço
    image: nginx:alpine  # imagem do container
    deploy:  # início da configuração de deployment
      replicas: 5  # número de réplicas desejadas para o serviço
      resources:  # configuração de recursos do container
        limits:
          cpus: "0.1"  # limite de CPU
          memory: 50M  # limite de memória
      restart_policy:  # política de reinício do container
        condition: on-failure  # reiniciar apenas se falhar
    ports:
      - "8080:80"  # mapeamento de portas entre o host e o container
    networks:  # redes às quais o serviço está conectado
      - webserver

networks:  # definição das redes que serão utilizadas
  webserver:  # nome da rede
```

**Passos para Realizar o Deploy com Docker Compose**

1. **Crie o Arquivo `docker-compose.yml`**:
    - Defina todos os serviços e suas configurações no arquivo `docker-compose.yml`.

2. **Verifique a Configuração**:
    - Antes de fazer o deploy, é uma boa prática verificar se o arquivo está correto usando:
      ```bash
      docker-compose config
      ```

3. **Realize o Deploy do Serviço**:
    - O **deploy** é o processo de criar e iniciar os serviços definidos no `docker-compose.yml` em um ambiente de swarm. Para fazer isso, execute o comando:
      ```bash
      docker stack deploy -c docker-compose.yml nome_da_stack
      ``` 

    > **Deploy**: O processo de **deploy** refere-se à implementação de uma aplicação ou serviço em um ambiente de execução, configurando todos os aspectos necessários para sua operação e disponibilidade. No contexto do Docker, o deploy envolve criar e iniciar containers, definir o número de réplicas, alocar recursos e garantir que os serviços estejam funcionando conforme esperado.

4. **Verifique o Status dos Serviços**:
    - Após o deploy, você pode verificar o status dos serviços com:
      ```bash
      docker stack services nome_da_stack
      ```

5. **Gerencie a Stack**:
    - Para remover a stack, use:
      ```bash
      docker stack rm nome_da_stack
      ```

**Vantagens do Docker Compose**

- **Simplicidade**: Permite definir e gerenciar múltiplos containers e suas interações em um único arquivo YAML.
- **Reusabilidade**: Facilita a criação de ambientes de desenvolvimento e produção consistentes.
- **Facilidade de Deploy**: Simplifica o processo de deploy com um único comando.
- **Orquestração de Containers**: Coordena a criação, atualização e remoção de containers de maneira eficiente.
