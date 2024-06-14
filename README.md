# Auth API

## Descrição

API para autenticação e gerenciamento de usuários. Inclui funcionalidades de login, criação de usuário, atualização de senha e exclusão de usuário.

## Endpoints

### Login

Realiza o login do usuário e retorna um token JWT.

- **URL**: `/api/v1/auth/login`
- **Método HTTP**: `POST`
- **Request Body**:
  ```json
  {
    "username": "string",
    "password": "string"
  }
Responses:
200 OK: Retorna o token JWT.
json
Copiar código
{
  "Token": "string"
}
400 Bad Request: Se o request body for inválido.
401 Unauthorized: Se o usuário ou senha forem inválidos.
500 Internal Server Error: Se ocorrer um erro no servidor.
###Criar Usuário
Cria um novo usuário.

URL: /api/v1/auth/criar
Método HTTP: POST
Request Body:
json
Copiar código
{
  "usuario": "string",
  "email": "string",
  "senha": "string"
}
Responses:
200 OK: Retorna o modelo do usuário criado.
json
Copiar código
{
  // Modelo do usuário criado
}
204 No Content: Se o usuário for criado com sucesso mas sem conteúdo a retornar.
400 Bad Request: Se o request body for inválido.
500 Internal Server Error: Se ocorrer um erro no servidor.
Atualizar Senha de Usuário
Atualiza a senha de um usuário existente.

URL: /api/v1/auth/atualizar
Método HTTP: POST
Request Body:
json
Copiar código
{
  "usuario": "string",
  "senha": "string"
}
Responses:
200 OK: Retorna o modelo do usuário atualizado.
json
Copiar código
{
  // Modelo do usuário atualizado
}
400 Bad Request: Se o request body for inválido.
500 Internal Server Error: Se ocorrer um erro no servidor.
Excluir Usuário
Exclui um usuário existente.

URL: /api/v1/auth/{id}
Método HTTP: DELETE
Request Param: id - ID do usuário a ser excluído.
Responses:
200 OK: Se o usuário for excluído com sucesso.
json
Copiar código
{
  // Modelo do usuário excluído
}
400 Bad Request: Se o request for inválido.
500 Internal Server Error: Se ocorrer um erro no servidor.
Configuração
Certifique-se de configurar as seguintes variáveis no arquivo appsettings.json:

json
Copiar código
{
  "Jwt": {
    "Key": "sua-chave-secreta",
    "Issuer": "seu-emissor",
    "Audience": "sua-audiencia"
  }
}
Serviços Utilizados
Criptografia de Senha
Utiliza o serviço ICryptoService para criptografar a senha do usuário antes de salvá-la no banco de dados.

Repositório de Usuário
Utiliza o repositório IUsuarioRepository para operações de validação e manipulação de dados do usuário.

Métodos Privados
GenerateJwtToken
Gera um token JWT para um usuário autenticado.

csharp
Copiar código
private string GenerateJwtToken(string username)
{
    var claims = new[]
    {
        new Claim(JwtRegisteredClaimNames.Sub, username),
        new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
        new Claim(ClaimTypes.Name, username)
    };

    var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["Jwt:Key"]));
    var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

    var token = new JwtSecurityToken(
        issuer: _configuration["Jwt:Issuer"],
        audience: _configuration["Jwt:Audience"],
        claims: claims,
        expires: DateTime.Now.AddMinutes(30),
        signingCredentials: creds);

    return new JwtSecurityTokenHandler().WriteToken(token);
}
CriarModelo
Cria um modelo de usuário a partir de uma entidade de usuário.

csharp
Copiar código
private UsuarioModel CriarModelo(UsuarioEntity entidade) => new(this, entidade);

# API Tarefas

## Descrição

API para gerenciamento de tarefas com funcionalidades de criação, atualização, exclusão e consulta de tarefas. A API também permite a exportação de tarefas para CSV e a filtragem por status ou usuário.

## Pré-requisitos

Antes de executar a API, certifique-se de que você tenha os seguintes softwares instalados em seu ambiente:

- .NET SDK (versão compatível com o projeto)
- Visual Studio (ou outro editor de sua escolha, como VS Code)
- Git (para controle de versão e clonagem do repositório)
- Postman (opcional, para testar a API)

## Como Executar a API Localmente

### Clonar o Repositório

Clone o repositório do GitHub para o seu ambiente local usando o comando:

```bash
git clone https://github.com/seu-usuario/seu-repositorio.git
Abrir o Projeto no Visual Studio
Abra o Visual Studio.
Vá em File > Open > Project/Solution.
Navegue até o diretório onde você clonou o repositório e selecione o arquivo .sln do projeto.
Configurar o Ambiente de Desenvolvimento
Verifique se o arquivo appsettings.json está configurado corretamente. Este arquivo contém as configurações necessárias para a execução da API, como conexões de banco de dados e outras variáveis de ambiente.

Restaurar Dependências
No terminal do Visual Studio ou usando o prompt de comando, navegue até o diretório do projeto e execute o comando:

bash
Copiar código
dotnet restore
Compilar o Projeto
Ainda no terminal, execute o comando:

bash
Copiar código
dotnet build
Executar a API
Para iniciar a API, use o comando:

bash
Copiar código
dotnet run
Ou pressione F5 no Visual Studio para executar o projeto em modo de depuração.

