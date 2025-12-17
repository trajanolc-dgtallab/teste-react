# Teste Técnico – Frontend React Developer (shadcn/ui)

## Objetivo

Avaliar sua capacidade de:

- Desenvolver uma **interface web moderna** em **React**;
- Utilizar **Tailwind CSS** e componentes baseados em **shadcn/ui / Radix UI**;
- Integrar o frontend com uma **API RESTful**, simulando um cenário de **microsserviços** com banco relacional;
- Entregar uma solução com foco em **alta performance**, **acessibilidade** e **boa experiência do usuário (UX)**;
- Escrever **código limpo**, organizado e de fácil manutenção.

---

## Contexto do Desafio

Você irá implementar uma interface de **MiniCRM de Clientes** para uso interno da empresa.

Nesse contexto, assuma que:

- A API de clientes faz parte de um **backend em arquitetura de microsserviços**;
- Os dados são persistidos em um **banco relacional** (Oracle, MySQL ou PostgreSQL) – aqui simulados via mock;
- Seu papel é criar uma interface elegante, funcional e performática para consumir essa API.

A aplicação deve permitir:

- Listar clientes;
- Filtrar e buscar clientes;
- Visualizar detalhes de um cliente;
- Criar e editar clientes.

---

## 1. Backend (Mock API) – JSON Server em Docker

A API será mockada com **JSON Server** rodando em um container Docker, para que você possa executar em **Linux, macOS ou Windows (incluindo WSL2)**.

### 1.1. Pré-requisitos

- Docker instalado.

### 1.2. Arquivo `db.json`

Crie na raiz do seu projeto um arquivo chamado `db.json` com o conteúdo abaixo:

```json
{
  "customers": [
    {
      "id": 1,
      "name": "João Silva",
      "email": "joao.silva@example.com",
      "phone": "+55 11 99999-0001",
      "status": "active",
      "createdAt": "2024-01-10T10:00:00.000Z",
      "notes": "Cliente antigo, alto potencial de upsell."
    },
    {
      "id": 2,
      "name": "Maria Souza",
      "email": "maria.souza@example.com",
      "phone": "+55 21 98888-0002",
      "status": "inactive",
      "createdAt": "2024-02-05T14:30:00.000Z",
      "notes": "Cancelou recentemente, observar possíveis motivos."
    },
    {
      "id": 3,
      "name": "Carlos Pereira",
      "email": "carlos.pereira@example.com",
      "phone": "+55 31 97777-0003",
      "status": "active",
      "createdAt": "2024-03-01T09:15:00.000Z",
      "notes": "Interessado em novos serviços."
    }
  ]
}
```

### 1.3. Subindo o JSON Server com Docker

No diretório onde está o `db.json`, execute:

#### Linux / macOS / WSL2

```bash
docker run \
  -p 3001:3000 \
  -v "$(pwd)/db.json:/data/db.json" \
  typicode/json-server \
  --watch /data/db.json --port 3000
```

#### Windows (PowerShell – ajuste o caminho se necessário)

```powershell
docker run `
  -p 3001:3000 `
  -v "${PWD}\db.json:/data/db.json" `
  typicode/json-server `
  --watch /data/db.json --port 3000
```

A API ficará disponível em:  
`http://localhost:3001`

### 1.4. Endpoints Disponíveis

A partir do JSON Server, você terá automaticamente:

- `GET /customers` – lista de clientes;
- `GET /customers/:id` – detalhes de um cliente;
- `POST /customers` – criação de cliente;
- `PUT /customers/:id` – edição de cliente;
- `PATCH /customers/:id` – atualização parcial;
- `DELETE /customers/:id` – exclusão (não é obrigatório implementar no frontend).

---

## 2. Requisitos Técnicos da Aplicação Frontend

- **Obrigatório**
  - React (pode ser Vite ou Next.js);
  - Tailwind CSS;
  - Uso de componentes baseados em **shadcn/ui** (botões, inputs, dialog, etc.);
  - Integração com a API RESTful `http://localhost:3001/customers`;
  - Organização mínima de pastas e componentes.

- Desejável (não eliminatório, mas será visto como positivo):
  - Uso de TypeScript;
  - Hooks customizados para consumo da API ou lógica de estado;
  - Alguma preocupação visível com performance (ex.: evitar re-renderizações desnecessárias, listas com keys adequadas, etc.).

---

## 3. Funcionalidades Obrigatórias

### 3.1. Listagem de Clientes

Crie uma **tela principal** com:

- Tabela ou lista de clientes exibindo, pelo menos:
  - Nome;
  - E-mail;
  - Status (*active* / *inactive*);
  - Data de criação (`createdAt` formatada);
