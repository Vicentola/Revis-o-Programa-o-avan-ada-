# 📚 Resumo — Revisão 2º Bimestre · PRAV80 Programação Avançada
> Vicente, fiz baseado nos slides do Prof. Baroni

---

## 1. ESTRUTURAS DE DADOS AVANÇADAS

### 🔹 Pilha (LIFO — Last In, First Out)
> Último a entrar, primeiro a sair. Usa `append()` para empilhar e `pop()` para desempilhar.

```python
# Usando lista diretamente
pilha = [3, 4, 5]
print('pilha:' + str(pilha))   # pilha:[3, 4, 5]
pilha.append(6)
pilha.append(7)
print('pilha:' + str(pilha))   # pilha:[3, 4, 5, 6, 7]
print('pop:' + str(pilha.pop()))  # pop:7
print('pilha:' + str(pilha))   # pilha:[3, 4, 5, 6]
print('pop:' + str(pilha.pop()))  # pop:6
print('pop:' + str(pilha.pop()))  # pop:5
print('pilha:' + str(pilha))   # pilha:[3, 4]
```

```python
# Classe Pilha (OO)
class Pilha():
    def __init__(self):
        self.items = []

    def vazia(self):
        return self.items == []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop()

    def topo(self):
        return self.items[-1]

    def tam(self):
        return len(self.items)

# Uso:
pilha = Pilha()
pilha.push(1)
pilha.push(2)
pilha.push(3)
print(pilha.pop())   # Saída: 3
print(pilha.topo())  # Saída: 2
print(pilha.tam())   # Saída: 2
```

---

### 🔹 Fila (FIFO — First In, First Out)
> Primeiro a entrar, primeiro a sair. Usa `collections.deque`.

```python
from collections import deque

fila = deque(["Klaus", "Davi", "Renan"])
print(fila)                    # deque(['Klaus', 'Davi', 'Renan'])
fila.append("Murilo")          # Murilo chega no final
fila.append("Patryck")         # Patryck chega no final
print(fila)                    # deque(['Klaus', 'Davi', 'Renan', 'Murilo', 'Patryck'])
print(fila.popleft())          # Klaus  ← 1º da fila sai
print(fila.popleft())          # Davi   ← novo 1º sai
print(fila)                    # deque(['Renan', 'Murilo', 'Patryck'])
```

```python
# Classe Fila (OO)
from collections import deque

class Fila():
    def __init__(self):
        self.items = deque()

    def vazia(self):
        return len(self.items) == 0

    def enfileira(self, item):
        self.items.append(item)

    def desenfileira(self):
        return self.items.popleft()

    def tam(self):
        return len(self.items)

# Uso:
fila = Fila()
fila.enfileira(1)
fila.enfileira(2)
fila.enfileira(3)
print(fila.desenfileira())  # Saída: 1  ← FIFO!
print(fila.tam())           # Saída: 2
```

---

### 🔹 Fila Prioritária
> Quem tem menor número de prioridade sai primeiro (usa `heapq`).

```python
import heapq

class FilaPrioritaria():
    def __init__(self):
        self.heap = []

    def vazia(self):
        return len(self.heap) == 0

    def enfileira(self, item, priority):
        heapq.heappush(self.heap, (priority, item))

    def desenfileira(self):
        return heapq.heappop(self.heap)[1]

    def tam(self):
        return len(self.heap)

# Uso:
fila_prioridades = FilaPrioritaria()
fila_prioridades.enfileira("Victor", 2)
fila_prioridades.enfileira("Alysson", 1)   # prioridade 1 = maior urgência
fila_prioridades.enfileira("João", 3)
print(fila_prioridades.desenfileira())  # Saída: Alysson  ← prioridade 1
print(fila_prioridades.tam())           # Saída: 2
```

---

## 2. MANIPULAÇÃO DE ARQUIVOS

### 🔹 Modos do `open()`

