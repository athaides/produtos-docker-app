# produtos-docker-app

CRUD de produtos com **Docker Compose**:

| Container | O que roda |
|-----------|-----------|
| `app` | Front-end (HTML/JS em `static/`) + back-end (Spring Boot, API `/api/produtos`) |
| `db`  | PostgreSQL 16, com volume para não perder os dados |

Os dois ficam na mesma rede (`produtos-net`); o app acha o banco pelo nome `db`.

## Rodar no PC

Precisa só do **Docker Desktop** (o Maven roda dentro do Docker).

```bash
docker compose up -d --build
```

Abra http://localhost:8080

Comandos úteis:
```bash
docker compose ps          # ver os containers
docker compose logs -f app # ver logs da aplicação
docker compose down        # parar (dados continuam no volume)
docker compose down -v     # parar e apagar os dados
```

## Subir na nuvem (AWS EC2 ou Google Cloud) via SSH

1. Crie uma VM Ubuntu e libere a porta **8080** (Security Group na AWS / Firewall no Google).
2. Conecte:
   - AWS: `ssh -i minha-chave.pem ubuntu@IP_PUBLICO`
   - Google: botão **SSH** no console, ou `gcloud compute ssh nome-da-vm`
3. No servidor:
   ```bash
   sudo apt update && sudo apt install -y docker.io docker-compose-v2 git
   git clone URL_DO_REPOSITORIO
   cd produtos-docker-app
   cp .env.example .env && nano .env   # defina uma senha
   sudo docker compose up -d --build
   ```
4. Acesse `http://IP_PUBLICO:8080`

## Alternativa: Render

1. Crie um **PostgreSQL** no Render e copie host, banco, usuário e senha.
2. Crie um **Web Service** apontando para o repositório (ele usa o `Dockerfile`).
3. Em *Environment*, defina `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`.
