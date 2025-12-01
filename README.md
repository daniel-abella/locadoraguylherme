# 🐍 MySQL Helper em Python

Pequeno módulo utilitário em **Python** para facilitar o acesso a bancos de dados **MySQL** usando `mysql.connector`, com foco em:

- ✅ Conexão e encerramento seguros  
- ✅ Uso de **Prepared Statements**  
- ✅ Tratamento de exceções com *rollback* em operações de escrita  
- ✅ Funções genéricas para **INSERT**, **SELECT**, **UPDATE** e **DELETE**

---

## 📁 Estrutura do módulo

```python
import mysql.connector

def criarConexao(endereco, usuario, senha, bancodedados): ...
def encerrarConexao(connection): ...
def insertNoBancoDados(connection, sql, dados): ...
def listarBancoDados(connection, sql, params=None): ...
def atualizarBancoDados(connection, sql, dados): ...
def excluirBancoDados(connection, sql, dados): ...
```

---

## 🔧 Pré-requisitos

- Python 3.8+
- MySQL Server (local ou remoto)
- Biblioteca `mysql-connector-python`

### Instalando o conector

```bash
pip install mysql-connector-python
```

---

## 🚀 Como usar

### 1. Importando o módulo

Supondo que o arquivo se chame `mysql_helper.py`:

```python
from mysql_helper import (
    criarConexao,
    encerrarConexao,
    insertNoBancoDados,
    listarBancoDados,
    atualizarBancoDados,
    excluirBancoDados
)
```

### 2. Criando a conexão

```python
connection = criarConexao(
    endereco="localhost",
    usuario="seu_usuario",
    senha="sua_senha",
    bancodedados="nome_do_banco"
)

if not connection:
    print("Não foi possível conectar ao banco de dados.")
    exit(1)
```

---

## 📝 Exemplos de uso

### 🔹 INSERT (create)

```python
sql = "INSERT INTO usuarios (nome, email) VALUES (%s, %s)"
dados = ("Maria", "maria@example.com")

id_novo = insertNoBancoDados(connection, sql, dados)

if id_novo:
    print(f"Registro inserido com ID: {id_novo}")
else:
    print("Falha ao inserir registro.")
```

---

### 🔹 SELECT (read)

```python
sql = "SELECT id, nome, email FROM usuarios WHERE email = %s"
params = ("maria@example.com",)

resultados = listarBancoDados(connection, sql, params)

for linha in resultados:
    print(linha)
```

---

### 🔹 UPDATE (update)

```python
sql = "UPDATE usuarios SET nome = %s WHERE id = %s"
dados = ("Maria Silva", 1)

linhas = atualizarBancoDados(connection, sql, dados)

print(f"Linhas afetadas: {linhas}")
```

---

### 🔹 DELETE (delete)

```python
sql = "DELETE FROM usuarios WHERE id = %s"
dados = (1,)

linhas = excluirBancoDados(connection, sql, dados)

print(f"Linhas afetadas: {linhas}")
```

---

### 🔚 Encerrando a conexão

```python
encerrarConexao(connection)
```

---

## 🧱 Boas práticas implementadas

- Uso de **Prepared Statements** (`cursor = connection.cursor(prepared=True)`), ajudando na prevenção de **SQL Injection**.
- Uso de `try / except / finally` para:
  - Realizar `commit()` somente em caso de sucesso.
  - Executar `rollback()` em erros nas operações de escrita (**INSERT**, **UPDATE**, **DELETE**).
  - Garantir que o `cursor.close()` seja chamado mesmo em caso de erro.
- Retorno de valores úteis:
  - `insertNoBancoDados` retorna o `lastrowid` ou `None`.
  - `listarBancoDados` retorna uma lista de resultados ou lista vazia.
  - `atualizarBancoDados` e `excluirBancoDados` retornam o número de linhas afetadas.

---

## 🧪 Sugestão de melhorias futuras

- Adicionar **tipagem estática** (type hints) em todas as funções.
- Centralizar mensagens de log usando a biblioteca `logging` ao invés de `print`.
- Suporte a **pool de conexões**.
- Criação de uma classe `DatabaseClient` para encapsular ainda mais a lógica.

---

## 📄 Licença

Sinta-se à vontade para usar e adaptar este código em seus projetos acadêmicos ou profissionais.  
Se reutilizar, considere mencionar o autor original do módulo. 😊
