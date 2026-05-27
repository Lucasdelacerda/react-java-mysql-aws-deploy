# React Java MySQL AWS Deploy

Projeto individual baseado no exemplo `react-java-mysql` do Docker Awesome Compose.

## Aplicacao

- Frontend: React
- Backend: Java Spring Boot
- Banco de dados: MariaDB compativel com MySQL
- Porta publica da aplicacao: 3000

## Deploys realizados

- EC2: http://3.18.106.130:3000
- Elastic Beanstalk: http://react-java-mysql-eb-env.eba-5ebnn3ch.us-east-2.elasticbeanstalk.com
- ECS/Fargate: http://18.218.116.212:3000

## Entrega

O relatorio individual esta em `RELATORIO.md`.

As evidencias visuais e tecnicas estao na pasta `aws/`.

## Teste esperado

Ao abrir qualquer uma das URLs publicas, a tela deve exibir:

```text
Hello from Docker
```

A rota `/api` deve retornar:

```json
{"id":1,"name":"Docker"}
```
