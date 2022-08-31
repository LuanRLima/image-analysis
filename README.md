<!--
title: 'API para analisar imagens com AWS Rekognition'
description: 'Uma API HTTP Serverless com lambda para analisar imagens com AWS Rekognition e taduzimos sua resposta para o portugues com AWS translate'
layout: Doc
framework: v3
platform: AWS
language: nodeJS
authorLink: 'https://github.com/LuanRLima'
authorName: 'Luan Rodrigues Lima.'
-->

# Serverless Framework Node HTTP API com Rekognition e Translate na AWS

Este projeto demonstra como fazer uma API HTTP com Node.js em execução no AWS Lambda API Gateway, Rekognition e Translate usando o Serverless Framework para identificar as imagens e traduzir sua reposta usamos o jest para fazer os testes de integração.

## Uso
E necessário a instalação do aws cli : https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

O guia para configurar o aws cli https://docs.aws.amazon.com/pt_br/cli/latest/userguide/cli-configure-quickstart.html

Para instalar todas as dependências dentro da /demo04-image-analysis/image-analysis execute o comando  a baixo para baixar as dependências do projeto
```
npm install
```
(Opcional) Instalar o ntl, ele vai lhe ajudar a rodar os scripts do package.json:
```
npm install -g ntl
```
### Deployment

```
sls deploy
```

Após a implantação, você deverá ver uma saída semelhante a:

```bash
Deploying image-analysis to stage dev (us-east-1)

✔ Service deployed to stack image-analysis-dev (89s)

endpoint: GET - https://8t217foo4m.execute-api.us-east-1.amazonaws.com/analyse
functions:
  img-analysis: image-analysis-dev-img-analysis (13 MB)
```

_Note_: Na forma atual, após a implantação, sua API é pública e pode ser invocada por qualquer pessoa. Para implantações de produção, convém configurar um autorizador. Para obter detalhes sobre como fazer isso, consulte [http event docs](https://www.serverless.com/framework/docs/providers/aws/events/apigateway/).

### Invocação

Após a implantação bem-sucedida, você pode chamar o aplicativo criado via HTTP:

```bash
curl https://8t217foo4m.execute-api.us-east-1.amazonaws.com/analyse?imageUrl={link_da_imagem}
```

O que deve resultar em uma resposta semelhante à seguinte

```json
{
    A imagem tem
  99.90% de ser do tipo Carro
  99.90% de ser do tipo Automóvel
  99.90% de ser do tipo Veículo
  99.90% de ser do tipo Transporte
  95.83% de ser do tipo Suv
}
```
Você pode invocar a função em prod usando o comando a baixo para executar local com mock o do test:
```
npm run invoke
```
Você pode invocar a função em prod usando o comando a abaixo e passando o path do arquivo json de sua preferência:
```
ls invoke -f img-analysis --path caminho/arquivo.json
```
Local development
Você pode invocar a função local usando o comando a abaixo para executar local com mock o do test:
```
npm run invoke:local
```
Você pode invocar a função local usando o comando a abaixo e passando o path do arquivo json de sua preferência
```
sls invoke local -f img-analysis --path caminho/arquivo.json
```
O que deve resultar em uma resposta semelhante à seguinte: 

```
event {
  queryStringParameters: { imageUrl: 'https://www.doglife.com.br/site/assets/images/cao.png' }
}
downloading image...
detecting labels...
translating to Portuguese...
finishing...
{
    "statusCode": 200,
    "body": "A imagem tem\n99.68% de ser do tipo Golden Retriever\n99.68% de ser do tipo cão\n99.68% de ser do tipo animal de estimação\n99.68% de ser do tipo animal\n99.68% de ser do tipo canino\n99.68% de ser do tipo mamífero"
}
```
### Test development
```
npm run test
```