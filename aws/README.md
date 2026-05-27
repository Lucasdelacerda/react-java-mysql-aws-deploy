# AWS deployment notes

Este diretório contem os modelos usados para evidenciar as tres modalidades pedidas no enunciado.

## EC2

1. Criar uma instancia EC2 Linux com Docker e Docker Compose instalados.
2. Liberar a porta 3000 no security group.
3. Clonar este repositorio na instancia.
4. Na pasta `react-java-mysql`, executar:

```bash
docker compose -f compose.prod.yaml up -d --build
docker compose -f compose.prod.yaml ps
docker compose -f compose.prod.yaml logs --tail=100
```

5. Acessar `http://IP_PUBLICO_DA_EC2:3000`.

## Elastic Beanstalk

1. Publicar as imagens `frontend` e `backend` em um registry como Amazon ECR ou Docker Hub.
2. Trocar `SEU_REGISTRY` no `Dockerrun.aws.json` pelas imagens publicadas.
3. Criar um ambiente Elastic Beanstalk do tipo Docker multicontainer.
4. Enviar o `Dockerrun.aws.json`.
5. Conferir status saudavel e acessar a URL publica na porta 3000.

## ECS

1. Publicar as imagens `frontend` e `backend` no Amazon ECR.
2. Substituir `ACCOUNT_ID` e `REGION` em `ecs-task-definition.json`.
3. Criar cluster ECS com Fargate.
4. Registrar a task definition.
5. Criar service usando subnets publicas, public IP habilitado e security group liberando a porta 3000.
6. Conferir task em execucao e acessar o endpoint publico.
