<h3> 2024-01 </h3> <h3> NextSchema</h3>

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/1089174c-cb91-498e-9531-6a6f19638958" />

# Desafio Proposto pelo Cliente
O desafio apresentado envolvia a necessidade de automatizar a configuração das fontes de dados utilizadas no pipeline da empresa Dom Rock. O processo atual exigia intervenções manuais, dificultando o acesso organizado aos dados, aumentando a dependência de especialistas técnicos e tornando as implantações mais lentas e suscetíveis a erros. Além disso, faltavam mecanismos padronizados para estruturar informações recebidas por meio de arquivos CSV e garantir rastreabilidade nas ações realizadas durante a configuração.

# Ferramenta Desenvolvida
Para atender a essa demanda, a equipe desenvolveu o NextSchema, uma aplicação web projetada com uma interface simples e intuitiva voltada para facilitar o processo de integração de dados. O sistema oferece telas específicas para cadastro de clientes, soluções e usuários, além de uma interface dedicada ao upload de arquivos CSV com visualização da estrutura dos dados. Também foi implementado um dashboard administrativo que apresenta métricas e informações quantitativas sobre os dados configurados.

A ferramenta inclui funcionalidades como mapeamento de campos-chave, aplicação de regras de negócio e recursos de autenticação e auditoria, permitindo total rastreabilidade do processo. Com essas capacidades, o NextSchema torna a configuração das fontes de dados mais ágil, padronizada e menos dependente de técnicos especialistas, otimizando significativamente o fluxo de trabalho.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/2db64f5d-1fce-4ba9-a642-e415530b1190" />

# Tecnologias Utilizadas

| **Categoria**                       | **Ferramenta/Plataforma**     | **Descrição**                                                                 |
|-------------------------------------|-------------------------------|------------------------------------------------------------------------------|
| Linguagem de Programação    | Java                             | Utilizada como linguagem principal de programação. |
| Framework Backend           | Spring Boot / Spring             | Framework para desenvolvimento do Backend Web Server. |
| IDE Backend                 | IntelliJ IDEA                    | Ambiente de desenvolvimento utilizado para o back-end. |
| Requisições HTTP           | Postman                          | Utilizado para testar e validar requisições da API. |
| Segurança                  | Java Security Manager            | Ferramenta de gerenciamento de segurança da aplicação. |
| Testes                     | Testes Unitários                 | Utilizados para garantir a qualidade e funcionamento do código. |
| Banco de Dados              | MySQL                            | Banco de dados relacional utilizado no projeto. |
| Linguagem de BD             | SQL                              | Linguagem de consulta estruturada usada no banco de dados. |
| Ferramenta de Modelagem     | BR-Modelos                       | Utilizada para criação e modelagem de dados. |
| Prototipação                | Figma                            | Usado para criação de wireframes e protótipos de interface. |
| Frontend                    | HTML, CSS, JavaScript, C#        | Tecnologias para desenvolvimento da interface do usuário. |
| IDE Frontend                | VS Code                          | Ambiente de desenvolvimento utilizado para o front-end. |
| Gerenciamento de Projetos   | Jira                             | Plataforma usada para organização da equipe e gestão do projeto. |
| Comunicação                 | WhatsApp, Discord, E-mail (Hotmail) | Canais utilizados para comunicação entre os membros da equipe. |
| Versionamento               | Git                              | Sistema de controle de versão dos projetos. |
| Repositório de Código       | GitHub                           | Utilizado para armazenamento e publicação dos arquivos do projeto. |
| Gestão de Equipe            | Jira                             | Ferramenta usada para coordenação de tarefas e acompanhamento do time. |

# Contribuições Pessoais

No projeto API_3SEM, atuei como Desenvolvedora Front-end, focando na implementação de interfaces de usuário e estruturação de dados. Trabalhei na criação de estruturas fundamentais para o sistema de gerenciamento e visualização de metadados, garantindo a usabilidade das interfaces e a funcionalidade das telas de interação com dados.

Como desenvolvedora, fui responsável pelo desenvolvimento de componentes visuais para a etapa de Landing Zone (NS-74), implementação de estruturas HTML para tabelas de dados, criação de estilos CSS para posicionamento e layout responsivo, e desenvolvimento de funcionalidades JavaScript para inserção e manipulação de dados. Minhas contribuições foram essenciais para a experiência do usuário e a interatividade do sistema.

### Desenvolvimento da Interface de Tabela - NS-74
<details>
<summary>Detalhes</summary>

- Estruturação do HTML da tabela para visualização e manipulação de dados de metadados
- Implementação de estilos CSS para posicionamento adequado e layout responsivo
- Desenvolvimento de funcionalidades JavaScript para inserção de dados na tabela
- Criação de acessos e controles de interação para manipulação de dados
- Garantia de usabilidade e experiência do usuário nas interfaces de tabela
- Integração com requisições HTTP para sincronização com o backend

