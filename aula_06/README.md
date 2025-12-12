# API Iris com JWT - Deploy em Producao

API de classificacao de flores Iris com autenticacao JWT, pronta para deploy no Render.

## Estrutura do Projeto

```
aula_06/
├── app/
│   ├── __init__.py      # Marca como pacote Python
│   ├── main.py          # API FastAPI (endpoints)
│   ├── auth.py          # Modulo de autenticacao JWT
│   └── models/
│       ├── modelo_iris.pkl   # Modelo treinado
│       └── classes_iris.pkl  # Classes do modelo
├── requirements.txt     # Dependencias Python
├── Dockerfile           # Container Docker
├── render.yaml          # Configuracao Render
├── .env.example         # Template de variaveis
├── .gitignore           # Arquivos ignorados
└── .github/
    └── workflows/
        └── deploy.yml   # CI/CD Pipeline
```

## Como Rodar Localmente

### Opcao 1: Com Docker (recomendado)

```bash
# Construir imagem
docker build -t api-iris-jwt .

# Rodar container
docker run -d -p 8000:8000 -e JWT_SECRET=minha-chave-secreta --name iris-api api-iris-jwt

# Testar
curl http://localhost:8000/
```

### Opcao 2: Sem Docker

#### 1. Criar ambiente virtual
```bash
python -m venv venv
venv\Scripts\activate  # Windows
source venv/bin/activate  # Linux/Mac
```

#### 2. Instalar dependencias
```bash
pip install -r requirements.txt
```

#### 3. Configurar variaveis de ambiente
```bash
copy .env.example .env  # Windows
cp .env.example .env    # Linux/Mac
```

#### 4. Rodar a API
```bash
uvicorn app.main:app --reload
```

### 5. Acessar
- API: http://127.0.0.1:8000
- Docs: http://127.0.0.1:8000/docs

## Como Fazer Deploy no Render

### 1. Subir para GitHub
```bash
git init
git add .
git commit -m "API Iris pronta para deploy"
git remote add origin https://github.com/SEU-USUARIO/api-iris-jwt.git
git push -u origin main
```

### 2. Criar Web Service no Render
1. Acesse https://render.com
2. Clique em "New +" > "Web Service"
3. Conecte seu repositorio GitHub
4. Configure:
   - Name: api-iris-jwt
   - Runtime: Python 3
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `uvicorn main:app --host 0.0.0.0 --port $PORT`
5. Adicione as variaveis de ambiente (JWT_SECRET, ENVIRONMENT)
6. Clique em "Create Web Service"

### 3. Testar
```bash
curl https://sua-api.onrender.com/health
```

## Endpoints

| Metodo | Endpoint | Descricao | Auth |
|--------|----------|-----------|------|
| GET | / | Info da API | Nao |
| GET | /health | Health check | Nao |
| POST | /login | Obter token JWT | Nao |
| GET | /me | Info do usuario | Sim |
| POST | /predict | Classificar flor | Sim |

## Exemplo de Uso

### Login
```bash
curl -X POST https://sua-api.onrender.com/login \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "admin123"}'
```

### Predicao
```bash
curl -X POST https://sua-api.onrender.com/predict \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer SEU_TOKEN" \
  -d '{"sepal_length": 5.1, "sepal_width": 3.5, "petal_length": 1.4, "petal_width": 0.2}'
```

## Licenca

MIT
