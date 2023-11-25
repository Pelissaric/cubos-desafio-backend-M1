# Cubos Academy - Desafio-backend-individual-05-pdv - Pelissaric 
Criando uma versão personalizada a partir do Readme da Cubos - Obrigada!

***Atividades iniciais do Desafio M5***


![](https://i.imgur.com/xG74tOh.png)
# Desafio Backend Módulo 01
Prazo: 10/09/2023

Este desafio deve ser feito no Hackerank, com acesso via este link
[Hackerank](https://www.hackerrank.com/desafio-de-logica-modulo-1-b2b-t09-dbe-ifood)


Instrucoes:
Crie um fork deste repositorio
Clone o repositorio para sua maquina
Acesse o desafio no Hackerrank
Faca o desafio
assim que estiver feliz com sua entrega na plataforma, copie o codigo para este repositorio
Assim que acabar, abra um PR por aqui
Adicione o link do seu perfil do hackerrank
Entrega:


Sua tarefa como desenvolvedor(a) será criar uma API para um PDV (Frente de Caixa) para pequenos comércios.
O PDV deverá permitir aos funcionários do comércio, dentre outras coisas (listadas mais abaixo), cadastrarem produtos e pedidos.
**Importante 1: Sempre que a validação de uma requisição falhar, responda com código de erro e mensagem adequada à situação, ok?**
**Importante 2: Para endpoints de cadastro/atualização os objetos de requisição devem conter as propriedades equivalentes as colunas das tabelas.**
**Exemplo:**
```javascript
// Corpo da requisição para cadastro de usuário (body)
{
    "nome": "José",
    "email": "jose@email.com",
    "senha": "jose"
}
```
**ATENÇÃO: Todos os endpoints deverão atender os requisitos citados acima.**
## **Banco de dados**
Você precisa criar um Banco de Dados PostgreSQL chamado `pdv`.
**IMPORTANTE: Deverá ser criado no projeto um arquivo .sql contendo os comandos de criação das tabelas e colunas.**
## **Requisitos obrigatórios**
-   A API a ser criada deverá acessar o banco de dados a ser criado, `pdv`, para persistir e manipular os dados.
-   O campo `id` das tabelas no banco de dados deve ser auto incremento, chave primária e não deve permitir edição uma vez criado.
-   Qualquer valor monetário deverá ser representado em centavos (Ex.: R$ 10,00 reais = 1000)
## **Status Codes**
Abaixo, listamos os possíveis **_status codes_** esperados como resposta da API.
```javascript
// 200 (OK) = requisição bem sucedida
// 201 (Created) = requisição bem sucedida e algo foi criado
// 204 (No Content) = requisição bem sucedida, sem conteúdo no corpo da resposta
// 400 (Bad Request) = o servidor não entendeu a requisição pois está com uma sintaxe/formato inválido
// 401 (Unauthorized) = o usuário não está autenticado (logado)
// 403 (Forbidden) = o usuário não tem permissão de acessar o recurso solicitado
// 404 (Not Found) = o servidor não pode encontrar o recurso solicitado
// 500 (Internal Server Error) = erro inesperado do servidor
```
- ## Efetuar deploy da aplicação
Fazer deploy do projeto e disponibilizar a URL.
- ## Banco de Dados
Crie as seguintes tabelas e colunas abaixo: 
**ATENÇÃO! Os nomes das tabelas e das colunas a serem criados devem seguir exatamente os nomes listados abaixo.**
-   usuarios
    -   id (chave primária)
    -   nome
    -   email (campo único)
    -   senha
    
-   produtos
    -   id (chave primária)
    -   descricao
    -   valor
    - 	produto_imagem
    
-   pedidos
    -   id (chave primária)
    -   data (valor padrão: momento da inserção do dado)
    -   valor_total
    
-   pedido_produtos
    -   id (chave primária)
    -   pedido_id
    -   produto_id
    -   quantidade_produto
- ## Cadastrar usuário
#### `POST` `/usuario`
Essa é a rota que será utilizada para cadastrar um novo usuário no sistema.
Após o cadastro, o usuário deve receber um e-mail de boas-vindas.
Critérios de aceite:
    
    - Validar os campos obrigatórios: 
        - nome
        - email
        - senha
    - A senha deve ser criptografada utilizando algum algoritmo de criptografia confiável.
    - O campo e-mail no banco de dados deve ser único para cada registro, não permitindo dois usuários possuírem o mesmo e-mail.
    - Deverá ser enviado ao usuário um e-mail de boas-vindas.
- ## Efetuar login do usuário
#### `POST` `/login`
Essa é a rota que permite o usuário cadastrado realizar o login no sistema.
Critérios de aceite:
    - Validar se o e-mail e a senha estão corretos para o usuário em questão.
    - Gerar um token de autenticação para o usuário.
---
## **ATENÇÃO**: Todas as funcionalidades (endpoints) a seguir, a partir desse ponto, deverão exigir o token de autenticação do usuário logado, recebendo no header com o formato Bearer Token. Portanto, em cada funcionalidade será necessário validar o token informado.
---
- ## Cadastrar Produto
#### `POST` `/produto`
Essa é a rota que permite o usuário logado cadastrar um novo produto no sistema.
Deverá ser permitido vincular uma imagem a um produto pela coluna `produto_imagem`.
Critérios de aceite:
    -   Validar os campos obrigatórios:
        -   descricao
        -   valor
    - O campo produto_imagem deve ser opcional. Caso enviado no corpo da requisição, deveremos processar a imagem vinculada a essa propriedade e armazenar a imagem em um servidor de armazenamento (Supabase, Blackblaze, etc...).
    - Armazenar na coluna produto_imagem a URL que possibilita visualizar a imagem que foi efetuada upload para o servidor de armazenamento.
**Lembre-se:** A URL retornada deve ser válida, ou seja, ao ser clicada deve possibilitar visualizar a imagem que foi feito *upload*.
**ATENÇÃO:** Abaixo encontra-se um exemplo de resposta retornada a uma requisição para esse endpoint com uma URL fictícia, ilustrando o que o serviço de armazenamento da Blackblaze retornaria após *upload* efetuado com sucesso. Portanto, essa seria, no caso, a URL que armazaremos na coluna `produto_imagem` no banco de dados.
```javascript
// Resposta cadastro/atualização de produto (body)
{
    "id": 7,		
    "descricao": "Motorola moto g9 plus",
    "valor": 15000,
    "produto_imagem": "https://s3.us-east-005.backblazeb2.com/desafio-final.jpg"
}
```
- ## Listar Produtos
#### `GET` `/produto`
Essa é a rota que será chamada quando o usuário logado quiser listar todos os produtos cadastrados.
Critérios de aceite:
    - Retornar todos os produtos cadastrados.
- ## Detalhar Produto
#### `GET` `/produto/:id`
Essa é a rota que permite ao usuário logado obter um de seus produtos cadastrados.  
Critérios de aceite:
    -   Validar se existe produto para o id enviado como parâmetro na rota.
- ## Excluir Produto por ID
#### `DELETE` `/produto/:id`
Essa é a rota que será chamada quando o usuário logado quiser excluir um de seus produtos cadastrados.  
Critérios de aceite:
	- Validar se existe produto para o id enviado como parâmetro na rota.
	- Na exclusão do produto, a imagem vinculada a ele deverá ser excluída do servidor de armazenamento.
- ## Cadastrar Pedido
#### `POST` `/pedido`
Essa é a rota que será utilizada para cadastrar um novo pedido no sistema.
**Lembre-se:** Cada pedido deverá conter ao menos um produto vinculado.
**Atenção:** As propriedades `produto_id` e `quantidade_produto` devem ser informadas dentro de um array, e para cada produto deverá ser criado um objeto neste array, como ilustrado no objeto de requisição abaixo.
Só deverá ser cadastrado o pedido caso todos produtos vinculados ao pedido realmente existirem no banco de dados.
```javascript
// Corpo da requisição para cadastro de pedido (body)
{
    "data": "22-12-2021",
    "pedido_produtos": [
        {
            "produto_id": 1,
            "quantidade_produto": 10
        },
        {
            "produto_id": 2,
            "quantidade_produto": 20
        }
    ]
}
```
Critérios de aceite:
    - Validar os campos obrigatórios:
	    -   produto_id
	    -   quantidade_produto
    - Validar se existe produto para cada produto_id informado dentro do array enviado no corpo (body) da requisição. O pedido deverá ser cadastrado, apenas, se todos os produtos estiverem validados.
    - Caso não seja passada a data, o pedido deve ser cadastrado com a data em que a requisição de cadastro foi feita.  
- ## Listar Pedidos
#### `GET` `/pedido`
Essa é a rota que será chamada quando o usuário logado quiser listar todos os pedidos cadastrados.
Deveremos incluir um parâmetro chamado **a_partir** do tipo *query* para que seja possível consultar pedidos feitos daquela data em diante.
```javascript
// Resposta para listagem de pedido (body)
[
    {
        "pedido": {
            "id": 1,
            "valor_total": 230010,
            "data": '22-12-1993',
        },
        "pedido_produtos": [
            {
                "id": 1,
                "quantidade_produto": 1,
                "valor_produto": 10,
                "pedido_id": 1,
                "produto_id": 1
            },
            {
                "id": 2,
                "quantidade_produto": 2,
                "valor_produto": 230000,
                "pedido_id": 1,
                "produto_id": 2
            }
        ]
    }
]
```
Critérios de aceite:
    - Lista os pedidos cadastrados da data a_partir em diante, caso esse parâmetro seja passado.
    - Listar todos os pedidos cadastrados, caso o parâmetro a_partir não seja passado.
---

## Aulas úteis:

-   [Revisão](https://aulas.cubos.academy/turma/503b31f6-db13-4a79-8c3f-132b3d44e96f/aulas/9c29ca80-51cc-4f74-86a3-d27cee05fc48)
-   [Git e fluxo de trabalho em equipe](https://aulas.cubos.academy/turma/503b31f6-db13-4a79-8c3f-132b3d44e96f/aulas/2044890a-5d35-442a-85b1-f8481589a1a9)
-   [Deploy](https://aulas.cubos.academy/turma/503b31f6-db13-4a79-8c3f-132b3d44e96f/aulas/9be7d540-8f4d-4922-9e42-663656bd2475)
-   [Envio de e-mails](https://aulas.cubos.academy/turma/503b31f6-db13-4a79-8c3f-132b3d44e96f/aulas/9b85ed35-9833-444a-a424-80d6eeeeccbc)
-   [Validações e boas práticas](https://aulas.cubos.academy/turma/503b31f6-db13-4a79-8c3f-132b3d44e96f/aulas/61394330-479c-42de-ba1c-176f712990e5)
-   [Upload de arquivos](https://aulas.cubos.academy/turma/503b31f6-db13-4a79-8c3f-132b3d44e96f/aulas/f2821d48-b7b7-486a-8158-afacb145509f)

###### tags: `back-end` `módulo 5` `nodeJS` `PostgreSQL` `API REST` `desafio`