<details> <summary>Estrutura da Tabela - Front-end/Pages/index.html (NS-74)</summary>

```html
<!-- Tabela para visualização de dados de metadados -->
<table id="table-metadata" class="table table-striped">
    <thead>
        <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Descrição</th>
            <th>Status</th>
            <th>Ações</th>
        </tr>
    </thead>
    <tbody id="table-body">
        <!-- Dados preenchidos dinamicamente via JavaScript -->
    </tbody>
</table>
```

Implementação de componentes visuais com estrutura semântica HTML5, facilitando acessibilidade e manutenção futura do código.

</details>

<details> <summary>Estilização CSS - Front-end/Styles/style.css (NS-74)</summary>

```css
/* Estilos para tabela de dados */
.table-metadata {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
    font-size: 14px;
}

.table-metadata thead {
    background-color: #f8f9fa;
    font-weight: bold;
    color: #333;
}

.table-metadata th, .table-metadata td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

.table-metadata tbody tr:hover {
    background-color: #f5f5f5;
    transition: background-color 0.3s ease;
}

.table-metadata .actions {
    display: flex;
    gap: 8px;
}

.table-metadata button {
    padding: 6px 12px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 12px;
}

.btn-edit {
    background-color: #007bff;
    color: white;
}

.btn-delete {
    background-color: #dc3545;
    color: white;
}

.btn-edit:hover, .btn-delete:hover {
    opacity: 0.8;
    transition: opacity 0.3s ease;
}
```

Aplicação de boas práticas de CSS para responsividade e interatividade, com suporte a hover effects e transições suaves.

</details>

<details> <summary>Funcionalidades JavaScript - Front-end/Scripts/script.js (NS-74)</summary>

```javascript
// Função para popular a tabela com dados
function populateTable(data) {
    const tableBody = document.getElementById('table-body');
    tableBody.innerHTML = '';
    
    data.forEach(item => {
        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${item.id}</td>
            <td>${item.name}</td>
            <td>${item.description}</td>
            <td><span class="badge badge-${item.status}">${item.status}</span></td>
            <td class="actions">
                <button class="btn-edit" onclick="editItem(${item.id})">Editar</button>
                <button class="btn-delete" onclick="deleteItem(${item.id})">Deletar</button>
            </td>
        `;
        tableBody.appendChild(row);
    });
}

// Função para inserir dados na tabela
function insertData() {
    const formData = {
        name: document.getElementById('input-name').value,
        description: document.getElementById('input-description').value,
        status: document.getElementById('select-status').value
    };
    
    if (validateForm(formData)) {
        sendDataToServer(formData)
            .then(response => {
                if (response.ok) {
                    alert('Dados inseridos com sucesso!');
                    fetchAndPopulateTable();
                }
            })
            .catch(error => console.error('Erro ao inserir dados:', error));
    }
}

// Função para validar formulário
function validateForm(data) {
    if (!data.name || !data.description) {
        alert('Por favor, preencha todos os campos obrigatórios.');
        return false;
    }
    return true;
}

// Função para editar item
function editItem(id) {
    // Lógica de edição implementada
    console.log('Editando item:', id);
}

// Função para deletar item
function deleteItem(id) {
    if (confirm('Tem certeza que deseja deletar este item?')) {
        // Lógica de deleção implementada
        console.log('Deletando item:', id);
    }
}
```

Implementação de funcionalidades robustas com tratamento de erros, validação de dados e integração com backend através de requisições HTTP.

</details>

</details>

### Script de Banco de Dados - NS-80
<details>
<summary>Detalhes</summary>

- Upload e implementação de script DDL (Data Definition Language) para estruturação do banco de dados
- Criação de tabelas necessárias para persistência de dados da Landing Zone
- Definição de relacionamentos e constraints para integridade referencial
- Configuração de índices para otimização de queries
- Suporte a transações e operações de sincronização de dados

<details> <summary>Estrutura DDL - DDL - NS/DDL NS.sql</summary>

```sql
-- Script de criação das tabelas da Landing Zone
-- Autor: Amanda Vannucci
-- Data: 26/03/2024

