# dashboard-_aplic_java_sql
Financial/Accounting Dashboard with SQL Server Integration

## Conectando ao SQL Server e Utilizando Views

Este dashboard financeiro foi projetado para se conectar diretamente às views do SQL Server. Aqui está como ele funciona:

### 1. Conexão com o SQL Server

O arquivo `lib/sql-server.ts` contém a configuração de conexão com o SQL Server e funções para executar consultas:

```javascript
// Exemplo de configuração
const sqlConfig = {
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_NAME,
  server: process.env.DB_SERVER,
  options: {
    encrypt: true,
    trustServerCertificate: true
  }
}
```

### 2. Views do SQL Server Utilizadas

O dashboard espera que as seguintes views existam no seu banco de dados SQL Server:

- **vw_MovimentacaoFinanceira**: Contém todas as transações financeiras

```sql
CREATE VIEW vw_MovimentacaoFinanceira AS
SELECT 
  id,
  data,
  tipo, -- 'RECEITA' ou 'DESPESA'
  valor,
  descricao,
  centro_custo
FROM MovimentacaoFinanceira;
```


- **vw_ContasReceber**: Contém as contas a receber

```sql
CREATE VIEW vw_ContasReceber AS
SELECT 
  id,
  cliente,
  documento,
  emissao,
  vencimento,
  valor,
  status -- 'A vencer' ou 'Vencido'
FROM ContasReceber;
```


- **vw_ContasPagar**: Contém as contas a pagar

```sql
CREATE VIEW vw_ContasPagar AS
SELECT 
  id,
  fornecedor,
  documento,
  emissao,
  vencimento,
  valor,
  status -- 'A vencer' ou 'Vencido'
FROM ContasPagar;
```




### 3. Server Actions para Buscar Dados

O arquivo `lib/actions.ts` contém Server Actions que podem ser chamadas diretamente dos componentes React:

```javascript
'use server'

export async function fetchFinancialMetrics() {
  // Busca métricas financeiras do SQL Server
}

export async function fetchRevenueExpenseData(startDate, endDate) {
  // Busca dados de receitas e despesas
}
```

### 4. API Routes para Acesso Externo

O arquivo `app/api/financial-data/route.ts` fornece endpoints de API para acessar os dados financeiros:

```plaintext
GET /api/financial-data?type=metrics
GET /api/financial-data?type=revenue-expense&startDate=2023-01-01&endDate=2023-12-31
GET /api/financial-data?type=profit-margin&startDate=2023-01-01&endDate=2023-12-31
GET /api/financial-data?type=cash-flow&startDate=2023-01-01&endDate=2023-12-31
GET /api/financial-data?type=receivables&status=A%20vencer
GET /api/financial-data?type=payables&status=Vencido
```

## Implementação em sua Aplicação Java

Para integrar este dashboard em sua aplicação Java existente, você tem várias opções:

### 1. Integração via API REST

A maneira mais simples é executar o dashboard Next.js como um serviço separado e integrá-lo à sua aplicação Java:

1. **Implante o dashboard Next.js** como um aplicativo independente
2. **Crie um iframe ou link** na sua aplicação Java que direcione para o dashboard
3. **Configure autenticação compartilhada** entre os sistemas


### 2. Incorporação Direta

Se você precisar incorporar o dashboard diretamente em sua aplicação Java:

```java
// Exemplo em um controlador Spring Boot
@Controller
public class DashboardController {
    
    @GetMapping("/dashboard")
    public String dashboard(Model model) {
        // Adicione quaisquer dados necessários ao modelo
        model.addAttribute("userRole", getCurrentUserRole());
        
        // Renderize a página que contém o iframe para o dashboard
        return "dashboard";
    }
}
```

E no arquivo JSP ou Thymeleaf:

```html
<!-- dashboard.jsp ou dashboard.html -->
<div class="dashboard-container">
    <iframe src="http://seu-dashboard-nextjs/dashboard" 
            width="100%" 
            height="800px" 
            frameborder="0">
    </iframe>
</div>
```

### 3. Compartilhamento de Banco de Dados

O dashboard Next.js e sua aplicação Java podem compartilhar o mesmo banco de dados SQL Server:

1. **Mesmas credenciais**: Configure o dashboard para usar as mesmas credenciais de banco de dados
2. **Views dedicadas**: Crie views específicas para o dashboard que não interfiram na aplicação principal
3. **Usuário somente leitura**: Para segurança, use um usuário de banco de dados com permissões somente leitura


## Configuração das Variáveis de Ambiente

Para conectar o dashboard ao seu SQL Server, configure as seguintes variáveis de ambiente:

```plaintext
DB_USER=seu_usuario
DB_PASSWORD=sua_senha
DB_NAME=seu_banco_de_dados
DB_SERVER=seu_servidor.database.windows.net
```

Você pode configurar essas variáveis no arquivo `.env.local` para desenvolvimento local ou nas configurações de ambiente do seu servidor de produção.

## Personalização para suas Necessidades

Este dashboard é altamente personalizável:

1. **Adicione novos relatórios**: Crie novas abas e componentes para relatórios específicos
2. **Modifique as consultas SQL**: Adapte as consultas para corresponder à estrutura do seu banco de dados
3. **Integre com outros sistemas**: Adicione APIs para integrar com outros sistemas da sua empresa
4. **Personalize a aparência**: Ajuste o tema, cores e layout para corresponder à identidade visual da sua empresa


## Próximos Passos

1. **Adapte as views SQL**: Ajuste as consultas para corresponder às suas views existentes
2. **Configure a autenticação**: Adicione autenticação para proteger o acesso ao dashboard
3. **Implemente exportação de relatórios**: Adicione funcionalidade para exportar relatórios em PDF, Excel, etc.
4. **Adicione alertas e notificações**: Configure alertas para métricas importantes


Você gostaria que eu detalhasse alguma parte específica da implementação ou explicasse como adaptar o dashboard para alguma necessidade específica do seu negócio?

Please make sure to add the following environment variables to your project:

DB_USER DB_PASSWORD DB_NAME DB_SERVER Submit