| Modo | O que faz |
|------|-----------|
| `'r'` | Leitura. Arquivo deve existir. |
| `'w'` | Escrita. **Apaga** o conteúdo existente. Cria se não existir. |
| `'a'` | Append. **Adiciona** ao final. Cria se não existir. |
| `'x'` | Criação exclusiva. Erro se o arquivo já existir. |
| `'b'` | Modo binário (ex: `'rb'`, `'wb'`) |
| `'+'` | Leitura + escrita (ex: `'r+'`, `'w+'`) |

### 🔹 Operações

```python
# Ler arquivo
file = open("teste.txt", "r")
content = file.read()
print(content)
file.close()

# Escrever (substitui tudo)
file = open("teste.txt", "w")
file.write("Hello, World!")
file.close()

# Adicionar ao final
file = open("geek.txt", "a")
file.write("This will add this line")
file.close()

# Com 'with' — fecha automaticamente (melhor prática!)
with open("teste.txt", "r") as file:
    content = file.read()
    print(content)
```

### 🔹 Tratamento de Exceções

```python
# Estrutura completa
try:
    # código que pode dar erro
    file = open("teste.txt", "r")
    content = file.read()
    print(content)
except:
    # trata a exceção
    pass
else:
    # executa SE não houve exceção
    pass
finally:
    file.close()  # SEMPRE executa, com ou sem erro
```

> ⚠️ **`finally` sempre roda** — é o lugar certo para fechar arquivos!

---

## 3. SERIALIZAÇÃO DE OBJETOS

> Converter objeto → sequência de bytes/texto para salvar/transmitir.
> **Deserialização** = processo inverso (carregar de volta).

### 🔹 Com Pickle (binário, só Python)

```python
import pickle

dados = {'nome': 'João', 'Idade': 23}

# Serializar (salvar)
with open('dados.pickle', 'wb') as arquivo:
    pickle.dump(dados, arquivo)

# Deserializar (carregar)
with open('dados.pickle', 'rb') as arquivo:
    dados_seriais = arquivo.read()
    arquivo.seek(0)
    dados_carregados = pickle.load(arquivo)

print("Tipo de dados serializados:", type(dados_seriais))
# Saída: Tipo de dados serializados: <class 'bytes'>

print("\nDados deserializados:", dados_carregados)
# Saída: Dados deserializados: {'nome': 'João', 'Idade': 23}

print("Tipo de dados deserializados:", type(dados_carregados))
# Saída: Tipo de dados deserializados: <class 'dict'>
```

### 🔹 Com JSON (texto, legível, ideal para APIs)

```python
import json

dados = {'nome': 'João', 'Idade': 23}

# Serializar
with open('dados.json', 'w') as arquivo:
    json.dump(dados, arquivo)

# Deserializar
with open('dados.json', 'r') as arquivo:
    dados_seriais = arquivo.read()
    arquivo.seek(0)
    dados_carregados = json.load(arquivo)

print("Tipo de dados serializados:", type(dados_seriais))
# Saída: Tipo de dados serializados: <class 'str'>

print("\nDados deserializados:", dados_carregados)
# Saída: Dados deserializados: {'nome': 'João', 'Idade': 23}

print("Tipo de dados deserializados:", type(dados_carregados))
# Saída: Tipo de dados deserializados: <class 'dict'>
```

### 🔹 Com YAML (texto, legível, hierárquico)

```python
import yaml

dados = {'nome': 'João', 'Idade': 23}

# Serializar
with open('dados.yaml', 'w') as arquivo:
    yaml.dump(dados, arquivo)

# Deserializar
with open('dados.yaml', 'r') as arquivo:
    serialized_data = arquivo.read()
    arquivo.seek(0)
    loaded_data = yaml.safe_load(arquivo)

# Saídas iguais ao JSON:
# Tipo de dados serializados: <class 'str'>
# Dados deserializados: {'nome': 'João', 'Idade': 23}
# Tipo de dados deserializados: <class 'dict'>
```

