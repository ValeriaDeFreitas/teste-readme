# Documentação Blog - Sistema Imobiliária Bortone

## Visão Geral
Sistema de Blog integrado ao projeto imobiliário, permitindo a publicação e gerenciamento de artigos relacionados ao mercado imobiliário, dicas, notícias e atualizações.

---

## API Endpoints

### **GET /api/blog/posts**
- **Descrição:** Lista todos os posts do blog com filtros opcionais.  
- **Método:** `GET`  
- **URL:** `/api/blog/posts`  

**Parâmetros opcionais:**
- `categoria`: filtrar por categoria (ex: `"mercado"`, `"dicas"`, `"noticias"`)  
- `tag`: filtrar por tag (ex: `"financiamento"`, `"aluguel"`, `"venda"`)  
- `busca`: buscar por palavra-chave no título ou conteúdo  
- `pagina`: número da página para paginação  
- `limite`: número de itens por página (padrão: 10)  

**Exemplo de Requisição:**
```bash
GET /api/blog/posts?categoria=mercado&tag=financiamento&busca=taxa&pagina=1&limite=5
```

**Exemplo de Resposta (200):**
```json
{
  "total": 25,
  "pagina": 1,
  "limite": 5,
  "itens": [
    {
      "id": 1,
      "titulo": "Como as taxas de juros afetam o mercado imobiliário?",
      "conteudo": "As taxas de juros são um fator crucial...",
      "resumo": "Análise do impacto das taxas de juros no mercado de imóveis",
      "categoria": "mercado",
      "tags": ["financiamento", "juros"],
      "autor": "João Silva",
      "dataPublicacao": "2024-01-15T10:30:00Z",
      "ativo": true
    }
  ]
}
```

**Códigos de Status:** `200`, `400`, `500`  
**Autenticação:** não requer  

---

### **GET /api/blog/posts/:id**
- **Descrição:** Busca um post específico por ID.  
- **Método:** `GET`  
- **URL:** `/api/blog/posts/:id`  

**Parâmetros:**
- `id` (obrigatório): ID do post  

**Exemplo de Requisição:**
```bash
GET /api/blog/posts/1
```

**Exemplo de Resposta (200):**
```json
{
  "id": 1,
  "titulo": "Como as taxas de juros afetam o mercado imobiliário?",
  "conteudo": "As taxas de juros são um fator crucial...",
  "resumo": "Análise do impacto das taxas de juros no mercado de imóveis",
  "categoria": "mercado",
  "tags": ["financiamento", "juros"],
  "autor": "João Silva",
  "dataPublicacao": "2024-01-15T10:30:00Z",
  "ativo": true
}
```

**Códigos de Status:** `200`, `404`, `500`  
**Autenticação:** não requer  

---

### **POST /api/blog/posts**
- **Descrição:** Cria um novo post no blog.  
- **Método:** `POST`  
- **URL:** `/api/blog/posts`  

**Body (JSON):**
```json
{
  "titulo": "string (obrigatório)",
  "conteudo": "string (obrigatório)",
  "resumo": "string (obrigatório)",
  "categoria": "string (obrigatório)",
  "tags": "array (opcional)",
  "autor": "string (obrigatório)"
}
```

**Exemplo de Requisição:**
```bash
POST /api/blog/posts
Content-Type: application/json

{
  "titulo": "Novas tendências no mercado imobiliário em 2024",
  "conteudo": "O mercado imobiliário está passando por transformações...",
  "resumo": "Confira as principais tendências para 2024",
  "categoria": "mercado",
  "tags": ["tendências", "2024"],
  "autor": "Maria Souza"
}
```

**Exemplo de Resposta (201):**
```json
{
  "id": 15,
  "titulo": "Novas tendências no mercado imobiliário em 2024",
  "conteudo": "O mercado imobiliário está passando por transformações...",
  "resumo": "Confira as principais tendências para 2024",
  "categoria": "mercado",
  "tags": ["tendências", "2024"],
  "autor": "Maria Souza",
  "dataPublicacao": "2024-09-13T14:25:00Z",
  "ativo": true
}
```

**Códigos de Status:** `201`, `400`, `500`  
**Autenticação:** requer token de administrador  

---

### **PUT /api/blog/posts/:id**
- **Descrição:** Atualiza um post existente.  
- **Método:** `PUT`  
- **URL:** `/api/blog/posts/:id`  

**Parâmetros:**
- `id` (obrigatório): ID do post  

