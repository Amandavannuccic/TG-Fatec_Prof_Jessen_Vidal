<h3>2025-02 </h3>
<h3>LuminIA</h3>

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/320e2699-9820-4aec-8079-4085d800336a" />

# Desafio Proposto pelo Cliente

Neste projeto o cliente foi a [Pro4Tech](https://www.pro4tech.com.br/#como-fazemos), uma empresa especializada em transformação digital, que nos desafiou a desenvolver um backend robusto para gerenciar tickets, usuários e fornecer análises automáticas (FAQ/embeddings, sentimentos e predições por período) para apoiar o processo de tomada de decisão. O objetivo incluiu a criação de rotas de API, persistência em banco de dados, integração com componentes de Machine Learning para inferência de FAQ e geração de métricas/relatórios.

# Ferramenta Desenvolvida

Para atender a essa demanda, o [grupo New Generation](https://github.com/new-ge) desenvolveu a ferramenta [LuminIA](https://github.com/new-ge/LuminIA) uma API backend robusta, responsável por gerenciar usuários e tickets, além de consolidar métricas essenciais para análise. A solução oferece endpoints completos de criação, consulta, atualização e exclusão, processamento de métricas por períodos customizados e integração com modelos de Machine Learning para inferência de FAQ e análise de tendências.

Com arquitetura em camadas incluindo routers, services, repositories, models, ml e utils e persistência estruturada na camada de banco de dados, a API garante organização, escalabilidade e facilidade de manutenção, apoiando tomadas de decisão de forma mais eficiente e inteligente.


 <img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/218dbbd0-f196-4568-8016-a094545e1f14" />


# Tecnologias Utilizadas

| **Categoria**                       | **Ferramenta/Plataforma**           | **Descrição**                                                                 |
|-------------------------------------|--------------------------------------|-------------------------------------------------------------------------------|
| **Linguagem de Programação**        | Python                               | Linguagem principal do backend e scripts de ML.                              |
| **Framework Web/API**               | FastAPI (estrutura de routers)       | Organização dos endpoints em `routers/` e execução via `main.py`.            |
| **Banco de Dados**                  | MongoDB                              | Armazenamento de tickets, usuários e logs (configurações em `db/`).         |
| **Autenticação**                    | JWT (PyJWT)                          | Implementação de tokens JWT para login, sessão e autorização.               |
| **Machine Learning**                | SentenceTransformers, scikit-learn, NumPy | Geração de embeddings, treinamento e inferência para FAQ e classificação. |
| **Processamento de Dados**          | Pandas, NumPy                        | Manipulação e limpeza de dados, preparação de datasets.                     |
| **Validação de Dados**              | Pydantic                              | Definição e validação de modelos para requisições e respostas.             |
| **Variáveis de Ambiente**           | python-dotenv                        | Gerenciamento seguro de credenciais e configurações.                         |
| **Bibliotecas Auxiliares**          | Pandas, scikit-learn (opcional)      | Modelos auxiliares e manipulação de dados.                                  |
| **Ferramenta de Logs**              | Módulo interno (`utils_logs`)        | Centralização de logs e auditoria via `repositories/repository_create_logs.py`. |
| **IDE (Desenvolvimento)**           | VSCode                               | Ambiente utilizado para desenvolvimento e depuração.                         |
| **Controle de Versão**              | Git                                  | Versionamento e colaboração via branches e PRs.                              |

# Contribuições Pessoais

No projeto LuminIA, atuei como Desenvolvedora Full-Stack, responsável pelo desenvolvimento completo do backend, abrangendo desde a definição da arquitetura em camadas (routers, services, repositories, models), implementação de funcionalidades críticas de negócio, criação de múltiplos endpoints REST para autenticação, gestão de usuários e análises, até a integração com Machine Learning para inferência de FAQ e análise de sentimentos. Também cuidei da persistência de dados em MongoDB, da implementação de segurança com JWT e das validações de acesso. O projeto consiste em uma API robusta de gestão e análise de tickets, com recursos avançados de inteligência artificial, desenvolvido entre setembro e novembro de 2025, totalizando 27 commits com contribuições significativas em múltiplas camadas da aplicação.


### Autenticação e Segurança

<details>
<summary><b>Detalhes</b></summary>

Implementação completa do sistema de autenticação com geração de tokens JWT, validação de credenciais e controle de acesso baseado em roles.

### Endpoint de Login

<details>
<summary>Código – Validação de Login com Pipeline MongoDB</summary>

```python
# api_6sem_back_end/routers/router_login.py
from datetime import datetime
from fastapi import APIRouter
from pydantic import BaseModel
from api_6sem_back_end.db.db_configuration import db_data
from api_6sem_back_end.repositories.repository_login_security import create_jwt_token
from api_6sem_back_end.utils.utils_logs import save_log

router = APIRouter(prefix="/login", tags=["Login"])
collection = db_data["users"]

class LoginRequest(BaseModel):
    username: str
    password: str

@router.post("/validate-login")
def validate_login(login_request: LoginRequest):
    try:
        if not login_request.username or not login_request.password:
            return None

        pipeline = [
            {
                "$match": {
                    "login.username": login_request.username,
                    "login.password": login_request.password
                }
            },
            {
                "$project": {
                    "_id": 0,
                    "agent_id": "$agent_id",
                    "username": "$login.username",
                    "role": "$role",
                    "name": "$name",
                    "firstaccess": "$firstaccess"
                }
            }
        ]

        result = list(collection.aggregate(pipeline))

        if not result:
            return {
                "success": False,
            }

        user = result[0]

        if user.get("firstaccess", False) is True:
            return {
                "success": True,
                "firstaccess": True,
                "user": user
            }

        token = create_jwt_token(user["name"], user["role"])
        save_log(user["agent_id"], "LOGIN", "SUCCESS")

        return {
            "success": True,
            "token": token,
            "user": user
        }

    except Exception as e:
        return {
            "success": False,
            "error": str(e)
        }
```

**Funcionalidades:**
- Validação de credenciais contra MongoDB
- Pipeline de agregação para projeção de campos específicos
- Detecção de primeiro acesso
- Geração de token JWT
- Logging automático de eventos de autenticação

</details>

### Geração de Token JWT

<details>
<summary>Código – Criação e Verificação de Token</summary>

```python
# api_6sem_back_end/repositories/repository_login_security.py
import glob
import os
from dotenv import load_dotenv
from fastapi import Depends, HTTPException
import datetime
import jwt
from fastapi.security import HTTPAuthorizationCredentials, HTTPBearer

dotenv_path = glob.glob(os.path.join(os.path.dirname(__file__), "*.env"))
load_dotenv(dotenv_path[0])

security = HTTPBearer()
SECRET_KEY = os.getenv("KEY_JWT")

def create_jwt_token(name, role):
    """
    Cria um token JWT com informações do usuário.
    Válido por 1 hora.
    """
    payload = {
        "name": name,
        "role": role,
        "iat": int(datetime.datetime.now().timestamp()),
        "exp": int((datetime.datetime.now() + datetime.timedelta(hours=1)).timestamp())
    }
    token = jwt.encode(payload, SECRET_KEY, algorithm="HS256")

    return token

def verify_token(credentials: HTTPAuthorizationCredentials = Depends(security)):
    """
    Verifica a validade do token JWT.
    Lança exceção em caso de token expirado ou inválido.
    """
    try:
        payload = jwt.decode(credentials.credentials, SECRET_KEY, algorithms=["HS256"])
        return payload
    except jwt.ExpiredSignatureError:
        raise HTTPException(status_code=401, detail="Token expirado")
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401, detail="Token inválido")
```

**Características:**
- Criação de token com payload contendo nome, role e timestamps
- Expiração automática em 1 hora
- Verificação com tratamento de exceções específicas
- Dependência do FastAPI para proteção de rotas

</details>

### Validações de Usuário

<details>
<summary>Código – Validação de E-mail e Nome (LMN-100)</summary>

A feature **LMN-100** implementou validações rigorosas de nome e e-mail durante a criação de usuários, garantindo dados consistentes e sem duplicatas.

```python
# Exemplo de validação aplicada no repositório de criação
if not user_data.get("email") or not user_data.get("name"):
    raise HTTPException(status_code=400, detail="Email e nome são obrigatórios")

# Verificação de e-mail duplicado
existing_user = collection.find_one({"email": user_data["email"]})
if existing_user:
    raise HTTPException(status_code=409, detail="E-mail já cadastrado")

# Validação de nome (comprimento mínimo e padrão)
if len(user_data.get("name", "")) < 3:
    raise HTTPException(status_code=400, detail="Nome deve ter pelo menos 3 caracteres")
```

</details>

### Validação de Primeiro Login (LMN-108)

<details>
<summary>Detalhes – Fluxo de Primeiro Acesso</summary>

A feature **LMN-108** implementou um fluxo especial para usuários em primeiro acesso, permitindo alteração obrigatória de senha.

**Fluxo implementado:**
1. Sistema detecta `firstaccess = True` na validação de login
2. Retorna flag especial ao frontend
3. Frontend redireciona para tela de alteração de senha
4. Após alteração, `firstaccess` é setado como `False`
5. Usuário pode fazer login normalmente

</details>

</details>

### Endpoints & Roteamento

<details>
<summary><b>Detalhes</b></summary>

Implementação de múltiplos endpoints organizados em routers especializados, seguindo padrão REST com validações de entrada, tratamento de erros e responses padronizadas.

### Estrutura de Routers

```
api_6sem_back_end/routers/
├── router_login.py                  # Autenticação
├── router_create_users.py           # Criação de usuários
├── router_get_all_users.py          # Listagem de usuários
├── router_update_user.py            # Atualização de usuários
├── router_delete_users.py           # Remoção de usuários
├── router_find_user.py              # Busca por usuário específico
├── router_opened.py                 # Tickets abertos
├── router_sentiment.py              # Análise de sentimentos
├── router_primary_themes.py         # Temas primários
├── router_predict_faq.py            # Inferência de FAQ
├── router_by_period.py              # Análises por período
├── router_get_all_logs.py           # Histórico de logs
└── router_exceeded_sla.py           # SLA excedidos
```

### Exemplo – Endpoint de Análise de Sentimento

<details>
<summary>Código – Router de Sentimentos</summary>

```python
# api_6sem_back_end/routers/router_sentiment.py
from fastapi import APIRouter, Depends, Query, HTTPException
from api_6sem_back_end.services.service_sentiment import ServiceSentiment
from api_6sem_back_end.utils.utils_query_filter import Filtro
from api_6sem_back_end.repositories.repository_login_security import verify_token

router = APIRouter(prefix="/sentiment", tags=["Sentiment Analysis"])

@router.get("/count")
def get_sentiment_count(
    start_date: str = Query(...),
    end_date: str = Query(...),
    include_positive: bool = Query(True),
    credentials = Depends(verify_token)
):
    """
    Retorna contagem de tickets por sentimento (positivo/negativo)
    filtrados por período.
    """
    try:
        filtro = Filtro(start_date=start_date, end_date=end_date)
        result = ServiceSentiment.count_tickets_by_sentiment(
            filtro,
            include_positive,
            credentials["role"]
        )
        return {
            "success": True,
            "data": result
        }
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

**Funcionalidades:**
- Proteção com JWT via `verify_token`
- Validação de parâmetros com Query
- Filtro de período customizável
- Resposta padronizada com sucesso/erro
- Diferenciação de acesso por role

</details>

</details>



### Lógica de Negócio & Serviços

<details>
<summary><b>Detalhes</b></summary>

Implementação de regras de negócio complexas em camada de serviços, incluindo agregações temporais, cálculos de métricas e filtros avançados com controle de acesso.

### Análise de Sentimentos

<details>
<summary>Código – Serviço de Contagem por Sentimento</summary>

```python
# api_6sem_back_end/services/service_sentiment.py
from api_6sem_back_end.db.db_configuration import db_data
from api_6sem_back_end.utils.utils_query_filter import build_query_filter, Filtro

collection = db_data["tickets"]

class ServiceSentiment:
    @staticmethod
    def count_tickets_by_sentiment(filtro: Filtro, include_positive: bool, role: str):
        """
        Conta tickets agrupados por sentimento (positivo/negativo).
        Aplica filtros de acesso baseados em role.
        """
        query_filter = build_query_filter(filtro)
        
        if (role != "Gestor"):
            # Controle de acesso em 3 níveis
            levels_map = {
                "N1": ["N1"],
                "N2": ["N1", "N2"],
                "N3": ["N1", "N2", "N3"]
            }

            allowed_levels = levels_map.get(role.upper(), [])

            base_filter = {
                "access_level": {"$in": allowed_levels}
            }
            
            query_filter = build_query_filter(filtro, base_filter)
        else:
            query_filter = build_query_filter(filtro)

        # Pipeline de agregação MongoDB
        pipeline = [
            {"$match": query_filter},
            {
                "$group": {
                    "_id": "$sentiment",
                    "count": {"$sum": 1}
                }
            },
            {
                "$project": {
                    "_id": 0,
                    "sentimento": "$_id",
                    "count": 1
                }
            }
        ]

        result = list(collection.aggregate(pipeline))

        sentimentos_count = {"negative": 0, "positive": 0}

        for r in result:
            sentimento_upper = r["sentimento"].lower()
            if sentimento_upper in sentimentos_count:
                sentimentos_count[sentimento_upper] = r["count"]

        if not include_positive:
            return {"negative": sentimentos_count["negative"]}
        else:
            return sentimentos_count
```

**Funcionalidades:**
- Agregação MongoDB com `$group` e `$match`
- Controle de acesso por nível (N1, N2, N3)
- Filtro de período customizável
- Normalização de resultados
- Suporte para retorno parcial (apenas negativos)

</details>

### Agregação de Tickets por Mês

<details>
<summary>Código – Serviço de Contagem Mensal</summary>

```python
# api_6sem_back_end/services/service_tickets_by_month.py
from api_6sem_back_end.db.db_configuration import db_data
from api_6sem_back_end.utils.utils_query_filter import build_query_filter, Filtro

collection = db_data["tickets"]

class ServiceTicketsByMonth:
    @staticmethod
    def count_tickets_by_month(filtro: Filtro, role: str):
        """
        Agregação de tickets por mês.
        Respeita controle de acesso baseado em role.
        """
        if (role != "Gestor"):
            levels_map = {
                "N1": ["N1"],
                "N2": ["N1", "N2"],
                "N3": ["N1", "N2", "N3"]
            }

            allowed_levels = levels_map.get(role.upper(), [])

            base_filter = {
                "access_level": {"$in": allowed_levels}
            }

            query_filter = build_query_filter(filtro, base_filter)
            
        else:
            query_filter = build_query_filter(filtro)

        pipeline = [
            {"$match": query_filter},
            {
                "$group": {
                    "_id": {"$month": "$created_at"},
                    "count": {"$sum": 1}
                }
            },
            {
                "$project": {
                    "_id": 0,
                    "month": {
                        "$cond": [
                            {"$lt": ["$_id", 10]},
                            {"$concat": ["0", {"$toString": "$_id"}]},
                            {"$toString": "$_id"}
                        ]
                    },
                    "count": 1
                }
            },
            {"$sort": {"month": 1}}
        ]

        result = list(collection.aggregate(pipeline))
        return {doc["month"]: doc["count"] for doc in result}
```

**Funcionalidades:**
- Extração de mês com operador `$month`
- Formatação de mês com padding zero
- Ordenação cronológica
- Dicionário de mês-contagem para fácil consumo

</details>

</details>

### Integração com Machine Learning

<details>
<summary><b>Detalhes</b></b></summary>

Implementação de pipelines de Machine Learning para inferência semântica de FAQ e processamento de embeddings usando SentenceTransformers.

### Inferência de FAQ com Embeddings Semânticos (LMN-3)

<details>
<summary>Código – Sistema de Busca Semântica de FAQ</summary>

```python
# api_6sem_back_end/ml/ml_faq_inference.py
import os
import re
import numpy as np
from sentence_transformers import SentenceTransformer
from sklearn.metrics.pairwise import cosine_similarity
import pandas as pd
from api_6sem_back_end.db.db_configuration import db_data
from api_6sem_back_end.ml.ml_train_faq_fixed import train_faq_classifier

MODEL_DIR = os.path.join(os.path.dirname(__file__), "artifacts/faq_sentence_transformer")

# Carrega modelo existente ou treina novo
if not os.path.exists(MODEL_DIR):
    embedder = train_faq_classifier()
else:
    print("Carregando modelo existente de:", MODEL_DIR)
    embedder = SentenceTransformer(MODEL_DIR)

def preprocess_text(text: str) -> str:
    """
    Pré-processamento de texto: lowercase, remove pontuação,
    normaliza espaços.
    """
    text = str(text).lower()
    text = re.sub(r"[^\w\s]", " ", text)
    text = re.sub(r"\s+", " ", text).strip()
    return text

# Carregamento de FAQ da base de dados
faq_data = list(db_data["faq"].find({}, {"Question": 1, "Answer": 1, "_id": 0}))
df_faq = pd.DataFrame(faq_data)
df_faq["Question_clean"] = df_faq["Question"].apply(preprocess_text)

# Cache de embeddings para performance
emb_path = os.path.join(os.path.dirname(__file__), "artifacts/faq_question_embeddings.npy")

if os.path.exists(emb_path):
    faq_embeddings = np.load(emb_path)
    print("Embeddings das perguntas carregados do cache.")
else:
    print("Gerando embeddings para todas as perguntas da base...")
    faq_embeddings = embedder.encode(
        df_faq["Question_clean"].tolist(),
        convert_to_numpy=True,
        show_progress_bar=True
    )
    np.save(emb_path, faq_embeddings)
    print("Embeddings gerados e salvos.")

def search_similar_questions(user_question: str, top_k=5):
    """
    Busca as K perguntas mais similares à pergunta do usuário
    usando similaridade de cosseno entre embeddings.
    """
    q_proc = preprocess_text(user_question)
    q_emb = embedder.encode([q_proc], convert_to_numpy=True)
    sims = cosine_similarity(q_emb, faq_embeddings)[0]

    top_indices = np.argsort(sims)[::-1][:top_k]
    results = []
    for idx in top_indices:
        results.append({
            "similarity": float(sims[idx]),
            "question": df_faq.iloc[idx]["Question"],
            "answer": df_faq.iloc[idx]["Answer"]
        })
    return results
```

**Funcionalidades:**
- Carregamento automático de modelo pré-treinado
- Pré-processamento de texto normalizado
- Geração e cache de embeddings
- Busca semântica com similiaridade de cosseno
- Top-K resultados ordenados por relevância

</details>

### Treinamento de Modelo FAQ

<details>
<summary>Código – Pipeline de Treinamento</summary>

```python
# Snippet do ml_train_faq_fixed.py
def train_faq_classifier():
    """
    Treina modelo SentenceTransformer com arquivos FAQ.
    Salva artefatos em artifacts/
    """
    from sentence_transformers import SentenceTransformer, InputExample, losses
    from torch.utils.data import DataLoader
    
    # Carrega pares de perguntas similares
    train_examples = load_faq_pairs()
    train_dataloader = DataLoader(train_examples, shuffle=True, batch_size=16)
    
    # Modelo base multilíngue
    model = SentenceTransformer('distiluse-base-multilingual-cased-v2')
    
    # Treina com CosineSimilarityLoss
    train_loss = losses.CosineSimilarityLoss(model)
    model.fit(
        train_objectives=[(train_dataloader, train_loss)],
        epochs=4,
        warmup_steps=100
    )
    
    # Salva modelo para inferência posterior
    model.save(MODEL_DIR)
    return model
```

</details>

</details>

### Modelagem de Dados

<details>
<summary><b>Detalhes</b></summary>

Definição de modelos Pydantic para padronizar estrutura de dados, validação de entrada e documentação automática da API.

### Modelo de Usuário

<details>
<summary>Código – Estrutura de Usuário com Validação</summary>

```python
# api_6sem_back_end/models/model_user.py
from pydantic import BaseModel, EmailStr
from typing import Optional

class LoginModel(BaseModel):
    username: Optional[str] = None
    password: str

class UserCreate(BaseModel):
    email: EmailStr              # Validação automática de e-mail
    name: str
    role: str                     # Gestor, N1, N2, N3
    isActive: bool
    login: LoginModel

class UserResponse(BaseModel):
    agent_id: str
    name: str
    email: str
    role: str
    isActive: bool
    firstaccess: bool
```

**Funcionalidades:**
- Validação automática de e-mail com `EmailStr`
- Modelos separados para entrada/saída
- Type hints completos
- Documentação automática via FastAPI

</details>

### Modelo de Ticket

<details>
<summary>Código – Estrutura de Ticket</summary>

```python
# api_6sem_back_end/models/model_ticket.py
from pydantic import BaseModel
from datetime import datetime
from typing import Optional

class TicketCreate(BaseModel):
    title: str
    description: str
    status: str                   # open, in_progress, closed
    priority: str                # low, medium, high, critical
    access_level: str            # N1, N2, N3
    sentiment: Optional[str] = None
    theme: Optional[str] = None

class TicketUpdate(BaseModel):
    status: Optional[str] = None
    priority: Optional[str] = None
    sentiment: Optional[str] = None

class TicketResponse(BaseModel):
    ticket_id: str
    title: str
    description: str
    status: str
    priority: str
    created_at: datetime
    updated_at: datetime
    sentiment: Optional[str]
    theme: Optional[str]
```

</details>

</details>

### Persistência e Acesso a Dados

<details>
<summary><b>Detalhes</b></summary>

Implementação da camada de acesso a dados com configuração MongoDB, operações CRUD e pipelines de agregação otimizados.

### Configuração de Banco de Dados

<details>
<summary>Código – Conexão MongoDB</summary>

```python
# api_6sem_back_end/db/db_configuration.py
import glob
import os
from pymongo import MongoClient
from dotenv import load_dotenv

# Carrega variáveis de ambiente
dotenv_path = glob.glob(os.path.join(os.path.dirname(__file__), "*.env"))
load_dotenv(dotenv_path[0] if dotenv_path else None)

# Credenciais de conexão
MONGO_URL = os.getenv("MONGO_URL")
DATABASE_NAME = os.getenv("DATABASE_NAME")

# Conexão com retry automático
client = MongoClient(MONGO_URL)
db_data = client[DATABASE_NAME]

# Verifica conexão
try:
    client.admin.command('ping')
    print(f"✓ Conectado ao MongoDB: {DATABASE_NAME}")
except Exception as e:
    print(f"✗ Erro de conexão: {e}")
```

</details>

### Operações de Manipulação de Dados

<details>
<summary>Código – CRUD com MongoDB</summary>

```python
# api_6sem_back_end/db/db_mongo_manipulate_data.py
from api_6sem_back_end.db.db_configuration import db_data
from bson.objectid import ObjectId

class MongoDataManipulation:
    @staticmethod
    def create_user(user_data: dict):
        """Insere novo usuário."""
        collection = db_data["users"]
        result = collection.insert_one(user_data)
        return str(result.inserted_id)

    @staticmethod
    def find_user_by_email(email: str):
        """Busca usuário por e-mail."""
        collection = db_data["users"]
        return collection.find_one({"email": email})

    @staticmethod
    def update_user(user_id: str, update_data: dict):
        """Atualiza documento de usuário."""
        collection = db_data["users"]
        result = collection.update_one(
            {"_id": ObjectId(user_id)},
            {"$set": update_data}
        )
        return result.modified_count > 0

    @staticmethod
    def delete_user(user_id: str):
        """Remove usuário (soft delete com flag)."""
        collection = db_data["users"]
        result = collection.update_one(
            {"_id": ObjectId(user_id)},
            {"$set": {"isActive": False}}
        )
        return result.modified_count > 0
```

</details>

</details>

### Repositories & Padrão de Segurança

<details>
<summary><b>Detalhes</b></summary>

Implementação de repositórios especializados para operações sensíveis, isolando lógica de persistência e facilitando auditoria.

### Repositório de Segurança de Login

<details>
<summary>Código – Operações de Autenticação Auditadas</summary>

```python
# api_6sem_back_end/repositories/repository_login_security.py
import datetime
import jwt
from fastapi import Depends, HTTPException
from fastapi.security import HTTPAuthorizationCredentials, HTTPBearer

security = HTTPBearer()
SECRET_KEY = os.getenv("KEY_JWT")

class RepositoryLoginSecurity:
    
    @staticmethod
    def create_jwt_token(name: str, role: str) -> str:
        """
        Cria token JWT com expiração de 1 hora.
        Inclui timestamp de emissão para auditoria.
        """
        payload = {
            "name": name,
            "role": role,
            "iat": int(datetime.datetime.now().timestamp()),
            "exp": int((datetime.datetime.now() + datetime.timedelta(hours=1)).timestamp())
        }
        return jwt.encode(payload, SECRET_KEY, algorithm="HS256")

    @staticmethod
    def verify_token(credentials: HTTPAuthorizationCredentials = Depends(security)) -> dict:
        """
        Valida token JWT.
        Levanta exceção detalhada em caso de falha.
        """
        try:
            payload = jwt.decode(credentials.credentials, SECRET_KEY, algorithms=["HS256"])
            return payload
        except jwt.ExpiredSignatureError:
            raise HTTPException(status_code=401, detail="Token expirado")
        except jwt.InvalidTokenError:
            raise HTTPException(status_code=401, detail="Token inválido")

    @staticmethod
    def hash_password(password: str) -> str:
        """Futura implementação com bcrypt para maior segurança."""
        pass
```

</details>

</details>

### Logging e Auditoria

<details>
<summary><b>Detalhes</b></summary>

Sistema centralizado de logging para auditoria de operações críticas e rastreamento de atividades de usuários.

### Utilities de Logging

<details>
<summary>Código – Sistema de Logs Centralizado</summary>

```python
# api_6sem_back_end/utils/utils_logs.py
from datetime import datetime
from api_6sem_back_end.db.db_configuration import db_data

def save_log(agent_id: str, action: str, status: str, details: str = ""):
    """
    Registra evento de auditoria no banco.
    
    Args:
        agent_id: ID do agente/usuário
        action: Tipo de ação (LOGIN, CREATE_USER, UPDATE_TICKET, etc)
        status: SUCCESS, FAILURE, ERROR
        details: Informações adicionais
    """
    collection = db_data["logs"]
    
    log_entry = {
        "agent_id": agent_id,
        "action": action,
        "status": status,
        "details": details,
        "timestamp": datetime.utcnow(),
        "ip_address": None  # Pode ser preenchido do request context
    }
    
    result = collection.insert_one(log_entry)
    return str(result.inserted_id)
```

**Funcionalidades:**
- Registro automático de operações críticas
- Timestamp centralizado
- Rastreamento por agente
- Status estruturado

</details>

</details>

### Filtros e Queries Avançadas

<details>
<summary><b>Detalhes</b></summary>

Implementação de utilitários para construção dinâmica de queries com suporte a múltiplos filtros.

### Query Builder Inteligente

<details>
<summary>Código – Construtor de Filtros Dinâmico</summary>

```python
# api_6sem_back_end/utils/utils_query_filter.py
from datetime import datetime
from typing import Dict, Any, Optional
from pydantic import BaseModel

class Filtro(BaseModel):
    start_date: Optional[str] = None
    end_date: Optional[str] = None
    status: Optional[str] = None
    priority: Optional[str] = None
    theme: Optional[str] = None

def build_query_filter(filtro: Filtro, base_filter: Optional[Dict] = None) -> Dict[str, Any]:
    """
    Constrói query MongoDB dinamicamente a partir de filtros.
    Suporta ranges de data, campos específicos e filtros base.
    """
    query = base_filter or {}

    if filtro.start_date and filtro.end_date:
        try:
            start = datetime.fromisoformat(filtro.start_date)
            end = datetime.fromisoformat(filtro.end_date)
            query["created_at"] = {
                "$gte": start,
                "$lte": end
            }
        except ValueError:
            pass

    if filtro.status:
        query["status"] = filtro.status

    if filtro.priority:
        query["priority"] = filtro.priority

    if filtro.theme:
        query["theme"] = filtro.theme

    return query
```

**Funcionalidades:**
- Construção dinâmica de queries
- Filtro de data range automático
- Suporte a múltiplos campos
- Validação com Pydantic
- Composição com filtros base

</details>
</details>
</details>

# Hard Skills Desenvolvidas - [Tabela de Níveis](https://github.com/Amandavannuccic/TG-Fatec/blob/main/TabelaDeN%C3%ADveis/TabelaNiveis.md)

### Desenvolvimento Backend — Intermediário
<details><summary>Detalhes</summary>

- Estruturei uma API completa utilizando FastAPI, arquitetura em camadas e boas práticas de organização;
- Implementei autenticação JWT, controle de acesso, validações e logs estruturados;
- Organizei rotas, serviços e repositórios garantindo manutenibilidade e escalabilidade.

#### **Motivo do nível:**  
Atuei com autonomia e domínio técnico nas principais partes do backend, entregando funcionalidades críticas com qualidade e consistência.

</details>

### Banco de Dados NoSQL (MongoDB) — Intermediário
<details><summary>Detalhes</summary>

- Modelei collections, queries e pipelines de agregação avançados;
- Realizei persistência, atualização e consultas otimizadas para relatórios;
- Trabalhei com filtros dinâmicos, manipulação de datas e projeções.

#### **Motivo do nível:**  
Dominei o uso de agregações e CRUDs completos, mas ainda não explorei temas avançados como indexação e tunning de performance profunda.

</details>

### APIs REST & Arquitetura em Camadas — Intermediário
<details><summary>Detalhes</summary>

- Construí múltiplos endpoints robustos com validação, responses padronizadas e segurança;
- Segui boas práticas de separação de responsabilidades: routers, services, repositories, models e utils;
- Documentei e organizei a API favorecendo entendimento e manutenção.

#### **Motivo do nível:**  
Fui responsável pela criação e organização de toda a estrutura da API, demonstrando domínio do padrão.

</details>

### Machine Learning Aplicado — Intermediário
<details><summary>Detalhes</summary>

- Integrei embeddings semânticos (SentenceTransformers) para inferência de FAQ;
- Implementei Similaridade de Cosseno, pré-processamento de texto e caching de embeddings;
- Realizei treinamento com modelos base e pipelines de inferência.

#### **Motivo do nível:**  
Apliquei técnicas relevantes de ML com sucesso, mas sem avançar para tópicos mais profundos como fine-tuning avançado, avaliação estatística ou deploy de modelos.

</details>

# Soft Skills Desenvolvidas - [Tabela de Níveis](https://github.com/Amandavannuccic/TG-Fatec/blob/main/TabelaDeN%C3%ADveis/TabelaNiveis.md)

### Comunicação Técnica — Avançado
<details> <summary>Detalhes</summary>

- Documentei rotas, modelos e camadas da API;
- Facilitei alinhamentos técnicos sobre arquitetura, requisitos e integrações;
- Expliquei decisões e fluxos complexos de forma clara para a equipe.

#### Motivo do nível:
Comuniquei conceitos técnicos complexos com clareza e segurança, sendo referência na troca de informações.

</details>

### Resolução de Problemas — Intermediário
<details> <summary>Detalhes</summary>

- Corrigi inconsistências entre as camadas (banco, API e ML);
- Depurei erros em pipelines, filtros e validações;
- Ajustei soluções rapidamente quando surgiam desafios durante o desenvolvimento.

#### Motivo do nível: 
Resolvi problemas relevantes com boa autonomia, ainda dependendo de apoio em casos muito avançados de arquitetura ou ML.

</details>

### Organização e Planejamento — Avançado
<details> <summary>Detalhes</summary>

- Estruturei tarefas e prioridades do backend;
- Mantive organização dos módulos, garantindo clareza para contribuições futuras;
- Planejei processos de integração, testes e entrega.

#### Motivo do nível:
Atuei de forma antecipada, organizada e consistente, garantindo fluidez no desenvolvimento do backend.

</details>

### Pensamento Analítico — Intermediário
<details> <summary>Detalhes</summary>

- Analisei cenários para definir a melhor abordagem técnica;
- Conectei dados, lógica e fluxo de operações para gerar relatórios e análises confiáveis;
- Avaliei impactos de decisões na arquitetura.

#### Motivo do nível:
Apresentei boa capacidade de análise, ainda evoluindo para níveis de antecipação estratégica mais profundos.

</details>

### Autonomia e Responsabilidade — Avançado
<details> <summary>Detalhes</summary>

- Fui responsável por uma parte crítica do sistema: autenticação, segurança, análises e ML;
- Trabalhei de forma independente, organizando e entregando módulos completos;
- Mantive consistência e confiabilidade nos commits e entregas.

#### Motivo do nível:
Exerci autonomia com segurança, lidando com funcionalidades essenciais do projeto de forma responsável.

</details>
