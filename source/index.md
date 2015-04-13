---
title: Pagamentos com celular

language_tabs:
  - xml: XML

toc_footers:
  - <a href='/Webservice-1.5/'>Integração Webservice 1.5</a>
  - <a href='/Webservice-1.5-Processamento-em-lote/'>Processamento em lote</a>

search: true
---

# Pagamentos com celular

Considerando o cenário de evolução tecnológica nos meios de pagamentos, a Cielo desenvolveu a funcionalidade de Pagamento com Celular integrada com a plataforma Cielo e-Commerce.

## Produtos, Bandeiras, Bancos e Operadoras Suportadas

A versão atual do Cielo e-Commerce Pagamento com Celular, possui suporte às seguintes Bandeiras, Bancos, produtos e Operadoras:

|Bandeira|Operadora|Emissor|Crédito à vista|Crédito parcelado loja|Crédito parcelado emissor|Débito e pré-pago|Voucher|
|--------|---------|-------|---------------|----------------------|-------------------------|-----------------|-------|
|Visa|Oi|Banco do Brasil|Sim|Sim|Sim|Sim|Não|
|MasterCard|Oi|Banco do Brasil|Sim|Sim|Sim|Não|Não|
|Visa|Claro|Alelo|Não|Não|Não|Sim|Não|

<aside class="warning">Para as transações do produto Pré-Pago deverá ser considerado o produto Débito.</aside>

## Funcionalidade de Pagamento com Celular

A funcionalidade Pagamento com Celular disponibiliza um novo modelo de captura de transações no Cielo e-Commerce, garantindo sempre os padrões PCI Compliant. Esta nova funcionalidade permite que sejam iniciadas transações de compra, via Buy Page Cielo ou Buy Page Loja, utilizando como dado de entrada um número de celular.

<aside class="notice">Apenas para as transações Pagamento com Celular, a transação é GARANTIDA, sendo o risco de fraude de responsabilidade do Emissor, pois o mesmo é o responsável pela autorização da transação</aside>

<aside class="warning">Para conclusão de uma transação é obrigatório que o Portador tenha um cartão de débito ou crédito vinculado previamente ao número do celular. Caso não possua, a transação será negada.</aside>

## Considerações Técnicas

Para as transações de pagamento com celular, o envio do XML para o Cielo e-Commerce deverá ser adequado para atender as novas especificações do schema ecommerce.xsd.

Os fluxos de cancelamentos permanecem inalterados.

Eventuais dúvidas ou procedimentos não tratados neste anexo, podem ser esclarecidas utilizando o Cielo eCommerce - Manual do Desenvolvedor ou através do Suporte Cielo e-Commerce.

# Visão geral

## Fluxo Pagamento com Celular

![Fluxo pagamento com celular](/images/fluxo-celular.png)

1. Portador acessa o site da Loja Virtual do Estabelecimento Comercial e realiza uma compra de produto ou serviço inserindo um número de Celular;
2. EC envia mensagem XML à Plataforma Cielo e-Commerce para criação de uma transação de Pagamento com Celular e recebe mensagem de retorno contendo TID (Transaction Identifier) da transação;
3. Portador recebe um SMS com dados da compra e do cartão para digitação da senha no celular;
4. Com os dados de cartão e senha, a Plataforma eCommerce seguirá com os fluxos de autorização e captura.
5. Após a transação ser autorizada, o portador recebe um SMS com a confirmação da transação.

## Regras

O layout do arquivo ecommerce.xsd foi adaptado para contemplar a nova funcionalidade Pagamento com Celular.

### Modalidades disponíveis

1. BuyPage Loja.
2. BuyPage Cielo.

No momento do retorno do TID pela Cielo, o Estabelecimento Comercial deve desenvolver uma mensagem de caráter informativo, alertando ao Portador que o mesmo receberá uma mensagem SMS, com os detalhes da transação, para a confirmação da compra. Em resposta ao SMS recebido, o Portador deverá digitar a senha do cartão no seu celular, dando prosseguimento no fluxo de compra.

### Exemplo de Mensagem:

*Seu pedido foi realizado com sucesso! Você receberá em instantes uma mensagem SMS, através de sua operadora de telefonia celular contendo os detalhes da transação. Seu pedido somente será confirmado após a digitação da senha do cartão no seu celular.*

### Mensagem de requisição

