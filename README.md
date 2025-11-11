# 🧠 EFI Python - API REST con Roles y Permisos (Flask)

Proyecto desarrollado para la **EFI de Programación Python I (DSW 2025)**.  
Implementa una **API REST segura** con autenticación JWT, control de acceso basado en roles (RBAC) y arquitectura modular.

---

## 🚀 Tecnologías utilizadas

- **Flask** (framework principal)
- **Flask-JWT-Extended** → autenticación con tokens JWT
- **Flask-SQLAlchemy** → ORM para la base de datos
- **Flask-Marshmallow** → serialización y validación
- **Flask-Migrate** → migraciones de base de datos
- **Flask-CORS** → soporte de peticiones cross-origin
- **Passlib (bcrypt)** → hash seguro de contraseñas
- **MySQL** como base de datos

---

## 📁 Estructura del proyecto

```
/efipython
├── app.py                  # Configuración principal y rutas
├── models/
│   └── models.py           # Modelos SQLAlchemy
├── schemas/
│   └── schemas.py          # Esquemas Marshmallow
├── views/
│   └── views.py            # Endpoints basados en MethodView
├── decorators/             # (para roles/permisos, opcional)
├── services/               # (lógica de negocio)
├── requirements.txt
└── README.md
```

---

## ⚙️ Configuración e instalación

### 1. Clonar el repositorio
```bash
git clone https://github.com/MateoVrr/efiPython3.git
cd efiPython3
```

### 2. Crear y activar entorno virtual
```bash
python -m venv venv
source venv/bin/activate   # En Linux/Mac
venv\Scripts\activate      # En Windows
```

### 3. Instalar dependencias
```bash
pip install -r requirements.txt
```

### 4. Configurar base de datos

En `app.py` se configura la conexión:
```python
app.config["SQLALCHEMY_DATABASE_URI"] = "mysql+pymysql://root:@localhost/db_efipython"
```
> ⚠️ Modificar usuario, contraseña y nombre de base de datos según tu entorno.

Crear la base de datos:
```sql
CREATE DATABASE db_efipython;
```

### 5. Ejecutar migraciones
```bash
flask db init
flask db migrate -m "create tables"
flask db upgrade
```

### 6. Ejecutar el servidor
```bash
python app.py
```
La API correrá en `http://127.0.0.1:5000/`

---

## 🔑 Autenticación y Roles

El sistema maneja **JWT (JSON Web Tokens)** con los siguientes roles:

| Rol | Permisos principales |
|-----|----------------------|
| `user` | Crear/editar/eliminar sus propios posts y comentarios |
| `moderator` | Puede editar categorías y eliminar cualquier comentario |
| `admin` | Control total: gestionar usuarios, roles, posts y categorías |

---

## 🧩 Endpoints principales

### 🔐 Autenticación
| Método | Ruta | Descripción |
|---------|------|-------------|
| `POST` | `/register` | Registrar nuevo usuario |
| `POST` | `/login` | Iniciar sesión y obtener JWT |

### 👤 Usuarios
| Método | Ruta | Rol | Descripción |
|---------|------|-----|-------------|
| `GET` | `/users` | admin | Listar todos los usuarios |
| `GET` | `/users/<id>` | autenticado | Ver perfil propio o de otro (si es admin) |
| `PUT` | `/users/<id>` | propietario/admin | Editar usuario |
| `DELETE` | `/users/<id>` | propietario/admin | Eliminar usuario |

### 📝 Posts
| Método | Ruta | Rol | Descripción |
|---------|------|-----|-------------|
| `GET` | `/posts` | público | Listar todos los posts |
| `GET` | `/posts/<id>` | público | Ver post específico |
| `POST` | `/posts` | autenticado | Crear post propio |
| `PUT` | `/posts/<id>` | autor/admin | Editar post |
| `DELETE` | `/posts/<id>` | autor/admin | Eliminar post |

### 💬 Comentarios
| Método | Ruta | Rol | Descripción |
|---------|------|-----|-------------|
| `GET` | `/reviews` | público | Listar comentarios |
| `POST` | `/reviews` | autenticado | Crear comentario |
| `PUT` | `/reviews/<id>` | autor/admin | Editar comentario |
| `DELETE` | `/reviews/<id>` | autor/moderador/admin | Eliminar comentario |

### 🗂 Categorías
| Método | Ruta | Rol | Descripción |
|---------|------|-----|-------------|
| `GET` | `/categories` | público | Listar categorías |
| `POST` | `/categories` | moderator/admin | Crear categoría |
| `PUT` | `/categories/<id>` | moderator/admin | Editar categoría |
| `DELETE` | `/categories/<id>` | admin | Eliminar categoría |

---

## 🧠 Ejemplo de uso (Postman o .http)

### 🔸 Registro
```http
POST http://localhost:5000/register
Content-Type: application/json

{
  "nombre": "Mateo",
  "email": "mateo@example.com",
  "password": "1234",
  "role": "admin"
}
```

### 🔸 Login
```http
POST http://localhost:5000/login
Content-Type: application/json

{
  "email": "mateo@example.com",
  "password": "1234"
}
```

**Respuesta:**
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJh..."
}
```

### 🔸 Crear Post
```http
POST http://localhost:5000/posts
Authorization: Bearer <TOKEN>
Content-Type: application/json

{
  "titulo": "Mi primer post",
  "contenido": "Contenido de prueba",
  "categoria_id": 1
}
```

---

## 🧰 Datos de prueba sugeridos

| Usuario | Email | Rol | Contraseña |
|----------|-------|-----|-------------|
| Admin | admin@example.com | admin | 1234 |
| Moderador | mod@example.com | moderator | 1234 |
| Usuario | user@example.com | user | 1234 |

---

## 📊 Próximos pasos / Mejoras posibles
- [ ] Implementar endpoint `/api/stats`
- [ ] Agregar refresh tokens JWT
- [ ] Documentación Swagger/OpenAPI
- [ ] Soft delete para usuarios y posts
- [ ] Paginación en listados

---

## 👨‍💻 Autores
- **Mateo Torres**
- **Facundo Bellandi**
- **Santiago Capellino**

---

## 📜 Licencia
Este proyecto es parte de una evaluación académica (EFI) y su uso está destinado únicamente a fines educativos.
