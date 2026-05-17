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


### 2. Configurar Maven en Jenkins

Instalación Maven con el nombre configurado en el `Jenkinsfile`.

#### Ir a:
```text
Manage Jenkins → Tools → Maven Installations
```

#### Agregar Maven con el nombre exacto:
```text
maven_3_8_1
```
> ⚠️ El nombre debe coincidir exactamente con el definido en el `Jenkinsfile`.


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

> ⚠️ Al ejecutar el pipeline en Jenkins debes tener corriendo Docker Desktop para contruir las imagenes.
---

## ▶️ Modificar Frontend env.

### 🛠️ Debemos modificar el .env segun el entorno

```bash
VITE_KEYCLOAK_REALM=travel-realm
VITE_KEYCLOAK_CLIENT_ID=travel-frontend

# Para localhost
VITE_API_URL=http://localhost:8090
VITE_KEYCLOAK_URL=http://localhost:9090

# Para Servidor
VITE_API_URL=https://api-travel.trebolapp.cl
VITE_KEYCLOAK_URL=https://auth.trebolapp.cl
```
---

## ▶️ Modificar Backend env.

### 🛠️ Debemos modificar el *application.properties* segun el entorno

```bash
# Para localhost
spring.security.oauth2.client.registration.keycloak.redirect-uri={baseUrl}/login/oauth2/code/keycloak
spring.security.oauth2.client.provider.keycloak.issuer-uri=http://${KEYCLOAK_HOST}:9090/realms/travel-realm
spring.security.oauth2.resourceserver.jwt.jwk-set-uri=http://${KEYCLOAK_HOST}:9090/realms/travel-realm/protocol/openid-connect/certs

# Para Servidor
spring.security.oauth2.client.provider.keycloak.issuer-uri=https://auth.trebolapp.cl/realms/travel-realm
spring.security.oauth2.resourceserver.jwt.issuer-uri=https://auth.trebolapp.cl/realms/travel-realm
```

---

## 🐳 Docker Compose (DevOps)

### ▶️ Levantar contenedores

Debes tener correindo Docker Desktop

```bash
docker compose up -d
```


### 🛑 Detener y eliminar contenedores

```bash
docker compose down
```


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

---

## 📦 Nginx

### 1. Configurar el bloque de servidor en Nginx
#### Primero, crea un archivo de configuración para tu sitio.
Crea el archivo:
```bash
sudo nano /etc/nginx/sites-available/travel
```

#### Pega el contenido de nginx.conf

Habilita el sitio y reinicia Nginx:
```bash
sudo ln -s /etc/nginx/sites-available/travel /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```


### 2. Instalar y configurar Certbot (SSL)
#### Para tener el candadito verde (HTTPS), usaremos Let's Encrypt.
Instala Certbot y el plugin de Nginx:
```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
```

Obtén el certificado: Este comando leerá tu configuración de Nginx y te preguntará si quieres redirigir todo el tráfico a HTTPS (recomiendo que digas que sí opción 2).
```bash
sudo certbot --nginx -d travel.trebolapp.cl -d api-travel.trebolapp.cl -d auth.trebolapp.cl
```
Verifica la renovación automática: Certbot instala un "timer" que renueva los certificados antes de que venzan. Puedes probar que funciona con:
```bash
sudo certbot renew --dry-run
```