Testar a API
Uma vez que a API esteja em execução, você pode fazer chamadas aos endpoints usando um cliente HTTP como o Postman ou o navegador.

Exemplos de Endpoints
Obter todas as tarefas:

http
Copiar código
GET http://localhost:5000/api/v1/tarefas
Obter tarefas por status:

http
Copiar código
GET http://localhost:5000/api/v1/tarefas/status?status=em-andamento
Obter tarefas por usuário:

http
Copiar código
GET http://localhost:5000/api/v1/tarefas/usuario
Criar uma nova tarefa:

http
Copiar código
POST http://localhost:5000/api/v1/tarefas
Body (JSON):
{
  "titulo": "Nova Tarefa",
  "descricao": "Descrição da tarefa"
}
Atualizar uma tarefa:

http
Copiar código
POST http://localhost:5000/api/v1/tarefas/atualizar
Body (JSON):
{
  "id": 1,
  "titulo": "Tarefa Atualizada",
  "descricao": "Descrição atualizada"
}
Excluir uma tarefa:

http
Copiar código
DELETE http://localhost:5000/api/v1/tarefas/{id}
Finalizar a Execução
Para finalizar a execução da API, pressione Ctrl+C no terminal onde a API está sendo executada.

Notas Adicionais
Autenticação: A API utiliza autenticação JWT. Certifique-se de incluir o token Bearer nas requisições autenticadas.
Erros e Logs: Os logs são gerados durante a execução para facilitar a identificação de problemas. Verifique os logs para obter informações detalhadas sobre a execução da API.
Documentação dos Endpoints
Obter todas as tarefas
csharp
Copiar código
/// <summary>
/// Obtem todas as tarefas, podendo exportá-las para CSV conforme parâmetro.
/// </summary>
/// <param name="status"></param>
/// <param name="exportar"></param>    
/// <param name="retornarTarefaEventHandler"></param>
/// <param name="cancellationToken"></param>
/// <returns></returns>
[HttpGet]
public async Task<ActionResult> Get(
     [FromQuery] string? status,
     [FromQuery] bool? exportar,
     [FromServices] IRequestHandler<DomainEvent<RetornarTarefaEvent>, List<TarefaEntity>> retornarTarefaEventHandler,
     CancellationToken cancellationToken)
{
    // Código do método
}
Obter tarefas por status
csharp
Copiar código
/// <summary>
/// Obtem todas as tarefas por status.
/// </summary>
/// <param name="status"></param>    
/// <param name="retornarTarefaEventHandler"></param>
/// <param name="cancellationToken"></param>
/// <returns></returns>
[HttpGet("status")]
public async Task<ActionResult> GetByStatus(
     [FromQuery] string status,
     [FromServices] IRequestHandler<DomainEvent<RetornarTarefaEvent>, List<TarefaEntity>> retornarTarefaEventHandler,
     CancellationToken cancellationToken)
{
    // Código do método
}
Obter tarefas por usuário
csharp
Copiar código
/// <summary>
/// Obtem todas as tarefas por Usuário.
/// </summary>        
/// <param name="retornarTarefaUsuarioEventHandler"></param>
/// <param name="cancellationToken"></param>    
/// <returns></returns>
[HttpGet("usuario")]
public async Task<ActionResult> GetByUsuario(
     [FromServices] IRequestHandler<DomainEvent<RetornarTarefaUsuarioEvent>, List<TarefaEntity>> retornarTarefaUsuarioEventHandler,
     CancellationToken cancellationToken)
{
    // Código do método
}
Criar uma nova tarefa
csharp
Copiar código
/// <summary>
/// Cria uma tarefa.
/// </summary>
/// <param name="input"></param>
/// <param name="criarTarefaEventHandler"></param>
/// <param name="cancellationToken"></param>
/// <returns></returns>
[HttpPost]
public async Task<ActionResult<TarefaModel>> Post(
    [FromBody] CriarTarefaInput input,
    [FromServices] IRequestHandler<DomainEvent<CriarTarefaEvent>, TarefaEntity> criarTarefaEventHandler,
    CancellationToken cancellationToken)
{
    // Código do método
}
Atualizar uma tarefa
csharp
Copiar código
/// <summary>
/// Atualiza uma Tarefa.
/// </summary>
/// <param name="input"></param>
/// <param name="atualizarTarefaEventHandler"></param>
/// <param name="cancellationToken"></param>
/// <returns></returns>
[HttpPost("atualizar")]
public async Task<ActionResult> Post(
    [FromBody] AtualizarTarefaInput input,
    [FromServices] IRequestHandler<DomainEvent<AtualizarTarefaEvent>, TarefaEntity> atualizarTarefaEventHandler,
    CancellationToken cancellationToken)
{
    // Código do método
}
Andamento em uma tarefa
csharp
Copiar código
/// <summary>
/// Andamento em uma Tarefa.
/// </summary>
/// <param name="idTarefa"></param>
/// <param name="andamentoTarefaEventHandler"></param>
/// <param name="cancellationToken"></param>
/// <returns></returns>
[HttpPost("andamento/{idTarefa}")]
public async Task<ActionResult> Post(
    [FromRoute] int idTarefa,
    [FromServices] IRequestHandler<DomainEvent<AndamentoTarefaEvent>, TarefaEntity> andamentoTarefaEventHandler,
    CancellationToken cancellationToken)
{
    // Código do método
}
