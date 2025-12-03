<h3> Em 2025-2 </h3>

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/320e2699-9820-4aec-8079-4085d800336a" />


<p align="center">
	
</p>

<b>Desafio Proposto</b>

Desenvolver um backend robusto para gerenciar tickets, usuários e fornecer análises automáticas (FAQ/embeddings, sentimentos e predições por período) para apoiar o processo de tomada de decisão. O objetivo incluiu a criação de rotas de API, persistência em banco de dados, integração com componentes de Machine Learning para inferência de FAQ e geração de métricas/relatórios.

<b>Ferramenta Desenvolvida</b>

O repositório contém a implementação de uma API (módulos em `api_6sem_back_end/`) responsável por:

- Autenticação e gerenciamento de usuários
- Endpoints para criação, consulta, atualização e exclusão de tickets
- Processamento e agregação de métricas por período (mês, período customizado)
- Integração com componentes de ML para: inferência de FAQ via embeddings, treinamento de modelos e análise de tendência
- Persistência e manipulação de dados no banco (camada em `db/`)
- Estrutura em camadas: `routers/`, `services/`, `repositories/`, `models/`, `ml/`, `utils/` para facilitar manutenção e testes

<h3>Tecnologias Utilizadas</h3>

| **Categoria**                       | **Ferramenta/Plataforma**     | **Descrição**                                                                 |
|-------------------------------------|-------------------------------|------------------------------------------------------------------------------|
| Linguagem de Programação            | Python                        | Linguagem principal do backend e scripts de ML.                              |
| Framework Web/API                   | FastAPI (estrutura de routers)| Organização dos endpoints em `routers/` e execução via `main.py`.            |
| Banco de Dados                      | MongoDB                       | Armazenamento de tickets, usuários e logs (configurações em `db/`).         |
| Machine Learning                    | SentenceTransformers, NumPy   | Embeddings para FAQ, rotinas de inferência e treinamentos em `ml/`.        |
| Bibliotecas Auxiliares               | pandas, scikit-learn (opcional)| Manipulação de dados e modelos auxiliares para análises e previsões.        |
| Ferramenta de Logs                   | Módulo interno (`utils_logs`) | Centralização de logs e auditoria via `repositories/repository_create_logs.py`.
| IDE (Desenvolvimento)               | VSCode                        | Ambiente utilizado para desenvolvimento e depuração.                         |
| Controle de Versão                   | Git                           | Versionamento e colaboração via branches e PRs.                             |

<h3>Contribuições Pessoais</h3>

No desenvolvimento deste backend, minhas contribuições principais foram:

- Endpoints & Roteamento

<details>

Criação e organização de múltiplos endpoints em `api_6sem_back_end/routers/` para autenticação (`router_login.py`), gestão de usuários, criação/consulta/atualização de tickets e consultas analíticas (por período, SLA, recorrência etc.).

📸 Sugestão de print: Tela do Postman/Insomnia com chamadas para endpoints ou trechos de `router_login.py`.

</details>

- Lógica de Negócio / Serviços

<details>

Implementação das regras de negócio nos serviços em `api_6sem_back_end/services/` — agregações temporais, cálculo de métricas, integração com módulos de ML para inferência de FAQ e previsões.

📸 Sugestão de print: Resultado de uma rota analítica ou trecho de `service_tickets_by_month.py`.

</details>

- Acesso a Dados / Persistência

<details>

Desenvolvimento da camada de persistência em `api_6sem_back_end/db/` (`db_configuration.py`, `db_mongo_manipulate_data.py`, `db_process_data.py`), incluindo configuração de collections, transformações antes de salvar e consultas otimizadas para relatórios.

📸 Sugestão de print: Diagrama de collections ou comando de consulta no MongoDB.

</details>

- Repositórios & Segurança

<details>

Implementação de repositórios em `repositories/` para operações específicas (criação de usuário, autenticação segura, logging), isolando a lógica de persistência e facilitando testes e auditoria.

📸 Sugestão de print: Trecho de `repository_login_security.py` mostrando verificação/geração de tokens.

</details>

- Modelagem e Estrutura do Código

<details>

Definição de modelos em `models/` (`model_user.py`, `model_ticket.py`, `model_store.py`) para padronizar os dados trocados entre camadas e garantir consistência nas operações.

📸 Sugestão de print: Exemplo de payload JSON de criação de ticket.

</details>

- Machine Learning / Inference

<details>

Integração e scripts em `ml/` para inferência de FAQ (uso de embeddings), scripts de treinamento e geração de artefatos (`ml/artifacts/`) utilizados em rotas de `router_predict_faq.py`.

📸 Sugestão de print: Exemplo de inferência de FAQ ou arquivos gerados em `ml/artifacts/`.

</details>

<h3>Hard Skills Desenvolvidas</h3>

- **Programação em Python** — Desenvolvimento e organização de um backend modular, manipulação de pacotes e scripts de ML.
- **APIs REST** — Projeto e implementação de endpoints, autenticação e versionamento de rotas.
- **Banco de Dados NoSQL (MongoDB)** — Modelagem de collections, queries e otimizações para relatórios.
- **Machine Learning Aplicado** — Uso de embeddings (SentenceTransformers) para inferência de FAQ, processamento de vetores e pipelines de inferência.
- **Estrutura em Camadas** — Separação clara entre `routers`, `services`, `repositories`, `models` e `utils` para melhor manutenção.
- **Controle de Versão (Git)** — Branching, commits e colaboração em equipe.

<h3>Soft Skills Desenvolvidas</h3>

- **Comunicação Técnica** — Documentação e definição de contratos entre camadas (ex.: payloads JSON, respostas de endpoints).
- **Trabalho Colaborativo** — Coordenação com colegas na divisão de responsabilidades e integração das peças do sistema.
- **Organização e Planejamento** — Priorização de entregas, definição de responsabilidades por módulo e manutenção do repositório.
- **Resolução de Problemas** — Depuração de integrações entre banco, API e módulos de ML.




