# 🚀 Despliegue con Docker — Proyecto DevOps

Guía rápida para configurar el pipeline en Jenkins y desplegar el proyecto utilizando Docker Compose.

---

## ⚙️ Configuración de Jenkins

### 1. Crear el Pipeline

1. Ingresa a Jenkins.
2. Selecciona **New Item**.
3. Elige **Pipeline**.
4. Asigna un nombre al proyecto.
5. Pega el contenido del `Jenkinsfile`.

---

### 2. Configurar Maven en Jenkins

El error ocurre porque Jenkins no encuentra una instalación Maven con el nombre configurado en el `Jenkinsfile`.

#### Ir a:
```text
Manage Jenkins → Tools → Maven Installations
```

#### Agregar Maven con el nombre exacto:
```text
maven_3_8_1
```
> ⚠️ El nombre debe coincidir exactamente con el definido en el `Jenkinsfile`.

---

### 3. Configurar Credenciales Docker Hub

Necesario para publicar imágenes Docker.

#### 📍 Ir a:

```text
Manage Jenkins → Credentials → Add Credentials
```

#### 🏷️ Tipo:

```text
Secret text
```

#### 📝 Completar:

| Campo | Valor |
|---|---|
| Secret | PasswordDeDockerHub |
| ID | dhpswid |


---

## ▶️ Frontend env.

### 🛠️ Debemos modificar el .env segun el entorno

```text
VITE_BACKEND_SERVER=localhost
```

---

## 🐳 Docker Compose (DevOps)

### ▶️ Levantar contenedores

```bash
docker compose up -d
```

---

### 🛑 Detener y eliminar contenedores

```bash
docker compose down
```

---

### 📌 Flujo recomendado

```bash
# Detener entorno anterior
docker compose down

# Levantar entorno actualizado
docker compose up -d

# Verificar contenedores
docker ps

# Validar aplicación
curl http://localhost:8090
```