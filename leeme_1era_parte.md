Perfecto 💪 Aquí tienes un **README.md** listo para subir a GitHub con todo el contenido del documento **UII_Act2_3Tablas_PROMPT_Lucia_Nava.pdf**, ya formateado con encabezados, listas y bloques de código Markdown.

---

````markdown
# 🏋️‍♀️ Proyecto GYM - Django CRUD

## 📘 Información General

**Proyecto:** GYM  
**Lenguaje:** Python  
**Framework:** Django  
**Editor:** Visual Studio Code  

---

## ⚙️ Primera Parte: Configuración Inicial

### 1️⃣ Crear la carpeta del proyecto
```bash
mkdir UIII_GYM_0433
cd UIII_GYM_0433
````

### 2️⃣ Abrir Visual Studio Code en la carpeta del proyecto

```bash
code .
```

### 3️⃣ Abrir la terminal integrada en VS Code

* Menú superior → **Terminal** → **New Terminal**

### 4️⃣ Crear el entorno virtual “.venv”

```bash
python -m venv .venv
```

### 5️⃣ Activar el entorno virtual

```bash
# En Windows PowerShell
.venv\Scripts\Activate
```

### 6️⃣ Activar el intérprete de Python en VS Code

* En VS Code, presionar **Ctrl + Shift + P**
* Escribir: **Python: Select Interpreter**
* Elegir el intérprete que apunte a `.venv`

### 7️⃣ Instalar Django

```bash
pip install django
```

### 8️⃣ Crear el proyecto `backend_Gym` sin duplicar carpetas

```bash
django-admin startproject backend_Gym .
```

### 9️⃣ Ejecutar el servidor en el puerto 8033

```bash
python manage.py runserver 8033
```

### 🔗 10️⃣ Copiar y pegar el link en el navegador

```
http://127.0.0.1:8033/
```

### 11️⃣ Crear la aplicación `app_gym`

```bash
python manage.py startapp app_gym
```

---

## 🧩 Modelos (`models.py`)

```python
from django.db import models

# ==========================================
# MODELO: MIEMBRO
# ==========================================
class Miembro(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    fecha_nacimiento = models.DateField()
    email = models.EmailField(unique=True)
    telefono = models.CharField(max_length=15, blank=True, null=True)
    fecha_inscripcion = models.DateField(auto_now_add=True)
    membresia_activa = models.BooleanField(default=True)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# ==========================================
# MODELO: CLASE
# ==========================================
class Clase(models.Model):
    nombre_clase = models.CharField(max_length=100)
    descripcion = models.TextField(blank=True, null=True)
    horario = models.CharField(max_length=50)  # Ejemplo: "Lunes 10:00-11:00"
    duracion_minutos = models.IntegerField()
    cupo_maximo = models.IntegerField()
    nivel_dificultad = models.CharField(
        max_length=20,
        choices=[
            ('Principiante', 'Principiante'),
            ('Intermedio', 'Intermedio'),
            ('Avanzado', 'Avanzado'),
        ],
        default='Principiante'
    )
    miembros_inscritos = models.ManyToManyField(
        'Miembro',
        related_name='clases_inscritas',
        blank=True
    )

    def __str__(self):
        return self.nombre_clase


# ==========================================
# MODELO: EMPLEADO
# ==========================================
class Empleado(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    fecha_contratacion = models.DateField(auto_now_add=True)
    puesto = models.CharField(max_length=100)  # Ej: "Instructor", "Recepcionista", "Gerente"
    salario = models.DecimalField(max_digits=10, decimal_places=2, blank=True, null=True)
    email = models.EmailField(unique=True)
    telefono = models.CharField(max_length=15, blank=True, null=True)
    clases_impartidas = models.ForeignKey(
        Clase,
        on_delete=models.SET_NULL,
        related_name='instructores',
        blank=True,
        null=True
    )

    def __str__(self):
        return f"{self.nombre} {self.apellido} ({self.puesto})"
```

---

## 🧱 Migraciones

12.5. Crear las migraciones y aplicarlas:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🧮 CRUD del Modelo `Miembro`

### 14️⃣ En `views.py` crear las funciones:

* `inicio_gym`
* `agregar_miembro`
* `actualizar_miembro`
* `realizar_actualizacion_miembro`
* `borrar_miembro`

---

## 🧾 Estructura de Carpetas

```
app_gym/
│
├── migrations/
├── templates/
│   ├── base.html
│   ├── header.html
│   ├── navbar.html
│   ├── footer.html
│   ├── inicio.html
│   └── miembros/
│       ├── agregar_miembros.html
│       ├── ver_miembros.html
│       ├── actualizar_miembros.html
│       └── borrar_miembro.html
├── models.py
├── views.py
├── urls.py
└── admin.py
```

---

## 🎨 Plantillas HTML

### `base.html`

* Incluir **Bootstrap** para CSS y JS.

### `navbar.html`

Debe contener:

* “Sistema de Administración Gym”
* “Inicio”
* Submenú **Miembro** con:

  * Agregar Miembro
  * Ver Miembro
  * Actualizar Miembro
  * Borrar Miembro
* Submenú **Clase** con:

  * Agregar Clase
  * Ver Clase
  * Actualizar Clase
  * Borrar Clase
* Submenú **Empleado** con:

  * Agregar Empleado
  * Ver Empleado
  * Actualizar Empleado
  * Borrar Empleado

> Incluir íconos en las opciones principales (no en los submenús).

### `footer.html`

Debe incluir:

```
© [Año actual] | Creado por Lucia Nava - CBTis 128
```

y mantenerse fijo al final de la página.

### `inicio.html`

Contiene información del sistema y una imagen sobre gimnasio tomada de internet.

---

## 🌐 Configuración de URLs

### `app_gym/urls.py`

Contiene las rutas para las funciones CRUD de `Miembro`.

### `backend_Gym/settings.py`

Agregar la app:

```python
INSTALLED_APPS = [
    ...
    'app_gym',
]
```

### `backend_Gym/urls.py`

Configurar la conexión con `app_gym`.

---

## 🛠️ Administración

* Registrar los modelos en `admin.py`
* Ejecutar nuevamente:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🎨 Recomendaciones de diseño

* Usar **colores suaves, atractivos y modernos**.
* Código HTML **sencillo y limpio**.
* **No validar** la entrada de datos.

---

## 🚀 Ejecución del servidor

```bash
python manage.py runserver 8033
```

Abrir en el navegador:

```
http://127.0.0.1:8033/
```

---

## ✅ Estado del Proyecto

Por ahora solo se trabaja con el modelo **Miembro**.
Los modelos **Clase** y **Empleado** se implementarán posteriormente.

---

**Creado por:**
👩‍💻 *Lucia Nava*
📍 *CBTis 128*

```

---

¿Quieres que le agregue una **sección final con comandos Git** (por ejemplo, cómo subir este proyecto a GitHub paso a paso)?
```