Exemplo do XML: Segue sugestão de arquivo xml para envio à plataforma e-Commerce com uma transação iniciada com Celular, com todos os campos possíveis para a versão 1.3.0 da mensagem:

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<requisicao-nova-transacao-celular versao="1.3.0" id="1ebf47b0-b51f-4833-bf46-ff7d114a0339">
   <dados-ec>
      <numero>1001734898</numero>
	  <chave>e84827130b9837473681c2787007da5914d6359947015a5cdb2b8843db0fa832</chave>
   </dados-ec>
   <dados-portador>
      <ddd>19</ddd>
      <numero>999999999</numero>
   </dados-portador>
   <dados-pedido>
      <numero>1843232188</numero>
      <valor>10000</valor>
      <moeda>986</moeda>
      <data-hora>2012-09-25T14:17:20</data-hora>
      <descricao>[origem:127.0.0.1]</descricao>
      <idioma>PT</idioma>
   </dados-pedido>
   <forma-pagamento>
      <produto>1</produto>
      <parcelas>1</parcelas>
   </forma-pagamento>
   <url-retorno>http://127.0.0.1:8080/lojaexemplo/retorno.jsp</url-retorno>
   <capturar>false</capturar>
   <campo-livre>Informações extras</campo-livre>
   <gerar-token>false</gerar-token>
</requisicao-nova-transacao-celular>
```

<aside class="notice">A loja deverá enviar o código do produto (Crédito a Vista, Parcelado ou Débito) e o número de parcelas para a Cielo na mensagem XML.</aside>

<aside class="notice">Para as transações do produto Pré-Pago deverá ser considerado o produto Débito.</aside>

### Mensagem de retorno

Exemplo do XML de retorno de uma requisição de transação celular:

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<transacao versao="1.3.0" id="d2573223-a662-4012-a1ef-f5222765636d" xmlns="http://e-commerce.cbmp.com.br" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
   <tid>100173489800035E1001</tid>
   <dados-pedido>
      <numero>1622989403</numero>
      <valor>1000</valor>
      <moeda>986</moeda>
      <data-hora>2012-11-01T10:57:48.710-02:00</data-hora>
      <descricao>[origem:127.0.0.1]</descricao>
      <idioma>PT</idioma>
      <taxa-embarque>0</taxa-embarque>
   </dados-pedido>
   <forma-pagamento>
      <bandeira xsi:nil="true"/>
      <produto>1</produto>
      <parcelas>1</parcelas>
   </forma-pagamento>
   <status>0</status>
   <url-autenticacao>http://localhost:7001/web/index.cbmp?id=14c4dba22a50273fb2f1efbffc21c7ba</url-autenticacao>
</transacao>
```

A tabela abaixo detalha as TAGS do XML que podem ser enviadas na mensagem para definir as configurações da transação Pagamento com Celular:

|Elemento|Tipo|Obrigatoriedade|Tamanho|Descrição|
|--------|----|---------------|-------|---------|
|dados-portador|n/a|n/a|Sim|Nó com os dados do Celular|
|dados-portador.ddd|N|Sim|2|Número do DDD|
|dados-portador.numero|N|Sim|8 ou 9|Número do Celular|

# Operações e Configurações

## Autorização de uma transação Pagamento com Celular

Para as transações de Pagamento com Celular, a loja não precisará enviar o xml de solicitação de autorização da transação. Todas as transações na modalidade Pagamento com Celular seguirão o fluxo de autenticação, autorização e captura automaticamente, após o envio do xml de requisição de transação.

## Autenticação Pagamento com Celular

Para garantir um maior nível de segurança tanto para o lojista quanto para o Portador, todas as transações iniciadas como Pagamento com Celular no eCommerce terão a autenticação do Portador.

O processo de autenticação é iniciado com o envio da SMS, contendo os dados do pedido, para o Portador que através da digitação da senha do cartão no celular do portador confirmará a compra, sendo que o processo será finalizado após o processamento pela Cielo.

<aside class="warning">Durante o processo de autenticação o status da transação será “Em Autenticação” e, somente, será alterada para “Autenticada” ou “Não Autenticada” após a finalização do processamento pela Cielo, quando deverá ser feita a consulta pelo Estabelecimento Comercial. <strong>O tempo máximo permitido para a conclusão do processo de Autenticação é de 60 segundos.</strong></aside>

## Captura de uma transação Pagamento com Celular

Não houve alteração quanto aos procedimentos existentes para requisição de captura de uma transação.

## Cancelamento de uma transação Pagamento com Celular

Não houve alteração quanto aos procedimentos existentes para requisição de cancelamento de uma transação.

## Nível de Segurança

<aside class="warning">Esse indicador é muito importante, pois é ele que determina as regras de Chargeback.</aside>

De modo análogo às transações com cartão, as transações de Pagamento com Celular, receberão o nó <autenticacao> no XML de resposta ao lojista, exceto na primeira resposta do Web Service da modalidade Buy Page Cielo.

A tabela abaixo fornece os detalhes dos possíveis valores que o campo ECI (Eletronic Commerce Indicator) pode assumir para as transações Pagamento com Celular:

|Resultado da autenticação|Visa|MasterCard|
|-------------------------|----|----------|
|Autenticação de Transação Pagamento com Celular efetuada através da digitação da senha do cartão diretamente no celular do Portador.|9|0|

<aside class="notice">Para Pagamento com Celular, o valor do campo ECI é populado automaticamente. Não há necessidade de estímulo pelo EC.</aside>

### Mensagem de retorno

