# Despliegue con Docker 🚀 Proyecto DevOps

## 🐳 Docker-Compose (Frontend)

Para crear la imagen de docker:
**entrarías así**:

http://IP_DEL_SERVIDOR/        frontend
http://IP_DEL_SERVIDOR/api/    backend
http://IP_DEL_SERVIDOR/auth/   keycloak

**Levantas con:**:
```bash
docker compose up -d
```

**Limpias con:**:
```bash
docker compose down
```

```bash
docker compose down
docker compose up -d
docker ps
curl http://localhost:8090
```