- Elementos da interface:
  - **Campo de busca** por nome ou e-mail;
  - **Filtro por status**:
    - Todos;
    - Ativo;
    - Inativo;
  - **Ordenação**:
    - Por Nome;
    - Por Data de criação (asc/desc – pode ser um select ou botões).

Ao clicar em um cliente na lista, deve ser possível visualizar seus detalhes.

### 3.2. Detalhes do Cliente

Quando o usuário clicar em um cliente:

- Exiba os detalhes utilizando um componente de **overlay** do shadcn/ui, como:
  - `Dialog`, `Sheet` ou similar.

Informações mínimas:

- Nome;
- E-mail;
- Telefone;
- Status;
- Data de criação;
- Observações (`notes`).

### 3.3. Criação e Edição de Cliente

Implemente um **formulário reutilizável** para criar e editar clientes.

- Campos:
  - Nome (obrigatório);
  - E-mail (obrigatório, formato válido);
  - Telefone (opcional);
  - Status (obrigatório: `active` / `inactive`);
  - Observações (opcional – área de texto).

- Requisitos:
  - Validações com **feedback visual** (mensagens de erro e estado visual no input);
  - Botões:
    - **Salvar**;
    - **Cancelar**;
  - Após salvar:
    - Atualizar a listagem na tela principal;
    - Fechar modal/dialog ou navegar conforme a sua decisão.

Você pode decidir se criação/edição será:

- Em modal/dialog; ou
- Em uma rota/tela separada.

---

## 4. Estados da Interface

A aplicação deve tratar explicitamente:

- **Loading**:
  - Exibir feedback visual enquanto os dados estão sendo carregados;
- **Empty state**:
  - Caso não haja clientes cadastrados ou a busca/filtro não retorne resultados, exibir uma mensagem apropriada;
- **Error state**:
  - Caso ocorra erro ao consumir a API (ex.: servidor indisponível), exibir mensagem de erro amigável.

---

## 5. UX, Acessibilidade e Performance

Serão observados pontos como:

- Uso de componentes shadcn/ui / Radix com boa base de acessibilidade;
- Navegação por teclado em elementos principais (botões, inputs, modais);
- Uso de `label` associado a `input` e hierarquia de títulos (`h1`, `h2`, etc.);
- Feedback claro em ações importantes (salvar, carregar, erro de validação);
- Preocupação mínima com performance:
  - Evitar chamadas desnecessárias à API;
  - Não realizar re-renderizações óbvias e evitáveis;
  - Uso adequado de `key` em listas.

Não é necessário micro-otimizar, mas esperamos **bom senso** e aderência a boas práticas.

---

## 6. Organização e Código Limpo

Vamos avaliar:

- Estrutura de pastas (ex.: `components/`, `pages/` ou equivalente);
- Separação de responsabilidades (componentes de UI, lógica de dados, serviços de API);
- Nomeação clara de variáveis, funções e componentes;
- Legibilidade do código (indentação, padrões consistentes).

---

## 7. Entrega

Envie:

1. **Link do repositório** (GitHub, GitLab ou similar) contendo:
   - Código-fonte completo da aplicação;
   - Arquivo `db.json`;
2. Um **README** na raiz do projeto contendo:
   - Passos para instalar dependências e rodar o projeto frontend;
   - Comando para subir o JSON Server em Docker;
   - Breve explicação das principais decisões de implementação;
   - (Opcional) Link de deploy (Vercel, Netlify, etc.), se houver.

---

## 8. Critérios de Avaliação

1. **Qualidade do código e organização (30%)**
   - Componentização, separação de responsabilidades, legibilidade.

2. **Uso de React, Tailwind e shadcn/ui / Radix UI (25%)**
   - Uso consistente dos componentes;
   - Estruturação do layout;
   - Clareza do JSX + Tailwind.

3. **Integração com a API RESTful e tratamento de estados (20%)**
   - Consumo correto da API;
   - Tratamento de loading, erro e empty state;
   - Atualização da interface após criação/edição.

4. **Experiência do usuário, acessibilidade e performance básica (15%)**
   - Interface intuitiva e fluida;
   - Feedbacks adequados;
   - Sinais de preocupação com acessibilidade e performance.

5. **Documentação da entrega (10%)**
   - README claro e objetivo;
   - Facilidade para subir o ambiente (frontend + mock API).

---

## 9. Observações Finais

- O foco é **qualidade**, não quantidade de features extras;
- Melhorias adicionais (ex.: paginação, toasts, temas, dark mode, etc.) são bem-vindas, desde que os requisitos mínimos sejam atendidos;
- Se você fizer alguma adaptação relevante em relação ao enunciado, descreva no README.

Boa sorte!
