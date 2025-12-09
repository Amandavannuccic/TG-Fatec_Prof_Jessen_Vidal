<h3>2024-02 </h3>
<h3>LocalTrack</h3>


<p align="center">
  <img src="https://github.com/user-attachments/assets/8242e90e-47f8-4009-bbf5-aa4a7ff4c403" width="600" height="auto">
</p>


# Desafio Proposto pelo Cliente

Neste projeto, o cliente foi a empresa [ITO1](https://www.linkedin.com/company/ito1/?originalSubdomain=br), especializada em soluções de dados e Internet das Coisas (IoT). A ITO1 nos apresentou a necessidade de gerenciar de forma eficiente e escalável os grandes volumes de dados gerados por seus dispositivos IoT, especialmente voltados para a geolocalização de pessoas, dispositivos e objetos. O desafio envolvia garantir alta disponibilidade, integridade e rastreabilidade dessas informações armazenadas em banco de dados relacional, facilitando o registro e a consulta de dados em tempo real.

# Ferramenta Desenvolvida

Para atender às demandas da ITO1, o grupo [Tech Horizon](https://github.com/TechHorizonBR) desenvolveu o [LocalTrack](https://github.com/TechHorizonBR/API_4SEM), um sistema web escalável e intuitivo, com uma interface amigável para visualização em mapas. A ferramenta integra o registro e a consulta dos dados de geolocalização dos dispositivos, oferecendo filtros avançados para pesquisas detalhadas e definição de zonas com alertas automáticos. Além disso, o sistema permite o acompanhamento em tempo real dos dispositivos, mantém o histórico completo de localizações e conta com um sistema de gestão de usuários com autenticação para controle de acesso. O GeoTrack utiliza um banco de dados relacional robusto, garantindo alta disponibilidade e confiabilidade.

  <img width="1359" height="705" alt="image" src="https://github.com/user-attachments/assets/6a71cdca-6de3-48cc-b5d6-ac4b6b9ef14a" />

# Tecnologias Utilizadas

| Categoria                      | Ferramenta/Plataforma  | Descrição                                                                    |
|-------------------------------|-----------------------|------------------------------------------------------------------------------|
| **Linguagem de Programação**  | Java 17               | Versão estável e suportada da linguagem Java, com melhorias de desempenho e recursos modernos. |
| **Framework Backend**          | Spring Boot           | Framework que facilita o desenvolvimento rápido de aplicações e APIs Java com configurações automáticas. |
| **Segurança**                 | Spring Security       | Módulo para implementar autenticação e autorização nas aplicações Spring.     |
| **ORM**                       | Hibernate             | Framework ORM que possibilita a interação eficiente entre o código Java e bancos de dados relacionais. |
| **Linguagem de Marcação**      | HTML                  | Linguagem usada para estruturar o conteúdo das páginas web.                    |
| **Estilo**                    | CSS                   | Linguagem de estilo para customização visual de páginas web.                   |
| **Linguagem de Programação**  | JavaScript            | Linguagem para adicionar interatividade às aplicações web.                     |
| **Banco de Dados**            | Oracle                | Sistema de gerenciamento de banco de dados relacional utilizado para armazenar e consultar os dados. |
| **Gerenciamento de Estado**   | Pinia                 | Gerenciador de estado para aplicações front-end construídas com Vue.js.       |
| **Framework Frontend**        | Vue.js                | Framework JavaScript para criação de interfaces de usuário reativas e dinâmicas. |
| **Ferramenta de Prototipagem**| Figma                 | Ferramenta utilizada para prototipação e design de interfaces do usuário.      |

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

### Serviços / Lógica de Negócio
<details> <summary>Detalhes</summary>

Implementação de regras para criação e validação de demarcações.

Conversão de listas de coordenadas em objetos geométricos (JTS).

Validação de polígonos para evitar geometrias inválidas.

Persistência de múltiplas demarcações em lote.

Tratamento de erros e exceções de negócio.

<details> <summary>Código – Trecho do método de criação (DemarcacaoServiceImpl)</summary>
  
    public List<Demarcacao> saveDemarcacoes(String nome, Long usuarioId, List<List<List<Double>>> polygonsCoordinates) {
        Optional<Usuario> optUsuario = usuarioRepository.findById(usuarioId);
    if (optUsuario.isEmpty()) {
        throw new IllegalArgumentException("Usuário inexistente");
    }

    GeometryFactory geometryFactory = new GeometryFactory();
    for (List<List<Double>> coordinates : polygonsCoordinates) {
        Coordinate[] coordinateArray = coordinates.stream()
                .map(coord -> new Coordinate(coord.get(0), coord.get(1)))
                .toArray(Coordinate[]::new);

        LinearRing linearRing = geometryFactory.createLinearRing(coordinateArray);
        Polygon espacoGeometrico = geometryFactory.createPolygon(linearRing);

        if (!espacoGeometrico.isValid()) {
            throw new IllegalArgumentException("Geometria inválida para um dos polígonos");
        }

        demarcacoes.add(demarcacaoRepository.save(demarcacao));
    }

    return demarcacoes;
      }

</details> </details>

### Controllers / Endpoints
<details> <summary>Detalhes</summary>

Criação e finalização de endpoints REST para criação, listagem e exclusão de demarcações.

Implementação de validações no recebimento da requisição.

Tratamento completo de respostas HTTP (400, 404, 201, 200).

Estruturação de rotas e boas práticas de API REST.

<details> <summary>Código – Trecho do endpoint de criação (DemarcacaoController)</summary>
  
        @PostMapping
        public ResponseEntity<List<Demarcacao>> createDemarcacoes(@RequestBody Map<String, Object> requestData) {
        if (!requestData.containsKey("nome") || !requestData.containsKey("usuarioId")
                || !requestData.containsKey("coordinates")) {
            return ResponseEntity.status(HttpStatus.BAD_REQUEST).build();
        }
    
        String nome = (String) requestData.get("nome");
        Long usuarioId = ((Number) requestData.get("usuarioId")).longValue();
        List<List<List<Double>>> polygonsCoordinates = (List<List<List<Double>>>) requestData.get("coordinates");
    
        List<Demarcacao> demarcacoes = demarcacaoService.saveDemarcacoes(nome, usuarioId, polygonsCoordinates);
        return ResponseEntity.status(HttpStatus.CREATED).body(demarcacoes);
    }
    
</details> 

<details> <summary>Código – Trecho do endpoint GET /demarcacoes/user/{id}</summary>
  
        @GetMapping("/user/{id}")
        public ResponseEntity<List<Demarcacao>> getDemarcacaoByUsuario(@PathVariable Long id) {
        List<Demarcacao> demarcacoes = demarcacaoService.getDemarcacaoByUsuarioId(id);
        
        if (demarcacoes.isEmpty()) {
            return ResponseEntity.notFound().build();
        }
    
        return ResponseEntity.ok(demarcacoes);
      }

</details> 
</details> 
</details> 

### DTOs e Entidades
<details> <summary>Detalhes</summary>

Criação de DTOs para transporte de dados entre entidade e API.

Atualização de entidades JPA com novos campos e relacionamentos.

Uso de @JsonAlias para compatibilidade com diferentes formatos JSON.

Refatoração para remover imports desnecessários e padronizar nomes.

<details> <summary>Código – DemarcacaoDTO (Atualizado)</summary>
  
      public record DemarcacaoDTO(
          @JsonAlias("id") Long id,
          @JsonAlias("nome") String nome,
          @JsonAlias("coordenadas") List<List<Double>> coordinate
      ) {}

</details> <details> <summary>Código – Usuario.java (Trecho)</summary>
  
    @Entity
    public class Usuario {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
        @Column(name = "nome", length = 200)
        private String nome;
    
        @OneToOne(mappedBy = "usuario", fetch = FetchType.LAZY)
        private Device device;
    }

</details> 
</details>

### Repositórios / Consultas
<details> <summary>Detalhes</summary>

Criação de repositórios JPA.

Implementação de consultas customizadas.

Adição de métodos utilitários para melhorar desempenho de busca.

<details> <summary>Código – DeviceRepository</summary>
  
    public interface DeviceRepository extends JpaRepository<Device, Long> {
        Optional<Device> findByCodigo(String codigo);
    }
    
    </details> <details> <summary>Código – DemarcacaoRepository (Trecho)</summary>
    public interface DemarcacaoRepository extends JpaRepository<Demarcacao, Long> {
        List<Demarcacao> findDemarcacaoByUsuarioId(Long usuarioId);
    }

</details>
</details>

# Hard Skills Desenvolvidas

### Desenvolvimento Backend com Java 17 + Spring Boot — Intermediário
<details><summary>Detalhes</summary>

Implementação de serviços, controllers, lógica de negócio e manipulação de entidades.

Criação de endpoints REST completos (POST, GET) com tratamento de erros e validações.

Uso de boas práticas de arquitetura em camadas (Controller → Service → Repository).

Motivo do nível: desenvolvi funcionalidades completas com autonomia, resolvendo demandas reais do sistema.

</details>

### JPA / Hibernate e Persistência de Dados — Intermediário
<details><summary>Detalhes</summary>

Implementação de repositórios JPA para consultas específicas e operações CRUD.

Criação e mapeamento de entidades, incluindo relacionamentos (ex.: @OneToOne).

Uso de DAOs, DTOs e camada de persistência estruturada para garantir integridade do banco.

Motivo do nível: implementei consultas, entidades e integrações com autonomia e consistência.

</details>

### Manipulação de Geometrias (JTS Topology Suite) — Intermediário
<details><summary>Detalhes</summary>

Conversão de coordenadas em objetos geométricos (Coordinate, Polygon, LinearRing).

Validação de polígonos para evitar geometrias inválidas.

Processamento de múltiplas demarcações de forma automatizada.

Motivo do nível: implementei lógica geométrica completa, envolvendo cálculos e validações especializadas.

</details>

### APIs REST e Boas Práticas de Arquitetura — Intermediário
<details><summary>Detalhes</summary>

Criação de endpoints REST seguros e padronizados (status code, validações, estrutura de resposta).

Processamento de JSON de entrada usando Map, DTOs e alias de atributos.

Projeto de rotas claras e integradas à lógica de negócio.

Motivo do nível: projetei e implementei endpoints reais utilizados pelo sistema.

</details>

### Tratamento de Exceções e Validações — Intermediário
<details><summary>Detalhes</summary>

Implementação de verificações de dados no controller e service.

Retorno adequado de status (400, 404, 201, 200).

Garantia de segurança, consistência e mensagens claras ao cliente.

Motivo do nível: tratei casos de erro comuns e críticos durante o fluxo das requisições.

</details>

### Modelagem de Dados e Arquitetura de Banco de Dados — Intermediário
<details><summary>Detalhes</summary>

Desenvolvimento da camada de persistência, entidades e relacionamento entre tabelas.

Configuração de conexão com o banco, testes, fechamento seguro e tratamento de erros.

Suporte à integridade, rastreabilidade e performance das consultas.

Motivo do nível: desenvolvi modelos de dados funcionais e integrei com o restante da aplicação.

</details>

### Versionamento com Git — Básico
<details><summary>Detalhes</summary>

Criação de commits organizados e acompanhamento do repositório.

Resolução de conflitos simples e manutenção de histórico limpo.

Motivo do nível: utilizo Git com segurança no básico, mas sem estratégias avançadas de branches ou workflows complexos.

</details>

### Documentação Técnica — Intermediário
<details><summary>Detalhes</summary>

Estruturação clara das contribuições e documentação do funcionamento das camadas implementadas.

Organização técnica do fluxo da aplicação.

Motivo do nível: consigo documentar funcionalidades de forma clara e completa.

</details>

# Soft Skills Desenvolvidas

### Trabalho em Equipe — Intermediário
<details><summary>Detalhes</summary>

Cooperação contínua durante o desenvolvimento das camadas backend.

Alinhamento com equipe sobre responsabilidades e integrações entre módulos.

Motivo do nível: trabalhei de forma ativa, colaborando e apoiando o time no desenvolvimento.

</details>

### Comunicação Efetiva — Intermediário
<details><summary>Detalhes</summary>

Comunicação com a equipe e explicação das decisões técnicas tomadas.

Compartilhamento constante do progresso e alinhamento com o grupo.

Motivo do nível: mantive comunicação clara e constante durante o desenvolvimento.

</details>

### Resolução de Problemas — Intermediário
<details><summary>Detalhes</summary>

Solução de erros de mapeamento, geometrias inválidas, validações e inconsistências de persistência.

Ajustes de lógica de negócio e tratamento de exceções.

Motivo do nível: enfrentei desafios técnicos variados e resolvi a maioria com autonomia.

</details>

### Organização e Planejamento — Intermediário
<details><summary>Detalhes</summary>

Estruturação da lógica de negócio em etapas claras.

Gerenciamento do fluxo de implementação por partes (persistência → services → controllers).

Motivo do nível: organizei as tarefas com clareza e priorizei com eficiência.

</details>

### Autonomia — Intermediário
<details><summary>Detalhes</summary>

Desenvolvimento completo de módulos sem necessidade de supervisão constante.

Tomada de decisões técnicas com base em boas práticas.

Motivo do nível: executei grande parte do backend de forma independente.

</details>

### Pensamento Crítico — Intermediário
<details><summary>Detalhes</summary>

Identificação de falhas no fluxo de dados e correção de pontos críticos.

Análise da melhor forma de criar, validar e persistir demarcações.

Motivo do nível: tive que avaliar cenários complexos e definir soluções adequadas.

</details>

### Adaptabilidade / Flexibilidade — Intermediário
<details><summary>Detalhes</summary>

Adaptação a mudanças no formato de dados, fluxos de validação e regras da API.

Ajustes em função de novas demandas e feedbacks da equipe.

Motivo do nível: adaptações constantes do código conforme o projeto evoluía.

</details>

### Gestão do Tempo — Intermediário
<details><summary>Detalhes</summary>

Organização das entregas por etapas e cumprimento dos prazos.

Distribuição eficiente do tempo entre desenvolvimento, revisão, testes e documentação.

Motivo do nível: gerenciei bem minhas entregas dentro do ciclo do projeto.

</details>
