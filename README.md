

# ToDo List em Go

Um simples aplicativo de ToDo List desenvolvido em Go. Este projeto permite adicionar tarefas, listar tarefas e marcar tarefas como concluídas.

## Tecnologias Utilizadas

- Go (Golang)
- Gorilla Mux (para roteamento)
- Testes utilizando o pacote `testing` do Go

## Instalação

1. **Certifique-se de que o Go está instalado**:
 Você pode verificar a instalação do Go executando `go version` no terminal.

2. **Clone o repositório**:
   ```bash
   git clone https://github.com/gabrielss2406/C214-Seminario.git
   cd C214-Seminario
   ```

3. **Instale as dependências**:
   ```bash
   go mod tidy
   ```
## Rodando localmente

Já estando dentro da pasta do projeto, inicie o server com

```bash
  go run main.go
```
OBS: essa maneira de rodar não implementar hot reload, caso queira, configure o ```aix```.
## Endpoints da API
**Adicionar Tarefa**: POST /todos
```
{
  "tarefa": "Sua nova tarefa"
}
```
**Listar Tarefas**: GET /todos

**Marcar Tarefa como Feita**: PUT /todos/{id}

## Rodando os testes

Para rodar os testes:
```
go test
```

Para rodar os testes e criar um report em html instale o pacote e depois rode os testes com as linhas de comando abaixo:
```
go get github.com/vakenbolt/go-test-report@latest
go install github.com/vakenbolt/go-test-report@latest
go test -json ./tests | go-test-report -o test_report.html
```

## Configurando o ambiente Docker/Jenkins
para que o Jenkins tenha acesso ao repositório privado do github, é necessário que seja configurado na pipeline com as Credentials de usuário e Secret Text, ambas contendo o PAT (Personal Authentication Token)




## Configurar envio de email no Jenkins
Caso tenha ```vi``` ou ```nano``` instalado no container, basta editar o `var/jenkins_home/cat hudson.plugins.emailext.ExtendedEmailPublisher.xml`


Caso não haja, podemos editar dessa maneira:

Copie o arquivo para o host para edição fora do container:
```bash
  docker cp <nome_do_container>:/var/jenkins_home/hudson.plugins.emailext.ExtendedEmailPublisher.xml .

```
Após editar o arquivo, volte para o container, caso necessário:
```
  docker cp hudson.plugins.emailext.ExtendedEmailPublisher.xml <nome_do_container>:/var/jenkins_home/
```

- também é necessário configurar as variáveis de ambiente, que nesse caso é a ```EMAIL_TO``` que define quem receberá o email de notificação
