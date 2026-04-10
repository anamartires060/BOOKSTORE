# BOOKSTORE

## 🔹 Middleware no Express

Middleware no **Express** são funções que ficam entre a requisição (request) e a resposta (response). Eles interceptam e processam os dados antes que cheguem na rota final.

###  Por que são fundamentais para JSON (mobile)?

Aplicativos mobile (como React Native) geralmente enviam dados em formato JSON. O Express, por padrão, não entende JSON automaticamente. Por isso usamos middleware como:

```js
app.use(express.json());
```

Esse middleware:

* Interpreta o JSON enviado no body da requisição
* Converte para objeto JavaScript
* Permite acessar via `req.body`

Sem isso, o backend não conseguiria ler dados enviados pelo app.

---

##  NoSQL vs SQL (Catálogo de Livros)

###  SQL (Tradicional)

* Estrutura fixa (tabelas e colunas)
* Precisa de schema definido
* Alterações estruturais são mais difíceis
* Exemplo:

  * tabela `livros`: id, titulo, autor, ano

###  NoSQL (Documentos – MongoDB)

* Estrutura flexível (JSON)
* Não precisa schema rígido
* Permite diferentes formatos no mesmo banco
* Ideal para mudanças constantes

###  Exemplo (MongoDB):

```json
{
  "titulo": "Livro X",
  "autor": "Autor Y",
  "genero": ["ficção", "aventura"],
  "avaliacao": 4.5
}
```

###  Conclusão

Para um catálogo que muda muito:

* **NoSQL é melhor**, pois permite adicionar campos novos sem quebrar o sistema.

---

## 🔹 Hashing com bcryptjs

Hashing é o processo de transformar uma senha em uma string criptografada irreversível.

###  Exemplo:

```js
const bcrypt = require('bcryptjs');

const senhaHash = await bcrypt.hash(senha, 10);
```

###  Por que protege?

* Senhas não ficam salvas em texto puro
* Mesmo se o banco vazar, ninguém vê a senha real
* Para login, usamos:

```js
bcrypt.compare(senhaDigitada, senhaHash);
```

###  Segurança

* Dificulta ataques
* Protege usuários em caso de vazamento

---

##  .env e dotenv (Segurança)

###  O que é `.env`?

Arquivo que guarda variáveis sensíveis:

```
DB_PASSWORD=123456
JWT_SECRET=abc123
```

###  dotenv

Carrega essas variáveis no projeto:

```js
require('dotenv').config();
```

###  Importância:

* Evita expor dados no código
* Protege credenciais no GitHub
* Facilita configuração em produção

###  Nunca fazer:

* Subir `.env` para o GitHub

Use `.gitignore`:

```
.env
```

---

##  Workflow de Signup (React Native + Express + MongoDB)

###  1. Usuário preenche formulário (React Native)

Campos:

* username
* email
* password

###  2. Envio da requisição

```js
fetch('http://localhost:3000/signup', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    username,
    email,
    password
  })
});
```

---

###  3. Backend (Express)

#### Middleware

```js
app.use(express.json());
```

#### Rota

```js
app.post('/signup', async (req, res) => {
  const { username, email, password } = req.body;

  const senhaHash = await bcrypt.hash(password, 10);

  const user = await User.create({
    username,
    email,
    password: senhaHash
  });

  res.status(201).json(user);
});
```

---

###  4. Banco (MongoDB)

Documento salvo:

```json
{
  "username": "natha",
  "email": "email@email.com",
  "password": "$2a$10$HASH..."
}
```

---

###  Fluxo resumido

1. Usuário preenche formulário
2. React Native envia JSON
3. Express (middleware) interpreta
4. bcrypt faz hash da senha
5. MongoDB salva usuário
6. Backend retorna resposta

---

##  Conclusão

* Middleware permite processar dados corretamente
* NoSQL é ideal para estruturas flexíveis
* Hashing protege senhas
* `.env` mantém segurança das credenciais
* Workflow conecta mobile → backend → banco de forma segura

---
