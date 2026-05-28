## Identificacao do aluno

- Nome: Lucas de Lacerda
- Turma: preencher com sua turma
- Data: 26/05/2026

## Repositorio utilizado

- Projeto base: https://github.com/docker/awesome-compose/tree/master/react-java-mysql
- Repositorio/fork de entrega: https://github.com/Lucasdelacerda/react-java-mysql-aws-deploy

## Resumo da aplicacao

A aplicacao possui tres servicos conteinerizados:

- `frontend`: aplicacao React exposta na porta 3000.
- `backend`: API Java Spring Boot na porta 8080.
- `db`: banco MariaDB compativel com MySQL.

O frontend acessa `/api`, o Nginx encaminha essa rota para o backend, e o backend consulta o banco para retornar a mensagem exibida na tela.

## Adaptacoes realizadas

### EC2

Foi criado o arquivo `compose.prod.yaml` para subir os tres servicos com Docker Compose em modo de producao, expondo o frontend na porta 3000 e mantendo backend e banco na rede interna do Docker.

### Elastic Beanstalk

Foi criado um pacote Docker Compose em `aws/elastic-beanstalk/docker-compose.yml`, usando imagens publicadas no Amazon ECR. O ambiente foi criado na plataforma Docker do Elastic Beanstalk e ficou com status saudavel.

### ECS

Foi criado o arquivo `aws/ecs-task-definition.json` como task definition Fargate, contendo os tres containers, portas, variaveis de ambiente, healthcheck do banco e imagens publicadas no Amazon ECR.

### ECR

Foram criados dois repositorios no Amazon ECR:

- `929123273508.dkr.ecr.us-east-2.amazonaws.com/react-java-mysql-frontend`
- `929123273508.dkr.ecr.us-east-2.amazonaws.com/react-java-mysql-backend`

## Passos executados para implantacao

### EC2

1. Criar instancia EC2 Linux.
2. Instalar Docker e Docker Compose.
3. Liberar porta 3000 no security group.
4. Clonar o repositorio.
5. Executar `docker compose -f compose.prod.yaml up -d --build`.
6. Validar com `docker compose -f compose.prod.yaml ps`.
7. Acessar `http://IP_PUBLICO_DA_EC2:3000`.

### Elastic Beanstalk

1. Gerar imagens Docker do frontend e backend.
2. Publicar as imagens em ECR ou Docker Hub.
3. Ajustar as imagens no `Dockerrun.aws.json`.
4. Criar ambiente Elastic Beanstalk Docker multicontainer.
5. Fazer upload do pacote com `Dockerrun.aws.json`.
6. Validar status saudavel e URL publica do ambiente.

### ECS

1. Criar repositorios ECR para frontend e backend.
2. Publicar as imagens.
3. Criar cluster ECS Fargate.
4. Registrar a task definition baseada em `aws/ecs-task-definition.json`.
5. Criar service com public IP e security group liberando porta 3000.
6. Validar cluster, service, task definition e task em execucao.

## URLs publicas ou endpoints

- EC2: preencher apos deploy
- EC2: http://3.18.106.130:3000
- Elastic Beanstalk: http://react-java-mysql-eb-env.eba-5ebnn3ch.us-east-2.elasticbeanstalk.com
- ECS: http://18.218.116.212:3000

## Dificuldades encontradas e solucoes

- A aplicacao original usa proxy de desenvolvimento do React. Para ambientes de producao, foi adicionado `frontend/nginx.conf` para encaminhar `/api` ao backend.
- O deploy em AWS exige imagens publicadas em um registry. Foram preparados arquivos para usar ECR ou Docker Hub.

## Evidencias visuais e tecnicas

Os prints organizados para entrega estao na pasta `prints/`:

- `01-ec2-aplicacao-funcionando.png`
- `02-elastic-beanstalk-aplicacao-funcionando.png`
- `03-ecs-aplicacao-funcionando.png`
- `04-ec2-instancia-running.png`
- `05-ec2-security-group-porta-3000.png`
- `06-elastic-beanstalk-health-green.png`
- `07-ecs-cluster.png`
- `08-ecs-service-running.png`
- `09-ecs-task-running.png`
- `10-ecs-task-definition.png`
- `11-ecr-repositories.png`

As evidencias tecnicas exportadas estao em `aws/evidence/`.

## Teste local realizado

Ambiente testado em 26/05/2026 com Docker Compose:

```bash
docker compose -f compose.prod.yaml up -d --build
docker compose -f compose.prod.yaml ps
```

Resultado dos containers:

```text
react-java-mysql-backend-1    Up             8080/tcp
react-java-mysql-db-1         Up (healthy)   3306/tcp
react-java-mysql-frontend-1   Up             0.0.0.0:3000->3000/tcp
```

Validacoes:

- `http://localhost:3000` retornou HTTP 200.
- `http://localhost:3000/api` retornou `{"id":1,"name":"Docker"}`.
- Print local salvo em `aws/app-localhost-3000.png`.

## Teste EC2 realizado

Ambiente testado em 27/05/2026 na instancia EC2 `3.18.106.130`.

Resultado dos containers na EC2:

```text
react-java-mysql-frontend-1   Up             0.0.0.0:3000->3000/tcp
react-java-mysql-backend-1    Up             8080/tcp
react-java-mysql-db-1         Up (healthy)   3306/tcp
```

Validacoes:

- `http://3.18.106.130:3000` retornou HTTP 200.
- `http://3.18.106.130:3000/api` retornou `{"id":1,"name":"Docker"}`.
- Print da aplicacao na EC2 salvo em `aws/ec2-public-app.png`.

## Teste Elastic Beanstalk realizado

Ambiente testado em 27/05/2026:

- Aplicacao: `react-java-mysql-eb`
- Ambiente: `react-java-mysql-eb-env`
- Status: `Ready`
- Health: `Green`
- URL: `http://react-java-mysql-eb-env.eba-5ebnn3ch.us-east-2.elasticbeanstalk.com`

Validacoes:

- A URL publica retornou HTTP 200.
- A rota `/api` retornou `{"id":1,"name":"Docker"}`.
- Print salvo em `aws/elastic-beanstalk-app.png`.
- Evidencia tecnica salva em `aws/evidence/elastic-beanstalk-environment.json`.

## Teste ECS realizado

Ambiente testado em 27/05/2026:

- Cluster: `react-java-mysql-cluster`
- Service: `react-java-mysql-service`
- Task definition: `react-java-mysql:2`
- Launch type: `FARGATE`
- Task status: `RUNNING`
- Health: `HEALTHY`
- Security group: porta 3000 liberada para acesso publico.
- URL: `http://18.218.116.212:3000`

Validacoes:

- A URL publica retornou HTTP 200.
- A rota `/api` retornou `{"id":1,"name":"Docker"}`.
- Print salvo em `aws/ecs-public-app.png`.
- Evidencias tecnicas salvas em `aws/evidence/ecs-service.json` e `aws/evidence/ecs-task-definition.json`.
