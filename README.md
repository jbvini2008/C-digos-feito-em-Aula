# Manual de SQL com Java e PostgreSQL

## 1. Conceitos Básicos

### Campo, Tabela, Linha e Coluna
- **Campo**: Unidade de dado armazenada em uma tabela.
- **Tabela**: Conjunto estruturado de dados organizados em linhas e colunas.
- **Linha (Registro)**: Conjunto de valores de uma tabela que representam uma única entrada.
- **Coluna (Atributo)**: Representa um tipo de dado específico dentro de uma tabela.

## 2. Tipos de Dados no Banco de Dados

Alguns tipos comuns no PostgreSQL:

- **Inteiros**: `INTEGER`, `SMALLINT`, `BIGINT`
- **Decimais**: `NUMERIC`, `REAL`, `DOUBLE PRECISION`
- **Texto**: `CHAR`, `VARCHAR`, `TEXT`
- **Data e Hora**: `DATE`, `TIME`, `TIMESTAMP`
- **Booleano**: `BOOLEAN`

## 3. Conexão em Java com PostgreSQL

### 3.1 Arquivo `pom.xml` para um projeto Maven

```xml
<dependencies>
    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>42.3.1</version>
    </dependency>
</dependencies>
```

### 3.2 Código de Conexão em Java

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class ConexaoPostgreSQL {
    public static Connection conectar() {
        String url = "jdbc:postgresql://localhost:5432/seubanco";
        String usuario = "seuusuario";
        String senha = "suasenha";
        
        try {
            return DriverManager.getConnection(url, usuario, senha);
        } catch (SQLException e) {
            e.printStackTrace();
            return null;
        }
    }
}
```

## 4. Comandos SQL

### 4.1 Criando e Removendo Banco de Dados
```sql
CREATE DATABASE meu_banco;
DROP DATABASE meu_banco;
```

### 4.2 Criando e Removendo Tabelas
```sql
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100),
    email VARCHAR(100)
);

DROP TABLE usuarios;
```

### 4.3 Execução no PostgreSQL com Java
```java
import java.sql.Connection;
import java.sql.Statement;

public class CriarTabela {
    public static void main(String[] args) {
        try (Connection conn = ConexaoPostgreSQL.conectar();
             Statement stmt = conn.createStatement()) {
            String sql = "CREATE TABLE clientes (id SERIAL PRIMARY KEY, nome VARCHAR(100));";
            stmt.executeUpdate(sql);
            System.out.println("Tabela criada com sucesso!");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 5. Truncando Tabela

### 5.1 Comando SQL
```sql
TRUNCATE TABLE usuarios;
```

### 5.2 Execução no PostgreSQL com Java
```java
try (Connection conn = ConexaoPostgreSQL.conectar();
     Statement stmt = conn.createStatement()) {
    stmt.executeUpdate("TRUNCATE TABLE usuarios;");
    System.out.println("Tabela truncada!");
} catch (Exception e) {
    e.printStackTrace();
}
```

## 6. Inserindo Dados

### 6.1 Comando SQL
```sql
INSERT INTO usuarios (nome, email) VALUES ('João Silva', 'joao@email.com');
```

### 6.2 Execução no PostgreSQL com Java
```java
try (Connection conn = ConexaoPostgreSQL.conectar();
     Statement stmt = conn.createStatement()) {
    stmt.executeUpdate("INSERT INTO usuarios (nome, email) VALUES ('Maria', 'maria@email.com');");
    System.out.println("Dados inseridos!");
} catch (Exception e) {
    e.printStackTrace();
}
```

## 7. Selecionando Dados

### 7.1 Comando SQL
```sql
SELECT * FROM usuarios;
```

### 7.2 Execução no PostgreSQL com Java
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

public class SelecionarDados {
    public static void main(String[] args) {
        try (Connection conn = ConexaoPostgreSQL.conectar();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM usuarios")) {
            
            while (rs.next()) {
                System.out.println("ID: " + rs.getInt("id") + ", Nome: " + rs.getString("nome"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## 8. Apresentação dos Dados no Console
```java
while (rs.next()) {
    System.out.println("ID: " + rs.getInt("id") + ", Nome: " + rs.getString("nome"));
}
```

## 9. Exemplo de JTable no Java

```java
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.sql.*;

public class TabelaUsuarios {
    public static void main(String[] args) {
        JFrame frame = new JFrame("Usuários");
        DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nome", "Email"}, 0);
        JTable table = new JTable(model);
        
        try (Connection conn = ConexaoPostgreSQL.conectar();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM usuarios")) {
            
            while (rs.next()) {
                model.addRow(new Object[]{rs.getInt("id"), rs.getString("nome"), rs.getString("email")});
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        
        frame.add(new JScrollPane(table));
        frame.setSize(500, 300);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

---

Este manual fornece uma introdução ao uso do SQL com Java e PostgreSQL. Com ele, você pode criar, manipular e visualizar dados de forma eficiente.
