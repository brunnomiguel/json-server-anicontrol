<h1 align="center">Json-server-aniControl</h1>

<p>
    Esse é o repositório criado utilizando JSON-SERVER e JSON-SERVER-AUTH, tendo como principal objetivo ser a base do projeto AniControl, criado para pessoas fãs de animes que desejam controlar melhor o que assistem, quando e como assistem.
</p>

##

<h2 align="center">Endpoints</h2>

<p>
    Assim como a documentação do JSON-Server-Auth traz (https://www.npmjs.com/package/json-server-auth), existem 3 endpoints que podem ser utilizados para cadastro e 2 endpoints que podem ser usados para login. Além dos endpoints citados, foi criado o endpoint animesList, que poderá receber diversos dados e terá como objetivo base controlar as requisições do usuário como será mostrado mais abaixo.
</p>

<p>
    A API possui o seguinte URL base:
</p>

```
https://json-server-anicontrol.herokuapp.com
```

---

### **Rotas que precisam de autenticação**

<h2> Sobre autenticação </h2>

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}

Sendo que o token, é o campo `accessToken` disponibilizado na resposta do endpoint /sessions/

Caso você tente acessar qualquer endpoint que necessite de autorização, sem passar o token, irá receber o seguinte erro:

`STATUS 401`

```json
{
  "Private resource access: entity must have a reference to the owner id"
}
```

---

## **USER**

O endpoint de users serve para controlar o fluxo dos usuários realizando login, registo e listagem dos próprios.

> A API é volátil, então registros são apagados após 24hrs necessitando um novo registro.

<h2 align ='center'> Listando usuários </h2>

### **USERS**:

`GET /users/:user_id - FORMATO DA RESPOSTA - STATUS 200`

Esta rota serve primariamente para listagem das informações do usuário registrado na API, respondendo todos em um formato de JSON:

```JSON
{
      "email": "kenzinho@mail.com",
      "password": "$2a$10$YQiiz0ANVwIgpOjYXPxc0O9H2XeX3m8OoY1xk7OGgxTnOJnsZU7FO",
      "name": "Kenzinho",
      "age": 38,
      "id": 1
    }
```

Esta rota também está protegida contra acessos não autorizados, necessitando o envio do ID do usuário para verificação e acesso, caso não seja provido a API irá retornar:

`GET /users - FORMATO DA RESPOSTA - STATUS 403`

```JSON
"Private resource access: entity must have a reference to the owner id"
```

<h2 align="center">Cadastrando Usuário</h2>

<p>
    Qualquer um desses 3 endpoints irá cadastrar o usuário na lista de "Users", sendo que os campos obrigatórios são os de email e password.
    Você pode ficar a vontade para adicionar qualquer outra propriedade no corpo do cadastro dos usuários.
</p>

<ul>
    <li>POST /register</li>
    <li>POST /signup</li>
    <li>POST /users</li>
</ul>

A rota de registro é usada primariamente para o registro de um novo usuario requerindo as seguintes informações:

```JSON
"email": "superEmail@mail.com",
"password": "senhaSuperSecreta123",
```

Caso o registro seja bem sucedido será respondido com a resposta:

`POST /register - FORMATO DA RESPOSTA - STATUS 201`

```JSON
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImtlbnppbmhvYUBtYWlsLmNvbSIsImlhdCI6MTY2MDY4ODc1MiwiZXhwIjoxNjYwNjkyMzUyLCJzdWIiOiIzIn0.01HegRe_ewNT4AVjqwyaOyJL5ZyafwJ0Gf7p1vjlWv4",
	"user": {
		"email": "kenzinhoa@mail.com",
		"id": 3
	}
}
```

A API possui a proteção para senhas curtas, requirindo uma senha de no minimo 4 caractéres, como também possuindo a proteção contra email já registrados:

Caso já esteja registrado:

```JSON
"Email already exists"
```

Caso a senha seja menor que 4 caractéres:

```JSON
"Password is too short"
```

<br>

---

### **LOGIN**:

<ul>
    <li>POST /login</li>
    <li>POST /signin</li>
</ul>

<p>
    Qualquer um desses 2 endpoints pode ser usado para realizar login com um dos usuários cadastrados na lista de "Users"