### 🔹 Exemplo com múltiplos registros (JSON)

```python
import json

dados = {
    'Aluno 1': {'nome': 'João', 'Idade': 23},
    'Aluno 2': {'nome': 'Lucas', 'Idade': 22},
    'Aluno 3': {'nome': 'Pedro', 'Idade': 25}
}

with open('dados.json', 'w') as arquivo:
    json.dump(dados, arquivo)

with open('dados.json', 'r') as arquivo:
    dados_seriais = arquivo.read()
    arquivo.seek(0)
    dados_carregados = json.load(arquivo)

print("Tipo de dados serializados:", type(dados_seriais))
# <class 'str'>

print("\nDados deserializados:", dados_carregados)
# {'Aluno 1': {'nome': 'João', 'Idade': 23}, ...}

print("Tipo de dados deserializados:", type(dados_carregados))
# <class 'dict'>

print("O 1º aluno se chama "
      + dados_carregados["Aluno 1"]["nome"]
      + " e tem "
      + str(dados_carregados["Aluno 1"]["Idade"])
      + " anos de vida.")
# Saída: O 1º aluno se chama João e tem 23 anos de vida.
```

> 🔑 **Diferença chave Pickle vs JSON:**
> - Pickle serializa como `bytes` → `<class 'bytes'>`
> - JSON serializa como texto → `<class 'str'>`
> - Ambos deserializam de volta para `<class 'dict'>`

---

## 4. MULTITHREADING / PARALELISMO

### 🔹 Conceitos

| Conceito | Definição |
|----------|-----------|
| **Concorrência** | Lida com mais de uma tarefa, sem necessariamente executar ao mesmo tempo |
| **Paralelismo** | Mais de uma tarefa executando **ao mesmo tempo** |
| **Thread** | Linha de execução dentro de um processo |
| **GIL** | Global Interpreter Lock — só uma thread executa código Python por vez |
| **FIFO/E/S** | Operações de arquivo/rede: boas candidatas para threading |

### 🔹 Threading com `threading.Thread`

```python
import threading

def cubo(num):
    print("{} ao Cubo é: {}".format(num, num * num * num))

def quadrado(num):
    print("{} ao Quadrado é: {}".format(num, num * num))

num = 10
t1 = threading.Thread(target=quadrado, args=(num,))
t2 = threading.Thread(target=cubo, args=(num,))

t1.start()
t2.start()

t1.join()
t2.join()

print("feito")

# Saída (ordem pode variar!):
# 10 ao Quadrado é: 100
# 10 ao Cubo é: 1000
# feito
```

> ⚠️ `start()` inicia a thread, `join()` espera ela terminar antes de continuar.

---

## 5. PADRÕES DE DESIGN (Design Patterns)

### 🔹 Categorias (GoF — Gang of Four)

| Categoria | O que resolve |
|-----------|--------------|
| **Criacionais** | Como criar objetos (Singleton, Factory Method, Builder) |
| **Estruturais** | Como organizar classes (Adapter, Decorator, Facade) |
| **Comportamentais** | Como objetos se comunicam (Command, Observer, Strategy) |

---

### 🔹 CRIACIONAIS

**1. Singleton** — Garante que existe **apenas uma instância** da classe.
```python
# Reconhecido por: método estático de criação que retorna sempre o mesmo objeto em cache
class Singleton:
    _instance = None

    @classmethod
    def get_instance(cls):
        if cls._instance is None:
            cls._instance = cls()
        return cls._instance
```

**2. Factory Method** — Interface para criar objetos, mas **subclasses decidem** qual tipo criar.
```python
# Reconhecido por: método de criação na classe base + subclasses que o sobrescrevem
class AnimalFactory:
    def criar_animal(self):  # subclasses sobrescrevem isso
        pass
```

