<h3> 2023-02 </h3>
<h3>TGsync</h3>

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/2355cda3-8457-42c8-aed6-1db5eced73d2" />

# Desafio Proposto pelo Cliente

Neste desafio, o cliente foi o [Professor Emanuel Mineda Carneiro](https://fatecsjc-prd.azurewebsites.net/docentes-bd), que nos apresentou a necessidade de modernizar o processo de avaliação dos Trabalhos de Conclusão de Curso (TCC) dos alunos do 5º e 6º período do curso de Análise e Desenvolvimento de Sistemas da Fatec. O método utilizado até então era manual, o que gerava diversas limitações, como a suscetibilidade a erros, a dificuldade na consolidação dos dados de avaliação, a ausência de relatórios padronizados, o tempo elevado para análise das informações e a falta de integração entre etapas, notas e feedbacks.

# Ferramenta Desenvolvida

Para solucionar os desafios do processo manual de avaliação de TCCs, o grupo [Tech Horizon](https://github.com/TechHorizonBR) desenvolveu a aplicação desktop [TGsync](https://github.com/Amandavannuccic/API_2_Sem), construída em Java com uma interface gráfica desenvolvida em JavaFX, focada na usabilidade e organização das informações. A ferramenta permite a importação de dados dos alunos por meio de arquivos CSV e realiza a validação automática das informações cadastradas, garantindo maior precisão no processo.

Além disso, o sistema oferece recursos completos para professores, como o registro de notas e feedbacks individualizados, acompanhamento de entregas e geração automatizada de relatórios essenciais. Entre os relatórios disponíveis, destacam-se: alunos aptos à defesa, notas consolidadas, progresso das entregas e certificados de orientação. A aplicação utiliza um banco de dados relacional MySQL para armazenamento seguro e estruturado das informações, promovendo confiabilidade e agilidade na gestão acadêmica.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/915012df-9ce7-417d-a982-b0d006fa5520" />

# Tecnologias Utilizadas

| Categoria                           | Ferramenta/Plataforma        | Descrição                                                                 |
|------------------------------------|------------------------------|---------------------------------------------------------------------------|
| **Linguagem de Programação**       | Java                         | Linguagem principal utilizada no desenvolvimento da aplicação.           |
| **Interface Gráfica (GUI)**        | JavaFX                       | Biblioteca usada para criar a interface gráfica do sistema desktop.      |
| **Banco de Dados**                 | MySQL                        | Banco de dados relacional para armazenamento de dados dos alunos, orientadores e avaliações. |
| **IDE (Ambiente de Desenvolvimento)** | IntelliJ                   | Ambiente de desenvolvimento utilizado para programar e gerenciar o projeto. |
| **Controle de Versão**             | Git                          | Ferramenta utilizada para controle de versões do código-fonte.           |
| **Plataforma de Repositórios**     | GitHub                       | Repositório online para colaboração e hospedagem do código.              |
| **Ferramenta de Prototipagem**     | Figma                        | Utilizada para prototipar e desenhar a interface do sistema.             |
| **Ferramenta de Diagramação**      | draw.io (Diagrams.net)       | Usada para criar diagramas de processos e estrutura do sistema.          |
| **Modelagem de Banco de Dados**    | BRModelo                     | Ferramenta utilizada para modelar o banco de dados relacional.           |
| **Método de Gerenciamento de Projetos** | Trello                  | Metodologia Kanban usada para acompanhar o progresso e distribuição das tarefas. |
| **Plataforma de Comunicação**      | Discord                      | Canal principal de comunicação entre os membros do grupo.                |

# Contribuições Pessoais

No projeto TGSync, atuei como Desenvolvedora, focando na implementação da camada de persistência, modelos de dados e telas relacionadas ao gerenciamento de entregas e avaliações. Trabalhei na criação de estruturas fundamentais para o sistema de acompanhamento de Trabalhos de Graduação, garantindo a integridade dos dados e a funcionalidade das interfaces.

Como desenvolvedora, fui responsável pela arquitetura do banco de dados de entregas e notas, implementação de DAOs (Data Access Objects), criação de DTOs (Data Transfer Objects) e desenvolvimento de telas controllers que conectam a lógica de negócio à interface gráfica. Minhas contribuições foram essenciais para o fluxo de dados do sistema.

### Implementação da Camada de Persistência
<details>
<summary>Detalhes</summary>

- Atualização e manutenção da classe de conexão com o banco de dados (`ConexaoBD.java`).  
- Implementação de métodos robustos para acesso aos dados.  
- Garantia de integridade e consistência dos dados através de DAOs especializados.  
- Configuração de queries SQL otimizadas para consultas de notas e entregas.  
- Tratamento adequado de exceções e fechamento de conexões.  
- Suporte a operações transacionais quando necessário.

<details> <summary>Código – Classe ConexaoBD (Atualizada)</summary>

      ```java
      package Model.ConexaoBD;
      
      import java.sql.Connection;
      import java.sql.DriverManager;
      import java.sql.SQLException;
      
      public class ConexaoBD {
          
          private static final String URL = "jdbc:mysql://localhost:3306/tgsync_db";
          private static final String USER = "root";
          private static final String PASSWORD = "sua_senha";
          private static final String DRIVER = "com.mysql.cj.jdbc.Driver";
      
          // Método para estabelecer conexão com o banco de dados
          public static Connection ConexaoBD() throws SQLException, ClassNotFoundException {
              Class.forName(DRIVER);
              Connection connection = DriverManager.getConnection(URL, USER, PASSWORD);
              return connection;
          }
      
          // Método para testar a conexão
          public static boolean testarConexao() {
              try {
                  Connection connection = ConexaoBD();
                  if (connection != null && !connection.isClosed()) {
                      connection.close();
                      return true;
                  }
              } catch (SQLException | ClassNotFoundException e) {
                  e.printStackTrace();
              }
              return false;
          }
      
          // Método para fechar conexão com segurança
          public static void fecharConexao(Connection connection) {
              try {
                  if (connection != null && !connection.isClosed()) {
                      connection.close();
                  }
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      }
      ```
</details>
</details>

### Criação de Data Transfer Objects (DTOs)
<details>
<summary>Detalhes</summary>
  
- Desenvolvimento de `EntregaDTO.java` para encapsular dados de entregas.  
- Desenvolvimento de `NotaDTO.java` para encapsular dados de notas e feedbacks.  
- Definição de construtores, getters/setters e métodos de validação.  
- Estruturação clara dos dados para transferência entre camadas.  

<details> <summary>Código – NotaDTO — Encapsulamento de Dados de Notas</summary>

```java
public class NotaDTO {
    private Long id;
    private String feedback;
    private Double valor;
    private Long idAluno;
    private Long idEntrega;
    private Double media;

    // Construtor com média e ID do aluno
    public NotaDTO(Double media, Long idAluno){
        this.media = media;
        this.idAluno = idAluno;
    }

    // Construtor completo com ID
    public NotaDTO(Long id, String feedback, Double valor, Long idAluno, Long idEntrega){
        this.id = id;
        this.feedback = feedback;
        this.valor = valor;
        this.idAluno = idAluno;
        this.idEntrega = idEntrega;
    }

    // Construtor sem ID (para inserção no banco)
    public NotaDTO(String feedback, Double valor, Long idAluno, Long idEntrega) {
        this.feedback = feedback;
        this.valor = valor;
        this.idAluno = idAluno;
        this.idEntrega = idEntrega;
    }

    // Construtor vazio
    public NotaDTO(){

    }

    // Getters e Setters
    public Long getId() {
        return this.id;
    }
    
    public void setId (Long id) {
        this.id = id;
    }
    
    public String getFeedback(){
        return this.feedback;
    }
    
    public void setFeedback (String feedback) {
        this.feedback = feedback;
    }
    
    public Double getValor(){
        return this.valor;
    }
    
    public void setValor(Double valor) {
        this.valor = valor;
    }
    
    public Long getIdAluno(){
        return this.idAluno;
    }
    
    public void setIdAluno(Long idAluno) {
        this.idAluno = idAluno;
    }

    public Long getIdEntrega() {
        return idEntrega;
    }

    public void setIdEntrega(Long idEntrega) {
        this.idEntrega = idEntrega;
    }

    // Método para recuperar nome da entrega associada
    public String getEntregaNome(){
        EntregaDAO entregaDAO = new EntregaDAO();
        EntregaDTO entregaDTO = entregaDAO.getEntregaPorId(this.idEntrega);
        if(entregaDTO != null && entregaDTO.getTituloEntrega() != null){
            return entregaDTO.getTituloEntrega();
        }
        return "";
    }
}
```

</details>

<details> <summary>Código – EntregaDTO — Encapsulamento de Dados de Entregas</summary>

```java
public class EntregaDTO {
     private Long idEntrega;
     private Date dataEntrega;
     private String tituloEntrega;
     private Long idTurma;
     private String tipo;

     // Construtor completo com ID
     public EntregaDTO(Long idEntrega, Date dataEntrega, String tituloEntrega, Long idTurma, String tipo) {
          this.idEntrega = idEntrega;
          this.dataEntrega = dataEntrega;
          this.tituloEntrega = tituloEntrega;
          this.idTurma = idTurma;
          this.tipo = tipo;
     }

     // Construtor sem ID (para inserção)
     public EntregaDTO(Date dataEntrega, String tituloEntrega, Long idTurma, String tipo) {
          this.dataEntrega = dataEntrega;
          this.tituloEntrega = tituloEntrega;
          this.idTurma = idTurma;
          this.tipo = tipo;
     }

     // Construtor alternativo sem turma
     public EntregaDTO(Long idEntrega, Date dataEntrega, String tituloEntrega, String tipo) {
          this.idEntrega = idEntrega;
          this.dataEntrega = dataEntrega;
          this.tituloEntrega = tituloEntrega;
          this.tipo = tipo;
     }

     // Construtor vazio
     public EntregaDTO() {

     }

     // Getters e Setters
     public Long getIdEntrega(){
          return idEntrega;
     }
     
     public void setIdEntrega(Long idEntrega) {
          this.idEntrega = idEntrega;
     }
     
     public Date getDataEntrega(){
          return this.dataEntrega;
     }
     
     public void setDataEntrega (Date dataEntrega){
          this.dataEntrega = dataEntrega;
     }
     
     public String getTituloEntrega() {
          return tituloEntrega;
     }
     
     public void setTituloEntrega (String tituloEntrega) {
          this.tituloEntrega = tituloEntrega;
     }
     
     public Long getIdTurmas() {
          return this.idTurma;
     }
     
     public void setIdTurmas (Long idTurma) {
          this.idTurma = idTurma;
     }
     
     public String getTipo(){
          return this.tipo;
     }
     
     public void setTipo(String tipo){
          this.tipo = tipo;
     }

     // Método para recuperar nota de um aluno nesta entrega
     public String getNotaAlunos(Long idAluno){
          NotaDAO notaDAO = new NotaDAO();
          NotaDTO notaDTO = notaDAO.getNotaPorAlunoEntrega(idAluno, this.idEntrega);
          if(notaDTO != null){
               return notaDTO.getValor().toString();
          }
          return "Sem avaliação";
     }
}
```
</details>
</details>

### Implementação de Data Access Objects (DAOs)

<details> <summary>Detalhes</summary>

- Implementação da classe `NotaDAO.java` para operações CRUD de notas.  
- Criação de método especializado `getNotaPorAlunoEntrega()` para recuperar avaliações específicas.  
- Implementação de lógica para filtrar notas por aluno, entrega e sprint.  
- Integração com a classe `NotaDTO` para transferência estruturada de dados.  

<details> <summary>Código – NotaDAO — Gerenciamento de Notas e Avaliações</summary>

```java
public class NotaDAO {

    Connection connection = null;

    // Método para adicionar nota ao banco de dados
    public int addNota(NotaDTO notaDTO){
        PreparedStatement stmt = null;
        int success = 0;

        try{
            connection = ConexaoBD.ConexaoBD();
            String sql = "INSERT INTO nota (feedback, valor, idAluno, idEntrega) VALUES(?,?,?,?)";
            stmt = connection.prepareStatement(sql);
            stmt.setString(1, notaDTO.getFeedback());
            stmt.setDouble(2, notaDTO.getValor());
            stmt.setLong(3, notaDTO.getIdAluno());
            stmt.setLong(4, notaDTO.getIdEntrega());

            stmt.executeUpdate();

        } catch (SQLException e){
            success = 1;
            e.printStackTrace();
        } finally {
            try {
                if(connection != null) connection.close();
            } catch (SQLException e){
                e.printStackTrace();
            }
        }
        return success;
    }

    // Método para recuperar nota específica de um aluno em uma entrega
    public NotaDTO getNotaPorAlunoEntrega(Long alunoId, Long entregaId){
        PreparedStatement stmt = null;
        ResultSet rs = null;

        try{
            connection = ConexaoBD.ConexaoBD();
            String sql = "SELECT * FROM nota WHERE idAluno = ? and idEntrega = ?";
            stmt = connection.prepareStatement(sql);
            stmt.setLong(1, alunoId);
            stmt.setLong(2, entregaId);
            rs = stmt.executeQuery();
            
            while (rs.next()){
                NotaDTO notaDTO = new NotaDTO(
                    rs.getLong("id"), 
                    rs.getString("feedback"), 
                    rs.getDouble("valor"), 
                    rs.getLong("idAluno"), 
                    rs.getLong("idEntrega")
                );
                return notaDTO;
            }

        }catch (SQLException e) {
            e.printStackTrace();
        }finally {
            try {
                if(connection != null){
                    connection.close();
                }
            }catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return null;
    }

    // Método para recuperar múltiplas notas de um aluno em uma entrega
    public List<NotaDTO> getNotasPorAlunoEntrega(Long alunoId, Long entregaId){
        PreparedStatement stmt = null;
        ResultSet rs = null;
        List<NotaDTO> listNotas = new LinkedList<>();

        try{
            connection = ConexaoBD.ConexaoBD();
            String sql = "SELECT * FROM nota WHERE idAluno = ? and idEntrega = ?";
            stmt = connection.prepareStatement(sql);
            stmt.setLong(1, alunoId);
            stmt.setLong(2, entregaId);
            rs = stmt.executeQuery();
            
            while (rs.next()){
                listNotas.add(new NotaDTO(
                    rs.getLong("id"), 
                    rs.getString("feedback"), 
                    rs.getDouble("valor"), 
                    rs.getLong("idAluno"), 
                    rs.getLong("idEntrega")
                ));
            }

        }catch (SQLException e) {
            e.printStackTrace();
        }finally {
            try {
                if(connection != null){
                    connection.close();
                }
            }catch (SQLException e) {
                e.printStackTrace();
            }
        }
        return listNotas;
    }
}
```
</details>
</details>

### Desenvolvimento de Controllers e Telas
<details>
<summary>Detalhes</summary>
- Desenvolvimento do controller `TelaEntregasController.java` para gerenciamento visual de entregas.  
- Implementação de exibição em tabela (TableView) com dados carregados do banco.  
- Integração com `EntregaDTO` para preenchimento dinâmico de dados.  
- Funcionalidades de filtragem, busca e navegação entre entregas.  
- Sistema robusto de validação de dados de entrada.  
- Criação da interface visual correspondente `telaEntregas.fxml`. 

<details> <summary>Código – TelaEntregasController — Interface de Gerenciamento de Entregas</summary>

```java
public class TelaEntregasController extends MudancaTelas {

    @FXML
    private Button ButtonCadastrar;

    @FXML
    private TableColumn<EntregaDTO, Date> colunaDataEntregaTG;
    @FXML
    private TableColumn<EntregaDTO, String> colunaMatriculaTG;

    @FXML
    private TableColumn<EntregaDTO, String> colunaTituloTG;
    @FXML
    private TableColumn<EntregaDTO, String> colunaTipoTG;

    @FXML
    private TableView<EntregaDTO> tabelaEntregasTG1;

    @FXML
    private DatePicker dateDataEntrega;

    @FXML
    private TextField textFieldTitulo;

    @FXML
    ObservableList<EntregaDTO> obsListEntregasTG1 = FXCollections.observableArrayList();

    // Inicialização da tela e carregamento da tabela
    public void initialize() {
        try {
            updateTableTG();
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }

    // Método para adicionar entrega com validações
    @FXML
    public void adicionarEntrega(ActionEvent event) {
        String titulo = textFieldTitulo.getText();
        LocalDate data = dateDataEntrega.getValue();
        Integer tg = comboBoxTG.getValue();
        String tipoTg = comboBoxTipoTG.getValue();
        LocalDate dataAtual = LocalDate.now();
        boolean verificador = true;

        // Validação do título
        if (titulo.equals("")) {
            Alerts.showAlert("Atenção!", "", "Não é possível cadastrar uma entrega com o título vazio.", Alert.AlertType.WARNING);
            verificador = false;
        }
        // Validação da data
        else if (data.isBefore(dataAtual)) {
            Alerts.showAlert("Atenção!", "", "Não é possível inserir uma data anterior a atual.", Alert.AlertType.WARNING);
            verificador = false;
        }
        // Validação do tipo de TG
        else if (tipoTg.equals("")) {
            Alerts.showAlert("Atenção!", "", "É obrigatório selecionar um tipo de TG.", Alert.AlertType.WARNING);
            verificador = false;
        }
        // Validação adicional para portfólio
        else if (tipoTg.equals("Portfólio")) {
            if(tg == null){
                Alerts.showAlert("Atenção!", "", "É necessário selecionar o TG.", Alert.AlertType.WARNING);
                verificador = false;
            }
        }

        if (verificador){
            EntregaDAO entregaDAO = new EntregaDAO();
            TurmaDAO turmaDAO = new TurmaDAO();
            TurmaDTO turmaService = new TurmaDTO();

            try {
                turmaService = TurmaService.buscarTurmaComDataDoPC();
                // Prosseguir com a adição da entrega
            } catch (ParseException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

</details>
</details>

### TelaPendenciasController — Monitoramento de Tarefas Pendentes

<details> <summary>Detalhes</summary>
  
- Criação de `TelaPendenciasSceneBilder.fxml` para definição da interface.  
- Ligação entre controller e view para exibição de pendências.  
- Carregamento dinâmico de dados através de `NotaDAO`.  
- Atualização em tempo real das pendências dos alunos.  
- Implementação de métodos para filtragem e ordenação de dados.  


<details> <summary>Código – Listagem de Pendências</summary>

```java
public class TelaPendenciasController {
    
    private List<NotaDTO> notasPendentes;
    
    // Carregamento de notas pendentes para um aluno específico
    public void carregarNotasPendentes(int idAluno) {
        NotaDAO notaDAO = new NotaDAO();
        notasPendentes = notaDAO.getNotasPendentes(idAluno);
        
        // Preenchimento da interface com as notas pendentes
        for (NotaDTO nota : notasPendentes) {
            // Exibição estruturada das notas pendentes
            adicionarLinhaTabela(nota);
        }
    }
    
    // Método auxiliar para adicionar linha à tabela
    private void adicionarLinhaTabela(NotaDTO nota) {
        // Preenchimento dinâmico da interface com dados de pendências
        atualizarVisualizacao(nota);
    }

    // Método para atualizar a visualização da tela
    private void atualizarVisualizacao(NotaDTO nota) {
        // Lógica para exibir dados no TableView ou elementos da tela
        String nomEntrega = nota.getEntregaNome();
        Double valor = nota.getValor();
        String feedback = nota.getFeedback();
        
        // Adicionar à interface visual
    }

    // Método para filtrar pendências por status
    public List<NotaDTO> filtrarPendenciasPorStatus(String status) {
        List<NotaDTO> pendenciasFiltrádas = new LinkedList<>();
        
        for (NotaDTO nota : notasPendentes) {
            if (nota.getStatus().equals(status)) {
                pendenciasFiltrádas.add(nota);
            }
        }
        
        return pendenciasFiltrádas;
    }
}
```

</details>
</details>

### Estruturação do Modelo de Dados
<details><summary>Detalhes</summary>

- Definição clara dos atributos de `EntregaDTO` (data de entrega, status, descrição).  
- Estruturação de `NotaDTO` com campos para avaliação, feedback e situação.  
- Criação de relacionamentos entre Aluno, Entrega, Nota e Turma.  
- Validação de dados em nível de DTO.  

<details> <summary>Código – Estrutura de Banco de Dados Implementada</summary>
    
    ```sql
    -- Tabela de Entregas
    CREATE TABLE entrega (
        id LONG PRIMARY KEY AUTO_INCREMENT,
        dataEntrega DATE NOT NULL,
        tituloEntrega VARCHAR(255) NOT NULL,
        idTurma LONG NOT NULL,
        tipo VARCHAR(50) NOT NULL,
        FOREIGN KEY (idTurma) REFERENCES turma(id)
    );
    
    -- Tabela de Notas
    CREATE TABLE nota (
        id LONG PRIMARY KEY AUTO_INCREMENT,
        feedback TEXT,
        valor DOUBLE,
        idAluno LONG NOT NULL,
        idEntrega LONG NOT NULL,
        dataAvaliacao TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        FOREIGN KEY (idAluno) REFERENCES aluno(id),
        FOREIGN KEY (idEntrega) REFERENCES entrega(id)
    );
    ```
  </details>
</details>


# Hard Skills Desenvolvidas - [Tabela de Níveis](https://github.com/Amandavannuccic/TG-Fatec/blob/main/TabelaDeN%C3%ADveis/TabelaNiveis.md)

### Desenvolvimento Java — Intermediário
<details> <summary>Detalhes</summary>

- Criação de classes estruturadas com padrão MVC.  
- Implementação de DTOs e DAOs com boas práticas.  
- Uso de coleções, streams e processamento de dados.  
- Motivo do nível: desenvolvi múltiplas classes e controllers com autonomia, aplicando padrões de design.  

</details>

### Banco de Dados e SQL — Intermediário
<details> <summary>Detalhes</summary>

- Escrita de queries otimizadas para consultas de notas e entregas.  
- Implementação de métodos para filtragem por aluno, turma e sprint.  
- Gerenciamento de conexões e integridade de dados.  
- Motivo do nível: implementei queries complexas e resolvi problemas de acesso a dados com segurança.  

</details>

### Modelos de Dados (DTOs) — Intermediário
<details> <summary>Detalhes</summary>

- Criação de DTOs bem estruturados com validações.  
- Implementação de construtores sobrecarregados e métodos auxiliares.  
- Integração entre múltiplos DTOs para fluxos complexos.  
- Motivo do nível: design e implementação de modelos completos com autonomia.  

</details>

### Desenvolvimento de Interfaces JavaFX/FXML — Intermediário
<details> <summary>Detalhes</summary>

- Criação de telas FXML (`telaEntregas.fxml`, `TelaPendenciasSceneBilder.fxml`).  
- Desenvolvimento de controllers associados com lógica interativa.  
- Integração de TableView com dados do banco de dados.  
- Motivo do nível: desenvolvi interfaces completas conectadas à lógica de negócio.  

</details>

### Manipulação de Dados em Coleções — Intermediário
<details> <summary>Detalhes</summary>

- Filtragem e processamento de listas de objetos.  
- Implementação de lógicas de ordenação e agrupamento.  
- Cálculo de médias, somatórios e estatísticas de notas.  
- Motivo do nível: resolvi desafios complexos de manipulação de dados com autonomia.  

</details>

### Controle de Versão com Git — Intermediário
<details> <summary>Detalhes</summary>

- Versionamento consistente com commits descritivos.  
- Estruturação de branches para sprints (Sprint2, Sprint3).  
- Merges e colaboração com outros desenvolvedores.  
- Motivo do nível: utilizei Git eficientemente em fluxos colaborativos com múltiplos contribuidores.  

</details>

### Documentação Técnica e Diagramas — Intermediário
<details> <summary>Detalhes</summary>

- Criação de diagrama de classes UML v.1 (`Diagrama_de_Classe UML .drawio.png`).  
- Documentação de estruturas de dados e fluxos.  
- Especificação clara de métodos e responsabilidades.  
- Motivo do nível: produzi documentação visual e técnica clara e completa.  

</details>

# Soft Skills Desenvolvidas - [Tabela de Níveis](https://github.com/Amandavannuccic/TG-Fatec/blob/main/TabelaDeN%C3%ADveis/TabelaNiveis.md)

### Trabalho em Equipe — Intermediário
<details> <summary>Detalhes</summary>

- Colaboração contínua com outros desenvolvedores em diferentes funcionalidades.  
- Participação em merges e resolução de conflitos.  
- Compartilhamento de conhecimento sobre padrões e estruturas.  
- Motivo do nível: trabalhei de forma colaborativa em sprints com múltiplos membros.  

</details>

### Comunicação Efetiva — Intermediário
<details> <summary>Detalhes</summary>

- Descrição clara de commits para rastreamento de mudanças.  
- Comunicação sobre dependências e interfaces entre módulos.  
- Documentação compreensível para facilitar manutenção futura.  
- Motivo do nível: mantive comunicação clara através de commits, código e documentação.  

</details>

### Organização e Planejamento — Intermediário
<details> <summary>Detalhes</summary>

- Estruturação de tarefas por sprint (Sprint 2, Sprint 3).  
- Priorização de funcionalidades críticas (banco de dados, DAOs).  
- Cumprimento de prazos de entrega de componentes.  
- Motivo do nível: planejei e executei entregáveis complexos dentro dos prazos.  

</details>

### Autonomia — Intermediário
<details> <summary>Detalhes</summary>

- Desenvolvimento independente de DAOs completos.  
- Tomada de decisões sobre estrutura de dados e padrões.  
- Resolução autônoma de problemas de integração.  
- Motivo do nível: implementei funcionalidades complexas com pouca necessidade de orientação.  

</details>

### Flexibilidade — Intermediário
<details> <summary>Detalhes</summary>

- Adaptação a mudanças de requisitos entre sprints.  
- Refatoração de código para atender novos padrões.  
- Ajustes na estrutura de dados conforme feedback.  
- Motivo do nível: respondi bem a mudanças e reformulei soluções quando necessário.  

</details>

### Pensamento Crítico — Intermediário
<details> <summary>Detalhes</summary>

- Avaliação de diferentes abordagens para persistência de dados.  
- Identificação de inconsistências em queries e lógica de negócio.  
- Proposição de melhorias na estrutura de DAOs.  
- Motivo do nível: analisei problemas profundamente e propus soluções estruturadas.  

</details>

### Resolução de Problemas — Intermediário
<details> <summary>Detalhes</summary>

- Solução de problemas de conexão com banco de dados.  
- Correção de queries e filtros complexos.  
- Ajustes em mapeamento entre DTOs e dados do banco.  
- Motivo do nível: enfrentei desafios técnicos diversos e resolvi com autonomia.  

</details>

### Gestão do Tempo — Intermediário
<details> <summary>Detalhes</summary>

- Entrega de componentes críticos dentro das sprints.  
- Priorização eficiente entre tarefas de banco de dados e interface.  
- Cumprimento de deadlines sem comprometer qualidade.  
- Motivo do nível: gerenciei múltiplas demandas e entreguei conforme prazos estabelecidos.  

</details>

### Atenção aos Detalhes — Intermediário
<details> <summary>Detalhes</summary>

- Validação de integridade de dados em DAOs.  
- Verificação de consistência entre DTOs e banco de dados.  
- Testes de funcionalidades implementadas.  
- Motivo do nível: mantive alta qualidade através de validações e testes frequentes.  

</details>
