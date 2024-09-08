# Docker Swarm

- Para construir clusters (conjunto de computadores ou servidores que trabalham juntos como se fossem um único sistema) de containers com características como balanceador de cargas e failover.

- Sua estrutura é simples: um `Manager` e diversos `Workers`. O Manager é responsável por organizar os containers e distribuí-los entre os hosts workers que hospedam os containers.