**3. Builder** — Constrói objetos complexos **passo a passo**. Suporta encadeamento de métodos.
```python
# Reconhecido por: um método de criação + vários métodos de configuração
algumBuilder.configValorA(1).configValorB(2).criar()
```

---

### 🔹 ESTRUTURAIS

**1. Adapter** — Permite que **interfaces incompatíveis** trabalhem juntas.
```python
# Ex: converter XML para JSON para que sistemas diferentes se comuniquem
# Reconhecido por: classe que converte a interface de um objeto para outra
```

**2. Decorator** — Adiciona **novos comportamentos** a objetos sem modificar sua classe.
```python
# Reconhecido por: "wrapper" — embrulha um objeto e adiciona funcionalidade
# Usa composição/agregação ao invés de herança múltipla
```

**3. Facade** — Fornece **interface simplificada** para um sistema complexo.
```python
# Reconhecido por: uma classe simples que "esconde" a complexidade de várias outras
# Ex: método codificar(arquivo, formato) que usa internamente uma biblioteca complexa de vídeo
```

---

### 🔹 COMPORTAMENTAIS

**1. Command** — Transforma um **pedido em objeto** independente com toda sua informação.
```python
# Reconhecido por: classes separadas para cada ação (ex: BotaoSalvar, BotaoCopiar)
# Separa quem faz o pedido de quem executa
```

**2. Observer** — Objeto **notifica automaticamente** outros sobre mudanças de estado.
```python
# Publisher (Publicador) ← o sujeito que tem estado
# Subscribers (Assinantes) ← quem quer ser notificado
# Ex: loja notifica clientes quando produto volta ao estoque
```

**3. Strategy** — Define família de algoritmos, **encapsula cada um** e os torna intercambiáveis.
```python
# Reconhecido por: classe Contexto + várias classes Estratégia com mesmo método
# Ex: GPS com RoadStrategy, WalkingStrategy, PublicTransportStrategy
# Cada uma implementa buildRoute(A, B) do seu jeito
```

---

## 6. APIs REST

### 🔹 Tipos de API

| Tipo | Características |
|------|----------------|
| **SOAP** | Usa XML, mais antigo, menos flexível |
| **RPC** | Cliente executa função no servidor |
| **WebSocket** | Comunicação **bidirecional**, contínua |
| **REST** | Mais popular, stateless, usa HTTP |

### 🔹 Métodos HTTP (REST)

| Método | Operação |
|--------|---------|
| `GET` | Leitura |
| `POST` | Criação |
| `PUT` | Atualização completa |
| `PATCH` | Atualização parcial |
| `DELETE` | Exclusão |

### 🔹 Características do REST
- **Stateless**: nenhum estado de sessão fica no servidor
- **Cliente-servidor**: separação clara
- **Cacheável**: respostas podem ser armazenadas em cache
- **Interface uniforme**: URLs e métodos padronizados

---

## 7. TESTES UNITÁRIOS

### 🔹 O que são?
- Testam as **menores partes** do código (funções/métodos) de forma **isolada**
- Não precisam de servidor ou banco de dados reais (usam **stubs**)
- Parte do **TDD** (Test-Driven Development)

### 🔹 O que testar?
- **Checagem de lógica**: caminhos esperados com entradas válidas
- **Checagem de limites**: Ex: se espera int entre 3-7, teste 5 (normal), 3 (limite), 9 (inválido)
- **Manuseio de erros**: o código avisa? trava? lida bem?
- **Checagem de OO**: estado do objeto é atualizado corretamente?

### 🔹 Python `unittest`

```python
import unittest

class MeuTeste(unittest.TestCase):

    def test_exemplo(self):
        self.assertEqual(2 + 2, 4)        # passa se a == b
        self.assertTrue(True)              # passa se x é True
        self.assertFalse(False)            # passa se x é False
        self.assertIsNone(None)            # passa se x is None
        self.assertIsInstance([], list)    # passa se a é instância de b
        self.assertIs("a", "a")           # passa se a is b
        self.assertIn(1, [1, 2, 3])        # passa se a in b

if __name__ == '__main__':
    unittest.main()
```

