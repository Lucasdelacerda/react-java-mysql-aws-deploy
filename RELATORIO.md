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

Foi criado o arquivo `Dockerrun.aws.json` para uso em ambiente Docker multicontainer, com definicao dos containers `frontend`, `backend` e `db`.

### ECS

Foi criado o arquivo `aws/ecs-task-definition.json` como modelo de task definition Fargate, contendo os tres containers, portas e variaveis de ambiente necessarias.

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
- Elastic Beanstalk: preencher apos deploy
- ECS: preencher apos deploy

## Dificuldades encontradas e solucoes

- A aplicacao original usa proxy de desenvolvimento do React. Para ambientes de producao, foi adicionado `frontend/nginx.conf` para encaminhar `/api` ao backend.
- O deploy em AWS exige imagens publicadas em um registry. Foram preparados arquivos para usar ECR ou Docker Hub.

## Evidencias visuais e tecnicas

Adicionar prints das seguintes evidencias:

- Aplicacao aberta no navegador.
- Containers em execucao.
- Logs de deploy ou execucao.
- Security group com porta 3000 liberada.
- EC2 ativa.
- Elastic Beanstalk com status saudavel.
- ECS com cluster, task definition, service e task em execucao.

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
