# **Sistema Web de Biblioteca**

Interface web desenvolvida em Next.js para gerenciamento de uma biblioteca, com autenticação, controle de acesso por perfil e integração com uma API REST.

## **Pré-requisitos**

Antes de executar o projeto, certifique-se de ter:

- Node.js 24 ou superior
- API da Biblioteca configurada e em execução
- SQLite para armazenamento local de autenticação

## **Configuração do ambiente**

Crie um arquivo `.env` na raiz do projeto e configure as variáveis necessárias:

```
API_URL=http://localhost:3000/v1
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=senha_segura_com_16_caracteres
AUTH_DATABASE_PATH=data/auth.sqlite
```

A variável `AUTH_DATABASE_PATH` é opcional e define o local de armazenamento dos dados de autenticação.

Na primeira inicialização, o sistema cria automaticamente o usuário administrador com base nas credenciais definidas no arquivo de ambiente.

## **Instalação**

Instale as dependências do projeto:

```
npm install
```

## **Execução em ambiente de desenvolvimento**

Inicie o servidor Next.js:

```
npm run dev
```

A aplicação estará disponível em:

[http://localhost:3001](http://localhost:3001?utm_source=chatgpt.com)

## **Níveis de acesso**

O sistema possui dois perfis de usuário, cada um com permissões específicas.

| Perfil            | Permissões                                                           |
| ----------------- | -------------------------------------------------------------------- |
| **Administrador** | Gerenciamento de usuários, categorias, autores, livros e relatórios. |
| **Atendente**     | Cadastro e gerenciamento de leitores, empréstimos e devoluções.      |

## **Recursos disponíveis**

- Autenticação de usuários e gerenciamento de sessões.
- Logout com revogação de refresh token.
- Gerenciamento de categorias e autores.
- Cadastro, pesquisa e filtragem de livros.
- Cadastro e consulta de leitores.
- Controle de empréstimos, consultas e devoluções.
- Dashboard administrativo com indicadores e relatórios da biblioteca.

## **Segurança e encerramento de sessão**

O encerramento da sessão é realizado por meio do endpoint:

`POST /v1/auth/logout`

A requisição envia o refresh token, que é invalidado no servidor, impedindo sua reutilização para gerar novos tokens de acesso.

### **Revogação de access tokens**

A tabela `logout_list` permite registrar tokens de acesso revogados antes de seu vencimento. Quando esse mecanismo está habilitado, cada requisição pode verificar se o token informado consta na lista de revogações.

Caso não seja necessário invalidar imediatamente o access token, a tabela pode ser dispensada. Nesse cenário, após o logout, o refresh token deixa de ser válido, mas o access token atual permanece utilizável até expirar.

## **Armazenamento local**

Os usuários cadastrados e suas permissões são armazenados no banco SQLite, por padrão em:

`data/auth.sqlite`

Em ambientes de produção, recomenda-se configurar `AUTH_DATABASE_PATH` para um diretório com armazenamento persistente, garantindo que os dados sejam mantidos mesmo após reinicializações ou atualizações da aplicação.

## **Scripts disponíveis**

| Comando         | Descrição                                        |
| --------------- | ------------------------------------------------ |
| `npm run dev`   | Inicia o ambiente de desenvolvimento.            |
| `npm run build` | Compila a aplicação para produção.               |
| `npm run start` | Executa a versão compilada.                      |
| `npm run lint`  | Analisa o código e verifica possíveis problemas. |