### 🔹 Saídas possíveis do Unittest

| Saída | Significado |
|-------|-------------|
| `OK` | Todos os testes passaram |
| `FAIL` | Teste falhou → lança `AssertionError` |
| `ERROR` | Qualquer outra exceção além de `AssertionError` |

### 🔹 Métodos assert principais

| Método | Equivalente |
|--------|------------|
| `assertEqual(a, b)` | `a == b` |
| `assertTrue(x)` | `bool(x) is True` |
| `assertFalse(x)` | `bool(x) is False` |
| `assertIsNone(x)` | `x is None` |
| `assertIsInstance(a, b)` | `isinstance(a, b)` |
| `assertIs(a, b)` | `a is b` |
| `assertIn(a, b)` | `a in b` |

---

## 8. CONEXÃO COM BANCO DE DADOS

### 🔹 SQL vs NoSQL

| SQL | NoSQL |
|-----|-------|
| PostgreSQL, MySQL, SQLite | MongoDB, Redis, Cassandra |
| Tabelas com estrutura fixa | Documentos, coleções flexíveis |

### 🔹 SQLite3 (embutido no Python)

```python
import sqlite3
# Não precisa instalar nada
# Arquivo local .db
```

### 🔹 MySQL

```python
import mysql.connector

mydb = mysql.connector.connect(
    host = "localhost",
    user = "seu_usuario",
    password = "sua_senha"
)

mycursor = mydb.cursor()

mycursor.execute("CREATE DATABASE meudb")
mycursor.execute("SHOW DATABASES")
for x in mycursor:
    print(x)

mycursor.execute("CREATE TABLE clientes (id INT AUTO_INCREMENT PRIMARY KEY, nome VARCHAR(255), endereco VARCHAR(255))")

sql = "INSERT INTO clientes (nome, endereco) VALUES (%s, %s)"
val = ("John", "Highway 21")
mycursor.execute(sql, val)

# Instalar com: pip install mysql-connector-python
```

### 🔹 MongoDB (NoSQL)

```python
import pymongo

conn = "mongodb://localhost:27017"
client = pymongo.MongoClient(conn)

db = client.classDB
print(db)

# Instalar com: pip install pymongo
```

---

## 🎯 COLA RÁPIDA — O QUE MAIS CAI EM CÓDIGO

### Pilha vs Fila
```
Pilha (LIFO):  append() + pop()        → retira do FINAL
Fila (FIFO):   append() + popleft()    → retira do INÍCIO
FilaPrioritaria: heappush/heappop      → retira pela PRIORIDADE (menor número = maior urgência)
```

### Serialização — Diferença de tipos
```
pickle.dump / pickle.load  → serializado é bytes  (<class 'bytes'>)
json.dump   / json.load    → serializado é str    (<class 'str'>)
yaml.dump   / yaml.safe_load → serializado é str  (<class 'str'>)
Todos deserializam para dict (<class 'dict'>)
```

### Modos de arquivo
```
'r' = só lê (arquivo precisa existir)
'w' = escreve e APAGA tudo antes
'a' = adiciona ao FINAL (append)
'x' = cria NOVO (erro se já existir)
'b' = binário (wb, rb)
with open(...) as f: → fecha automaticamente
finally: → sempre executa
```

### Padrões
```
Singleton   → só 1 instância
Factory     → subclasse decide o tipo a criar
Builder     → passo a passo + encadeamento
Adapter     → adapta interfaces incompatíveis
Decorator   → adiciona comportamento (wrapper)
Facade      → simplifica interface complexa
Command     → pedido vira objeto
Observer    → Publisher notifica Subscribers
Strategy    → algoritmos intercambiáveis
```