**Body (JSON):**
```json
{
  "titulo": "string (opcional)",
  "conteudo": "string (opcional)",
  "resumo": "string (opcional)",
  "categoria": "string (opcional)",
  "tags": "array (opcional)",
  "autor": "string (opcional)",
  "ativo": "boolean (opcional)"
}
```

**Exemplo de Requisição:**
```bash
PUT /api/blog/posts/1
Content-Type: application/json

{
  "titulo": "Título atualizado",
  "conteudo": "Conteúdo atualizado...",
  "ativo": true
}
```

**Códigos de Status:** `200`, `400`, `404`, `500`  
**Autenticação:** requer token de administrador  

---

### **DELETE /api/blog/posts/:id**
- **Descrição:** Remove um post do blog.  
- **Método:** `DELETE`  
- **URL:** `/api/blog/posts/:id`  

**Parâmetros:**
- `id` (obrigatório): ID do post  

**Códigos de Status:** `204`, `404`, `500`  
**Autenticação:** requer token de administrador  

---

## Middleware de Validação

### **Validação de Post**
Localização: `back-end/src/middleware/blog-validation.js`  

**Regras para título:**
- Campo obrigatório para criação/atualização  
- Mínimo de 10 caracteres  
- Máximo de 200 caracteres  
- Não pode conter apenas espaços em branco  

**Regras para conteúdo:**
- Campo obrigatório para criação/atualização  
- Mínimo de 100 caracteres  
- Máximo de 10000 caracteres  
- Não pode conter apenas espaços em branco  

**Regras para resumo:**
- Campo obrigatório para criação/atualização  
- Mínimo de 50 caracteres  
- Máximo de 500 caracteres  
- Não pode conter apenas espaços em branco  

### **Validação de Categoria**
Categorias permitidas:
- `mercado`
- `dicas`
- `noticias`
- `financiamento`
- `documentacao`
- `geral`

---

## Tratamento de Erros

### **Erro 400 - Bad Request**
Retornado quando:
- Dados de entrada inválidos  
- Campos obrigatórios ausentes  
- Formato de dados incorreto  

**Exemplo de Resposta:**
```json
{
  "erro": "Dados inválidos",
  "detalhes": [
    "Campo 'titulo' é obrigatório",
    "Campo 'categoria' deve ser um dos valores permitidos"
  ]
}
```

### **Erro 404 - Not Found**
Retornado quando:
- Post não encontrado pelo ID  
- Rota não existente  

**Exemplo de Resposta:**
```json
{
  "erro": "Post não encontrado",
  "detalhes": "Não foi possível encontrar um post com o ID fornecido"
}
```

### **Erro 500 - Internal Server Error**
Retornado quando:
- Erro interno do servidor  
- Falha na conexão com banco de dados  
- Erro não tratado  

**Exemplo de Resposta:**
```json
{
  "erro": "Erro interno do servidor",
  "detalhes": "Ocorreu um erro inesperado. Tente novamente mais tarde."
}
```

---

## Guia de Uso - Frontend

### **Como Acessar o Blog**
- **Navegação Principal:**
  - Acesse o menu principal do site  
  - Clique em **"Blog"**  
  - Você será redirecionado para `/blog`  

- **Link Direto:**  
  - Use a URL direta: `https://seusite.com/blog`  

### **Como Ler um Post**
- **Para Usuários Comuns:**
  - Acesse a página do blog  
  - Use os filtros por categoria ou tags  
  - Use a barra de busca para encontrar posts específicos  
  - Clique no título do post para abrir a página completa  
  - Leia o conteúdo e compartilhe se desejar  

- **Para Administradores:**
  - Faça login como administrador  
  - Acesse `/admin/blog`  
  - Clique em **"Novo Post"**  
  - Preencha os campos obrigatórios  
  - Selecione a categoria e adicione tags  
  - Salve o post  

---

## Estrutura de Arquivos
```
back-end/src/
├── routes/
│   └── blog.js              # Rotas do blog
├── middleware/
│   └── blog-validation.js   # Validações
├── controllers/
│   └── blog-controller.js   # Lógica de negócio
└── models/
    └── blog-model.js        # Modelo de dados

front-end/src/
├── pages/
│   ├── Blog.js              # Página principal do blog
│   └── BlogPost.js          # Página individual do post
├── components/
│   ├── BlogPostItem.js      # Item individual de post
│   ├── BlogSearch.js        # Componente de busca
│   ├── BlogCategories.js    # Filtro de categorias
│   └── BlogTags.js          # Filtro de tags
└── services/
    └── blog-service.js      # Serviços de API
```
