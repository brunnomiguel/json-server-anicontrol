<h1 align="center">Json-server-aniControl</h1>

##

<p>
    Esse é o repositório criado utilizando JSON-SERVER e JSON-SERVER-AUTH, tendo como principal objetivo ser a base do projeto AniControl, criado para pessoas fãs de animes que desejam controlar melhor o que assistem, quando e como assistem.
</p>

##

<h2 align="center">Endpoints</h2>

<p>
    Assim como a documentação do JSON-Server-Auth traz (https://www.npmjs.com/package/json-server-auth), existem 3 endpoints que podem ser utilizados para cadastro e 2 endpoints que podem ser usados para login. Além dos endpoints citados, foi criado o endpoint animesList, que poderá receber diversos dados como será mostrado mais abaixo.
</p>

<h3>Cadastro</h3>

<ul>
    <li>POST /register</li>
    <li>POST /signup</li>
    <li>POST /users</li>
</ul>

<p>
    Qualquer um desses 3 endpoints irá cadastrar o usuário na lista de "Users", sendo que os campos obrigatórios são os de email e password.
    Você pode ficar a vontade para adicionar qualquer outra propriedade no corpo do cadastro dos usuários.
</p>

<h3>Login</h3>

<ul>
    <li>POST /login</li>
    <li>POST /signin</li>
</ul>

<p>
    Qualquer um desses 2 endpoints pode ser usado para realizar login com um dos usuários cadastrados na lista de "Users"
</p>
