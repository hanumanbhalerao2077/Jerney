# SETUP.md — Step-by-step (copy/paste ready)

This guide covers:
- Docker Compose local setup
- verification
- teardown

---

## 1) Prerequisites

Install:
- Docker Desktop
- Git

---

## 2) Clone repository

```bash
git clone <YOUR_REPO_URL>
cd <REPO_DIR>
```

Example if you cloned into `jerney`:

```bash
cd jerney
```

---

## 3) Create `.env`

Create a `.env` file at repo root:

```bash
cat > .env <<'EOF'
DB_USER=jerney_user
DB_PASSWORD=jerney_pass_2026
DB_NAME=jerney_db
PORT=5000
EOF
```

---

## 4) Start using Docker Compose

```bash
docker compose up --build
```

Open:
- Frontend: http://localhost:8080
- Backend health: http://localhost:5000/api/health

---

## 5) Verify backend API

```bash
curl -s http://localhost:5000/api/health
```

List posts:

```bash
curl -s http://localhost:5000/api/posts | head
```

---

## 6) Verify database

Run psql inside the DB container:

```bash
docker compose exec -T db psql -U jerney_user -d jerney_db -c "\dt"
```

---

## 7) Stop stack

```bash
docker compose down
```

---

## 8) Teardown (delete DB volume)

Warning: this removes Postgres data.

```bash
docker compose down -v
```