</p>

A rota de login possui o mesmo endpoint para o uso de acessar as outras rotas da API, podemos realizar o login enviando uma requisição POST com email e senha para a rota:

`POST /login - FORMATO DE REQUISIÇÃO - STATUS 200`

```JSON
{
	"email": "kenzinhodsa@mail.com",
	"password": "146d"
}
```

Caso tudo ocorra como esperado a API irá responder com:

`POST /login - FORMATO DE REQUISIÇÃO - STATUS 200`

```JSON
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImtlbnppbmhvZHNhQG1haWwuY29tIiwiaWF0IjoxNjYwNjg5MDc5LCJleHAiOjE2NjA2OTI2NzksInN1YiI6IjUifQ.DpNxS-T4QXzLFS0Won5VQklES9_hq4KT9Kesc5K2D3I",
	"user": {
		"email": "kenzinhodsa@mail.com",
		"id": 5
	}
}
```

---

## **ANIMES LIST**

<p>
    Endpoint específico para receber dados de animes que poderão ser adicionados e controlados pelo usuário. Existirão basicamente quatro endpoints para requisições, conforme segue abaixo:
</p>

<h2 align="center">Listando Animes</h3>

<p>
    Essa rota tem como principal funcionalidade, requisitar a lista completa de animes vinculados ao id do usuário em questão, sendo necessário o envio do id do usuário a cada requisição, para trazer apenas os animes do usuário, utilize o query param `?user_id={idDoUsuário}`
</p>

Caso a requisição seja bem sucessida:

`GET /animesList/?user_id={idDoUsuário} FORMATO DA REQUISIÇÃO - STATUS 200`

```JSON
[
	{
		"anime": {},
		"status": "completed",
		"rating": 10,
		"review": "",
		"userId": 3,
		"id": 1
	},
	{
		"anime": {},
		"status": "completed",
		"rating": 10,
		"review": "",
		"userId": 3,
		"id": 2
	},
	{
		"anime": {},
		"status": "completed",
		"rating": 10,
		"review": "",
		"userId": 3,
		"id": 3
	},
	{
		"anime": {},
		"status": "completed",
		"rating": 10,
		"review": "",
		"userId": 3,
		"id": 4
	},
	{
		"anime": {},
		"status": "completed",
		"rating": 10,
		"review": "",
		"userId": 3,
		"id": 5
	}
]
```

<h2 align="center">Adicionando Animes</h3>

<p>
    Este endpoint foi construido com o objetivo de adicionar novos animes para a lista do usuário, recebendo as seguintes informações em sua requisição:
</p>

> Todas as requisições OBRIGATORIAMENTE necessitam receber o id do usuário para co-relacionar ambos

`POST /animesList/ FORMATO DA REQUISIÇÃO - STATUS 201`

```JSON
{
	"anime": {},
	"status": "completed",
	"rating": 10,
	"review": "",
	"userId": 3
}
```

<p>Em caso de sucesso a resposta será:</p>

```JSON
{
	"anime": {},
	"status": "completed",
	"rating": 10,
	"review": "",
	"userId": 3,
	"id": 5
}
```

<h2 align="center">Editando Animes</h3>

<p>
    Assim como a base de todas as requisições, esta rota existe para a principal funcionalidade editar o alguma informação do anime do usuário,
    especificamente construida para alterar os campos `rating` , `review` e `status`:
</p>

`PATCH /animesList/:anime_id - FORMATO DA REQUISIÇÃO - STATUS 200`

```JSON
{
	"anime": {},
	"status": "incompleted",
	"rating": 20,
	"review": "iviwubgwrjgweivnewoivbew",
	"userId": 3
}
```

<p>Sendo esperado o mesmo objeto com as informações atualizadas com uma resposta status 200</p>

<h2 align="center">Removendo Animes</h3>

Assim como a base de todas as requisições, esta rota existe para a principal funcionalidade remover um anime da lista do usuário

`DELETE /animesList/:anime_id - FORMATO ESPERADO DE RESPOSTA - STATUS 200`

```JSON
{}
```

---

Feito com ♥ by <a href="https://github.com/brunnomiguel">**Brunno Miguel**</a> && <a href="https://github.com/nittts">**Nittts**</a> 👋
