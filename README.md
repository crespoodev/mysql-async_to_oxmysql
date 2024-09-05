# Conversão de `mysql-async` para `oxmysql`

Este guia serve para ajudar na conversão de scripts de **FiveM** que utilizam a biblioteca `mysql-async` para a alternativa mais eficiente e moderna, `oxmysql`. A mudança para `oxmysql`.

## Principais Diferenças

1. **Placeholders**: 
   - No `mysql-async`, os placeholders são declarados com `@nome` (parâmetros nomeados).
   - No `oxmysql`, são usados `?` para placeholders, e as variáveis são passadas diretamente por ordem.

2. **Funções Renomeadas**:
   - O `oxmysql` utiliza nomes de funções mais curtos e intuitivos:
     - `MySQL.Async.fetchAll` → `MySQL.query`
     - `MySQL.Async.execute` → `MySQL.update` ou `MySQL.insert` (dependendo do contexto)
     - `MySQL.Async.fetchScalar` → `MySQL.scalar`

3. **Melhoria de Desempenho**:
   - O `oxmysql` foi otimizado para servidores com um volume alto de consultas, resultando em um tempo de resposta mais rápido e menor utilização de recursos.

4. **Compatibilidade com Promises**:
   - O `oxmysql` suporta `Promises`, permitindo o uso de `async/await` para um fluxo de código mais moderno e legível.

## Exemplo de Conversões

### Consulta Simples

- **`mysql-async`:**
  ```lua
  MySQL.Async.fetchAll('SELECT * FROM users WHERE id = @id', {
      ['@id'] = userId
  }, function(result)
      print(result)
  end)
  ```

- **`oxmysql`:**
  ```lua
  MySQL.query('SELECT * FROM users WHERE id = ?', {userId}, function(result)
      print(result)
  end)
  ```

### Inserção de Dados

- **`mysql-async`:**
  ```lua
  MySQL.Async.execute('INSERT INTO users (name, age) VALUES (@name, @age)', {
      ['@name'] = userName,
      ['@age'] = userAge
  }, function(rowsChanged)
      print(rowsChanged)
  end)
  ```

- **`oxmysql`:**
  ```lua
  MySQL.insert('INSERT INTO users (name, age) VALUES (?, ?)', {userName, userAge}, function(insertId)
      print(insertId)
  end)
  ```

### Atualização de Dados

- **`mysql-async`:**
  ```lua
  MySQL.Async.execute('UPDATE users SET age = @age WHERE id = @id', {
      ['@age'] = newAge,
      ['@id'] = userId
  }, function(rowsChanged)
      print(rowsChanged)
  end)
  ```

- **`oxmysql`:**
  ```lua
  MySQL.update('UPDATE users SET age = ? WHERE id = ?', {newAge, userId}, function(affectedRows)
      print(affectedRows)
  end)
  ```

### Obtenção de Um Valor Específico

- **`mysql-async`:**
  ```lua
  MySQL.Async.fetchScalar('SELECT age FROM users WHERE id = @id', {
      ['@id'] = userId
  }, function(age)
      print(age)
  end)
  ```

- **`oxmysql`:**
  ```lua
  MySQL.scalar('SELECT age FROM users WHERE id = ?', {userId}, function(age)
      print(age)
  end)
  ```

Para mais informações, consulta a [documentação oficial do oxmysql](https://overextended.dev/oxmysql)
