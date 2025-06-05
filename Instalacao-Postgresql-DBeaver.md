
# 🚀 Instalação do PostgreSQL 17.5 no Ubuntu (WSL2) + DBeaver

Este guia descreve os passos para instalar o **PostgreSQL 17.5** em uma distribuição **Ubuntu no WSL2**, bem como instalar e configurar o **DBeaver** para conectar ao banco de dados.

---

## 📦 Instalação do PostgreSQL 17.5 no Ubuntu via WSL2

### 1. Atualize os repositórios
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Adicione o repositório oficial do PostgreSQL
```bash
sudo apt install wget ca-certificates -y
wget -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
```

### 3. Atualize os pacotes e instale o PostgreSQL 17.5
```bash
sudo apt update
sudo apt install postgresql-17 -y
```

### 4. Verifique se o serviço está rodando
```bash
sudo service postgresql status
```

---

## ⚙️ Criar usuário e banco no PostgreSQL

### 1. Acesse o terminal interativo do PostgreSQL
```bash
sudo -u postgres psql
```

### 2. Comandos dentro do PostgreSQL
```sql
-- Criar um usuário com senha
CREATE USER seu_usuario WITH PASSWORD 'sua_senha';

-- Criar um banco de dados e atribuir ao usuário
CREATE DATABASE seu_banco OWNER seu_usuario;

-- Conceder permissões
GRANT ALL PRIVILEGES ON DATABASE seu_banco TO seu_usuario;
```

Para sair do PostgreSQL, digite:
```sql
\q
```

---

## 🛠️ Alterar configurações para acesso externo (opcional)

### Editar `postgresql.conf`
```bash
sudo nano /etc/postgresql/17/main/postgresql.conf
```
Altere a linha:
```
# listen_addresses = 'localhost'
```
Para:
```
listen_addresses = '*'
```

### Editar `pg_hba.conf`
```bash
sudo nano /etc/postgresql/17/main/pg_hba.conf
```

Adicione a linha no final:
```
host    all             all             0.0.0.0/0               md5
```

### Reinicie o serviço
```bash
sudo service postgresql restart
```

---

## 🐿️ Instalação do DBeaver

### 1. Baixe o instalador (.deb) do site oficial
```bash
wget https://dbeaver.io/files/dbeaver-ce_latest_amd64.deb
```

### 2. Instale o DBeaver
```bash
sudo apt install ./dbeaver-ce_latest_amd64.deb -y
```

---

## 🔌 Configuração do DBeaver para PostgreSQL

1. Abra o DBeaver.
2. Clique em **Database > New Database Connection**.
3. Selecione **PostgreSQL**.
4. Preencha os campos:

   - **Host**: `localhost`
   - **Port**: `5432`
   - **Database**: `seu_banco`
   - **Username**: `seu_usuario`
   - **Password**: `sua_senha`

5. Clique em **Test Connection** e depois em **Finish**.

---

## ✅ Pronto!

Seu ambiente com PostgreSQL 17.5 rodando no Ubuntu via WSL2 está configurado e o DBeaver está pronto para uso!
