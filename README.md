# API DE CRUD SIMPLES COM DJANGO REST FRAMEWORK
[Django REST framework](http://www.django-rest-framework.org/) é um kit de ferramentas poderosas e flexíveis para criar APIs da Web.

## Exigências
- Python 3.6
- Django (2.1)
- Django REST Framework
- Django Rest Auth

## Instalação
```
	pip install django
	pip install djangorestframework
	pip install django-rest-auth
	pip install django-allauth
```

## Estrutura
Em uma API RESTful, os terminais (URLs) definem a estrutura da API e os usuários acessam dados de nosso aplicativo usando os métodos HTTP - GET, POST, PUT, DELETE. Os pontos de fixação devem ser organizados logicamente em torno de coleções e elementos , quais são os recursos.

No nosso caso, como temos um único recurso, moviesusaremos os seguintes URLS - /movies/e /movies/<id>para coleções e elementos, respectivamente:

Endpoint |Método HTTP | Método CRUD | Resultado
-- | -- |-- |--
`movies` | GET | LER | Obter todos os filmes
`movies/:id` | GET | LER | Obter um único filme
`movies`| POST | CRIAR | Criar um novo filme
`movies/:id` | PUT | ATUALIZAR | Atualizar um filme
`movies/:id` | DELETE | EXCLUIR | Excluir um filme

## Usar
Podemos testar a API usando [curl](https://curl.haxx.se/) ou [httpie](https://github.com/jakubroztocil/httpie#installation). Httpie é um cliente http amigável, escrito em Python. Vamos instalar isso.

Você pode instalar o httpie usando o pip:
```
pip install httpie
```

Primeiro, temos que iniciar o servidor de desenvolvimento do Django.
```
	python manage.py runserver
```
Somente usuários autenticados podem usar os serviços da API, por esse motivo, se tentar o seguinte:
```
	http  http://127.0.0.1:8000/api/v1/movies/3
```
Nós temos:
```
 {  "detail":  "Você deve estar autenticado!"  }
```
Em vez disso, tente acessar com credenciais:
```
	http http://127.0.0.1:8000/api/v1/movies/3 "Authorization: Token 7530ec9186a31a5b3dd8d03d84e34f80941391e3"
```
nós conseguimos o filme com id = 3
```
{  "title":  "Avengers",  "genre":  "Superheroes",  "year":  2012,  "creator":  "admin"  }
```

## Login e Tokens

Para obter um token primeiro, precisamos fazer o login
```
	http http://127.0.0.1:8000/rest-auth/login/ username="admin" password="root1234"
```
depois disso, obter ou token
```
{
    "key": "fb448767999dfc09d283bdbf7a4b6070309df295"
}
```
**TODAS as solicitações devem ser autenticadas com um token válido, caso contrário não será permitido**

Nós podemos criar novos usuários. (senha1 e senha2 devem ser iguais)
```
http POST http://127.0.0.1:8000/rest-auth/registration/ username="USERNAME" password1="PASSWORD" password2="PASSWORD"
```
E podemos sair, o token deve ser o seu token real
```
http POST http://127.0.0.1:8000/rest-auth/logout/ "Authorization: Token <YOUR_TOKEN>" 
```

A API tem algumas restrições:
-   Os filmes são sempre associados a um criador (usuário que criou).
-   Somente usuários autenticados podem criar e ver filmes.
-   Somente o criador de um filme pode atualizá-lo ou excluí-lo.
-   Solicitações não autenticadas não devem ter acesso.

### Comandos
```
http http://127.0.0.1:8000/api/v1/movies/ "Authorization: Token <YOUR_TOKEN>"
http GET http://127.0.0.1:8000/api/v1/movies/3 "Authorization: Token <YOUR_TOKEN>"
http POST http://127.0.0.1:8000/api/v1/movies/ "Authorization: Token <YOUR_TOKEN>" title="Ant Man and The Wasp" genre="Action" year=2018
http PUT http://127.0.0.1:8000/api/v1/movies/3 "Authorization: Token <YOUR_TOKEN>" title="AntMan and The Wasp" genre="Action" year=2018
http DELETE http://127.0.0.1:8000/api/v1/movies/3 "Authorization: Token <YOUR_TOKEN>"
```

### Paginação
Uma API suporta paginação, por padrão, como respostas têm page_size = 10, mas se você desejar alterar, poderá passar por configurações page = size = X
```
http http://127.0.0.1:8000/api/v1/movies/?page=1 "Authorization: Token <YOUR_TOKEN>"
http http://127.0.0.1:8000/api/v1/movies/?page=3 "Authorization: Token <YOUR_TOKEN>"
http http://127.0.0.1:8000/api/v1/movies/?page=3&page_size=15 "Authorization: Token <YOUR_TOKEN>"
```

Por fim, forneça um banco de dados para fazer esses testes.

