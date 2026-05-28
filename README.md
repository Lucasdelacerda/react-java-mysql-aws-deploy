# 🚀 React + Java Spring + MariaDB na AWS

Projeto individual de implantação da aplicação `react-java-mysql`, baseada no repositório **Docker Awesome Compose**.

O objetivo da atividade é demonstrar a execução da mesma aplicação conteinerizada em três modalidades diferentes da AWS: **Amazon EC2**, **AWS Elastic Beanstalk** e **Amazon ECS/Fargate**.

## 👤 Identificação

- Aluno: Lucas de Lacerda
- Projeto base: `react-java-mysql` do Docker Awesome Compose
- Região AWS utilizada: `us-east-2`

## 🧱 Arquitetura da Aplicação

A aplicação é composta por três serviços principais:

| Serviço | Tecnologia | Função |
|---|---|---|
| Frontend | React + Nginx | Interface acessível pelo navegador |
| Backend | Java Spring Boot | API responsável por consultar o banco |
| Banco de dados | MariaDB | Banco compatível com MySQL |

Fluxo da aplicação:

```text
Navegador -> React -> /api -> Spring Boot -> MariaDB -> Spring Boot -> React
```

Resultado esperado na tela:

```text
Hello from Docker
```

Resposta esperada da API:

```json
{"id":1,"name":"Docker"}
```

## 🌐 URLs Públicas

| Ambiente | URL |
|---|---|
| Amazon EC2 | http://3.18.106.130:3000 |
| Elastic Beanstalk | http://react-java-mysql-eb-env.eba-5ebnn3ch.us-east-2.elasticbeanstalk.com |
| ECS/Fargate | http://18.218.116.212:3000 |

## ☁️ Implantações Realizadas

### 1. Amazon EC2

A aplicação foi implantada em uma instância EC2 usando Docker e Docker Compose.

Evidências:

- Instância EC2 ativa.
- Security Group com porta `3000` liberada.
- Três containers em execução: `frontend`, `backend` e `db`.
- Frontend acessível externamente pela porta `3000`.

### 2. AWS Elastic Beanstalk

A aplicação foi adaptada para execução conteinerizada no Elastic Beanstalk usando Docker Compose e imagens publicadas no Amazon ECR.

Evidências:

- Ambiente Elastic Beanstalk criado.
- Status saudável.
- Aplicação acessível publicamente.
- Rota `/api` respondendo corretamente.

### 3. Amazon ECS/Fargate

A aplicação foi implantada no ECS usando Fargate, imagens no Amazon ECR, Task Definition, Service, Cluster e rede com IP público.

Evidências:

- Cluster ECS criado.
- Task Definition registrada.
- Service ativo.
- Task em execução.
- Security Group liberando acesso pela porta `3000`.

## 📦 Registry de Imagens

As imagens Docker foram publicadas no Amazon ECR:

```text
929123273508.dkr.ecr.us-east-2.amazonaws.com/react-java-mysql-frontend
929123273508.dkr.ecr.us-east-2.amazonaws.com/react-java-mysql-backend
```

## 📁 Estrutura Principal

```text
backend/              API Java Spring Boot
frontend/             Aplicação React e configuração Nginx
db/                   Arquivo de senha usado no Compose original
aws/                  Arquivos de deploy e evidências técnicas
prints/               Prints organizados para entrega
compose.yaml          Compose original
compose.prod.yaml     Compose de produção para EC2/local
Dockerrun.aws.json    Arquivo de referência para Elastic Beanstalk
RELATORIO.md          Relatório individual da atividade
```

## 🖼️ Evidências Visuais

Os prints solicitados estão na pasta `prints/`:

```text
01-ec2-aplicacao-funcionando.png
02-elastic-beanstalk-aplicacao-funcionando.png
03-ecs-aplicacao-funcionando.png
04-ec2-instancia-running.png
05-ec2-security-group-porta-3000.png
06-elastic-beanstalk-health-green.png
07-ecs-cluster.png
08-ecs-service-running.png
09-ecs-task-running.png
10-ecs-task-definition.png
11-ecr-repositories.png
```

## 🧾 Evidências Técnicas

As evidências técnicas exportadas estão em `aws/evidence/`:

```text
ec2-docker-compose-ps.txt
ecr-repositories.json
elastic-beanstalk-environment.json
ecs-service.json
ecs-task-definition.json
```

## 📄 Relatório

O relatório individual completo está em:

```text
RELATORIO.md
```

Ele contém:

- Identificação do aluno.
- Link do repositório.
- Adaptações realizadas.
- Passos executados em EC2, Elastic Beanstalk e ECS.
- URLs públicas.
- Dificuldades encontradas e soluções adotadas.
- Evidências visuais e técnicas.

## ▶️ Execução Local

Para executar localmente com Docker Compose:

```bash
docker compose -f compose.prod.yaml up -d --build
```

Para verificar os containers:

```bash
docker compose -f compose.prod.yaml ps
```

Para parar:

```bash
docker compose -f compose.prod.yaml down
```

## ✅ Status Final

- ✅ EC2 implantado e testado
- ✅ Elastic Beanstalk implantado e testado
- ✅ ECS/Fargate implantado e testado
- ✅ Imagens publicadas no Amazon ECR
- ✅ Prints organizados
- ✅ Evidências técnicas salvas
- ✅ Relatório individual preenchido
