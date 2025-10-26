from pathlib import Path

# Criar os arquivos para o projeto n8n de Allison
base_dir = Path("/mnt/data/turismo_n8n_project")
base_dir.mkdir(parents=True, exist_ok=True)

# .gitignore
gitignore_content = """.env
__pycache__/
*.pyc
*.log
node_modules/
dist/
"""

# .env.example
env_example_content = """# Exemplo de variáveis de ambiente para o projeto Turismo IAllison (n8n)
WEBHOOK_URL=https://seu-dominio-aqui
N8N_HOST=$(PRIMARY_DOMAIN)
GENERIC_TIMEZONE=America/Sao_Paulo
N8N_COMMUNITY_PACKAGES_ALLOW_TOOL_USAGE=true

DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=localhost
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_USER=postgres
DB_POSTGRESDB_PASSWORD=COLOQUE_SUA_SENHA_AQUI
"""

# docker-compose.yml
docker_compose_content = """version: '3.9'

services:
  n8n:
    image: n8nio/n8n:latest
    restart: always
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=${DB_TYPE}
      - DB_POSTGRESDB_HOST=${DB_POSTGRESDB_HOST}
      - DB_POSTGRESDB_PORT=${DB_POSTGRESDB_PORT}
      - DB_POSTGRESDB_USER=${DB_POSTGRESDB_USER}
      - DB_POSTGRESDB_PASSWORD=${DB_POSTGRESDB_PASSWORD}
      - N8N_HOST=${N8N_HOST}
      - WEBHOOK_URL=${WEBHOOK_URL}
      - GENERIC_TIMEZONE=${GENERIC_TIMEZONE}
      - N8N_COMMUNITY_PACKAGES_ALLOW_TOOL_USAGE=${N8N_COMMUNITY_PACKAGES_ALLOW_TOOL_USAGE}
    volumes:
      - ./n8n_data:/home/node/.n8n
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15
    restart: always
    environment:
      POSTGRES_USER: ${DB_POSTGRESDB_USER}
      POSTGRES_PASSWORD: ${DB_POSTGRESDB_PASSWORD}
    volumes:
      - ./postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:alpine
    restart: always
    ports:
      - "6379:6379"
    volumes:
      - ./redis_data:/data

volumes:
  n8n_data:
  postgres_data:
  redis_data:
"""

# README.md
readme_content = """# 🤖 Projeto Turismo IAllison

Automação de chatbot para turismo em Três Marias, integrando n8n, Redis, PostgreSQL e IA (Google Gemini).

## 🚀 Tecnologias
- n8n (Automação de fluxos)
- Redis (gerenciamento de memória de chat)
- PostgreSQL (armazenamento de dados)
- AI Agent (Google Gemini API)
- Docker e Docker Compose

## ⚙️ Como rodar
1. Crie um arquivo `.env` com base no `.env.example` e preencha suas variáveis.
2. Execute o projeto:
   ```bash
   docker-compose up -d