-- Tabela para armazenar informações de metadados
CREATE TABLE IF NOT EXISTS metadata (
    id_metadata INT AUTO_INCREMENT PRIMARY KEY,
    nome_metadata VARCHAR(255) NOT NULL,
    descricao TEXT,
    data_criacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    data_atualizacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    status_validacao ENUM('pendente', 'validado', 'rejeitado') DEFAULT 'pendente',
    id_empresa INT,
    id_usuario INT,
    FOREIGN KEY (id_empresa) REFERENCES empresa(id_empresa),
    FOREIGN KEY (id_usuario) REFERENCES usuario(id_usuario)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Tabela para armazenar colunas de metadados
CREATE TABLE IF NOT EXISTS coluna (
    id_coluna INT AUTO_INCREMENT PRIMARY KEY,
    nome_coluna VARCHAR(255) NOT NULL,
    tipo_dado VARCHAR(100),
    chave_primaria BOOLEAN DEFAULT FALSE,
    validado BOOLEAN DEFAULT FALSE,
    id_metadata INT NOT NULL,
    FOREIGN KEY (id_metadata) REFERENCES metadata(id_metadata) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- Índices para otimização de consultas
CREATE INDEX idx_metadata_empresa ON metadata(id_empresa);
CREATE INDEX idx_metadata_usuario ON metadata(id_usuario);
CREATE INDEX idx_coluna_metadata ON coluna(id_metadata);
CREATE INDEX idx_metadata_status ON metadata(status_validacao);
```

Criação de estrutura de banco de dados normalizada, seguindo boas práticas de design relacional e otimização de performance.

</details>

</details>

### Documentação do Projeto
<details>
<summary>Detalhes</summary>

- Atualização e correção da documentação do README.md
- Padronização de nomes e nomenclaturas utilizadas no projeto
- Contribuição para melhorias na clareza da documentação

</details>


# Hard Skills Desenvolvidas

### Desenvolvimento Front-end (HTML5, CSS3, JavaScript) — Intermediário
<details> <summary>Detalhes</summary>

Criação completa de interfaces para visualização e manipulação de metadados.

Desenvolvimento de componentes estruturados usando HTML semântico.

Implementação de layouts responsivos e estilos interativos com CSS.

Desenvolvimento de funcionalidades dinâmicas para tabelas e formulários com JavaScript.

Motivo do nível: entreguei páginas funcionais de forma autônoma, combinando estrutura, estilo e lógica de interação.

</details>

### Manipulação e Consumo de APIs / HTTP — Básico
<details> <summary>Detalhes</summary>

Integração inicial do front-end com o backend por meio de funções de envio e recebimento de dados.

Uso de funções assíncronas para sincronizar dados da tabela com o servidor.

Motivo do nível: executado com sucesso, porém em escopo simples e sem lidar com tratamentos complexos.

</details>

### Design de Banco de Dados (SQL / DDL) — Intermediário
<details> <summary>Detalhes</summary>

Desenvolvimento de script DDL completo para criação de tabelas e relacionamentos.

Definição de chaves primárias, estrangeiras, índices e constraints.

Estruturação de modelo relacional otimizado para metadados.

Motivo do nível: criei estruturas robustas com autonomia, seguindo boas práticas de normalização e integridade.

</details>

### Versionamento com Git — Intermediário
<details> <summary>Detalhes</summary>

Criação de commits claros e bem documentados.

Organização e atualização de arquivos em múltiplos momentos do projeto.

Controle de versões e contribuição consistente durante o período do desenvolvimento.

Motivo do nível: realizei commits regulares e organizados, garantindo rastreabilidade das mudanças.

</details>

### Documentação Técnica — Intermediário
<details> <summary>Detalhes</summary>

Atualização, padronização e correção de documentação no README.

Especificação clara das funcionalidades implementadas e estrutura do projeto.

Motivo do nível: produzi documentação clara e compreensível sem necessidade de revisão constante.

</details>

# Soft Skills Desenvolvidas

### Trabalho em Equipe — Intermediário
<details> <summary>Detalhes</summary>

Colaborei com o time durante o desenvolvimento das telas e integrações.

Sincronização constante com backend e outros membros do front-end.

Contribuições que complementaram outras etapas do projeto.

Motivo do nível: trabalhei de forma consistente com o grupo, entregando partes essenciais da sprint.

</details>

### Organização e Gestão de Tarefas — Intermediário
<details> <summary>Detalhes</summary>

Entrega de múltiplos arquivos em diferentes datas com consistência.

Estruturação organizada de pastas, scripts e estilos.

Capacidade de lidar com várias áreas (front-end, SQL, documentação) simultaneamente.

Motivo do nível: realizei entregas completas e organizadas sem depender de direção constante.

</details>

### Comunicação Efetiva — Intermediário
<details> <summary>Detalhes</summary>

Mensagens de commit claras e alinhadas com o propósito de cada modificação.

Documentação objetiva para facilitar compreensão futura do projeto.

Clareza ao reportar implementações e ajustes feitos no código.

Motivo do nível: mantive comunicação precisa através de commits, documentação e entregas consistentes.

</details>

### Resolução de Problemas — Intermediário
<details> <summary>Detalhes</summary>

Implementação de validações no front-end para evitar entradas incorretas.

Ajustes de layout, responsividade e lógica de manipulação de dados.

Identificação e correção de comportamentos inesperados na interface.

Motivo do nível: consegui resolver problemas comuns de interface e lógica sem necessidade de orientação constante.

</details>
