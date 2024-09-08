# Docker Secrets

- Pode conter qualquer coisa, porém com o tamanho máximo de 500KB.

1. Verificando subcomandos do `docker secret`
    ```
    docker secret --help
    ```
    - `create` Cria uma secret a partir do conteúdo d eum arquivo STDIN
    - `inspect` Mostra informações de uma ou mais secrets
    - `ls` Lista secrets existentes
    - `rm` Remove uma ou mais secrets


2. Exemplo de uso dos comandos:

    - Criar uma secret a partir de um arquivo:
        ```bash
        echo "minha_secret" | docker secret create minha_secret -
        ```

    - Listar secrets existentes:
        ```bash
        docker secret ls
        ```

    - Inspecionar uma secret:
        ```bash
        docker secret inspect minha_secret
        ```

    - Atualizar uma secret em um serviço:
        ```bash
        docker service update --secret-rm minha_secret --secret-add source=minha_secret,target=minha_secret meu_servico
        ```

    - Remover uma secret:
        ```bash
        docker secret rm minha_secret
        ```