```xml
<?xml version="1.0" encoding="ISO-8859-1" ?>
<transacao versao="1.3.0" id="c4581d04-c48d-4be8-99e1-f0bb09d41740" xmlns="http://e-commerce.cbmp.com.br">
    <tid>10017348980008AB1001</tid>
    <pan>+fknzLgGy/IEfTw3InfskCDlQLf9u/hDbAXMOZwoMf0=</pan>
    <dados-pedido>
        <numero>2082523851</numero>
        <valor>1000</valor>
        <moeda>986</moeda>
        <data-hora>2013-01-16T10:06:46.700-02:00</data-hora>
        <descricao>[origem:127.0.0.1]</descricao>
        <idioma>PT</idioma>
        <taxa-embarque>0</taxa-embarque>
    </dados-pedido>
    <forma-pagamento>
        <bandeira>mastercard</bandeira>
        <produto>1</produto>
        <parcelas>1</parcelas>
    </forma-pagamento>
    <status>5</status>
	
    <autenticacao>
        <codigo>5</codigo>
        <mensagem>Autenticada com sucesso</mensagem>
        <data-hora>2013-01-16T10:06:59.836-02:00</data-hora>
        <valor>1000</valor>
        <eci>0</eci>
    </autenticacao>
	
    <autorizacao>
        <codigo>5</codigo>
        <mensagem>Autorização negada</mensagem>
        <data-hora>2013-01-16T10:07:16.702-02:00</data-hora>
        <valor>1000</valor>
        <lr>999</lr>
        <nsu>002219</nsu>
    </autorizacao>
</transacao>
```

# Anexo

## Códigos de Resposta - LR

As mensagens de resposta da autorização são fornecidas pelo banco emissor do cartão e, portanto, podem variar de um emissor para outro. Alguns emissores podem simplesmente retornar uma mensagem genérica como: Transação Não Autorizada, não especificando o motivo.

|Código de Resposta (LR)|Descrição|Ação|Permite retentativa|
|-----------------------|---------|----|-------------------|
|55|Senha Inválida|Autenticação incorreta pelo portador.|Não|

## Erros da Mensagem XML

Os erros que podem ser apresentados na mensagem XML, através da TAG <erro>, estão dispostos a seguir:

|Código|Erro|Descrição|Ação|
|------|----|---------|----|
|062|O campo DDD não tem o tamanho esperado!|Quando DDD informado possui apenas 1 dígito.|Revisar as informações enviadas na mensagem XML frente às especificações|
|063|O campo DDD não tem o tamanho esperado!|Quando DDD informado possui mais de 2 dígitos.|Revisar as informações enviadas na mensagem XML frente às especificações|
|064|O campo DDD deve conter somente números!|Quando DDD informado possui caracteres não numéricos.|Revisar as informações enviadas na mensagem XML frente às especificações|
|065|Obrigatório o preenchimento do campo DDD!|Quando DDD não é informado (vazio).|Revisar as informações enviadas na mensagem XML frente às especificações|
|066|O campo Numero Celular não tem o tamanho esperado!|Quando Celular informado possui menos de 8 dígitos.|Revisar as informações enviadas na mensagem XML frente às especificações|
|067|O campo Numero Celular não tem o tamanho esperado!|Quando Celular informado possui mais de 9 dígitos.|Revisar as informações enviadas na mensagem XML frente às especificações|
|068|O campo Numero Celular deve conter somente números!|Quando Celular informado possui possui caracteres não
numéricos.|Revisar as informações enviadas na mensagem XML frente às especificações|
|069|Obrigatório o preenchimento do campo Numero Celular!|Quando celular não é informado (vazio).|Revisar as informações enviadas na mensagem XML frente às especificações|
|070|Não existe(m) número(s) de cartão(ões) vinculado(s) ao número de celular informado.|Quando Número Celular Informado pelo Portador na Transação, Não Possui Cartão Vinculado|Transação Cancelada – EC deverá enviar nova solicitação com um número de celular que possua um cartão vinculado|

## Erros Buy Page Cielo

Os erros e mensagens de caráter informativo que podem ser apresentados na tela da Buy Page Cielo estão dispostos a seguir:

|Erro|Descrição|
|----|---------|
|O campo DDD não tem o tamanho esperado!|Quando DDD informado possui apenas 1 dígito.|
|O campo DDD não tem o tamanho esperado!|Quando DDD informado possui mais de 2 dígitos.|
|O campo DDD deve conter somente números!|Quando DDD informado possui caracteres não numéricos.|
|Obrigatório o preenchimento do campo DDD!|Quando DDD não é informado (vazio).|
|O campo Numero Celular não tem o tamanho esperado!|Quando Celular informado possui menos de 8 dígitos.|
|O campo Numero Celular não tem o tamanho esperado!|Quando Celular informado possui mais de 9 dígitos.|
|O campo Numero Celular deve conter somente números!|Quando Celular informado possui possui caracteres não numéricos.|
|Obrigatório o preenchimento do campo Numero Celular!|Quando celular não é informado (vazio).|