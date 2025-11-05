Perfecto — lo convierto en **un README paso a paso**: cada acción numerada y lista para seguir (copiar/pegar). Está completo — configuración, código esencial y comandos. Copia todo y pégalo en tu `README.md`.

---

# 🏋️‍♀️ Proyecto GYM - Django CRUD (Paso a paso)

## Información rápida

* **Lenguaje:** Python
* **Framework:** Django
* **Editor recomendado:** Visual Studio Code

---

## Paso 1 — Crear la carpeta del proyecto

```bash
mkdir UIII_GYM_0433
cd UIII_GYM_0433
```

---

## Paso 2 — Abrir VS Code en la carpeta

```bash
code .
```

---

## Paso 3 — Crear y activar el entorno virtual

1. Crear:

```bash
python -m venv .venv
```

2. Activar (Windows PowerShell):

```bash
.venv\Scripts\Activate
```

(Ajusta el comando si usas bash / macOS / Linux)

---

## Paso 4 — Seleccionar intérprete en VS Code

1. Presiona `Ctrl + Shift + P`
2. Escribe `Python: Select Interpreter` → elige el que apunta a `.venv`

---

## Paso 5 — Instalar Django

```bash
pip install django
```

---

## Paso 6 — Crear el proyecto Django

```bash
django-admin startproject backend_Gym .
```

---

## Paso 7 — Crear la aplicación

```bash
python manage.py startapp app_gym
```

---

## Paso 8 — Estructura recomendada (crear carpetas/archivos)

Crea las carpetas y archivos según esta estructura:

```
UIII_GYM_0433/
├── .venv/
├── backend_Gym/
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── app_gym/
│   ├── migrations/
│   ├── templates/
│   │   ├── base.html
│   │   ├── navbar.html
│   │   ├── footer.html
│   │   ├── inicio.html
│   │   └── miembros/
│   │       ├── agregar_miembros.html
│   │       ├── ver_miembros.html
│   │       ├── actualizar_miembros.html
│   │       └── borrar_miembro.html
│   ├── static/
│   │   └── app_gym/css/styles.css
│   ├── models.py
│   ├── views.py
│   ├── urls.py
│   ├── forms.py
│   └── admin.py
└── manage.py
```

---

## Paso 9 — Copiar `models.py` (archivo: `app_gym/models.py`)

```python
from django.db import models

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

class Clase(models.Model):
    nombre_clase = models.CharField(max_length=100)
    descripcion = models.TextField(blank=True, null=True)
    horario = models.CharField(max_length=50)
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
    miembros_inscritos = models.ManyToManyField('Miembro', related_name='clases_inscritas', blank=True)

    def __str__(self):
        return self.nombre_clase

class Empleado(models.Model):
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    fecha_contratacion = models.DateField(auto_now_add=True)
    puesto = models.CharField(max_length=100)
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

## Paso 10 — Copiar `forms.py` (archivo: `app_gym/forms.py`)

```python
from django import forms
from .models import Miembro

class MiembroForm(forms.ModelForm):
    class Meta:
        model = Miembro
        fields = ['nombre', 'apellido', 'fecha_nacimiento', 'email', 'telefono', 'membresia_activa']
        widgets = {
            'fecha_nacimiento': forms.DateInput(attrs={'type': 'date'}),
        }
```

---

## Paso 11 — Copiar `views.py` (archivo: `app_gym/views.py`)

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Miembro
from .forms import MiembroForm

def inicio_gym(request):
    return render(request, 'inicio.html')

def ver_miembros(request):
    miembros = Miembro.objects.all().order_by('apellido', 'nombre')
    return render(request, 'miembros/ver_miembros.html', {'miembros': miembros})

def agregar_miembro(request):
    if request.method == 'POST':
        form = MiembroForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('ver_miembros')
    else:
        form = MiembroForm()
    return render(request, 'miembros/agregar_miembros.html', {'form': form})

def actualizar_miembro(request, pk):
    miembro = get_object_or_404(Miembro, pk=pk)
    if request.method == 'POST':
        form = MiembroForm(request.POST, instance=miembro)
        if form.is_valid():
            form.save()
            return redirect('ver_miembros')
    else:
        form = MiembroForm(instance=miembro)
    return render(request, 'miembros/actualizar_miembros.html', {'form': form, 'miembro': miembro})

def borrar_miembro(request, pk):
    miembro = get_object_or_404(Miembro, pk=pk)
    if request.method == 'POST':
        miembro.delete()
        return redirect('ver_miembros')
    return render(request, 'miembros/borrar_miembro.html', {'miembro': miembro})
```

---

## Paso 12 — Copiar `app_gym/urls.py` (archivo: `app_gym/urls.py`)

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.inicio_gym, name='inicio'),
    path('miembros/', views.ver_miembros, name='ver_miembros'),
    path('miembros/agregar/', views.agregar_miembro, name='agregar_miembro'),
    path('miembros/actualizar/<int:pk>/', views.actualizar_miembro, name='actualizar_miembro'),
    path('miembros/borrar/<int:pk>/', views.borrar_miembro, name='borrar_miembro'),
]
```

---

## Paso 13 — Ajustes en `backend_Gym/settings.py`

Cambia/añade los fragmentos clave:

```python
from pathlib import Path
BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = 'tu-secret-key-aqui'
DEBUG = True
ALLOWED_HOSTS = []

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_gym',
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [ BASE_DIR / 'app_gym' / 'templates' ],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

STATIC_URL = '/static/'
STATICFILES_DIRS = [ BASE_DIR / 'app_gym' / 'static' ]
STATIC_ROOT = BASE_DIR / 'staticfiles'

LANGUAGE_CODE = 'es-mx'
TIME_ZONE = 'America/Chihuahua'
USE_I18N = True
USE_TZ = True
```

---

## Paso 14 — Copiar `backend_Gym/urls.py` (archivo: `backend_Gym/urls.py`)

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_gym.urls')),
]

if settings.DEBUG:
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

---

## Paso 15 — Plantillas HTML (guardar en `app_gym/templates/`)

### `base.html`

```html
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>{% block title %}Gym Admin{% endblock %}</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <link rel="stylesheet" href="{% static 'app_gym/css/styles.css' %}">
</head>
<body>
  {% include "navbar.html" %}
  <main class="container my-4">
    {% block content %}{% endblock %}
  </main>
  {% include "footer.html" %}
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### `navbar.html`

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light shadow-sm">
  <div class="container">
    <a class="navbar-brand" href="{% url 'inicio' %}">Sistema de Administración Gym</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navMenu">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navMenu">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link" href="{% url 'inicio' %}">Inicio</a></li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">Miembro</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="{% url 'agregar_miembro' %}">Agregar Miembro</a></li>
            <li><a class="dropdown-item" href="{% url 'ver_miembros' %}">Ver Miembros</a></li>
          </ul>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">Clase</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Clase</a></li>
            <li><a class="dropdown-item" href="#">Ver Clase</a></li>
          </ul>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">Empleado</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Agregar Empleado</a></li>
            <li><a class="dropdown-item" href="#">Ver Empleado</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

### `footer.html`

```html
<footer class="footer mt-auto py-3 bg-light">
  <div class="container text-center">
    <span>© {% now "Y" %} | Creado por Lucia Nava - CBTis 128</span>
  </div>
</footer>
```

### `inicio.html`

```html
{% extends "base.html" %}
{% block title %}Inicio - Gym{% endblock %}
{% block content %}
<div class="row">
  <div class="col-md-8">
    <h1>Bienvenid@ al Sistema de Administración Gym</h1>
    <p>Gestiona miembros, clases y empleados de forma sencilla.</p>
  </div>
  <div class="col-md-4">
    <img src="https://images.unsplash.com/photo-1549576490-b0b4831ef60a" alt="gimnasio" class="img-fluid rounded">
  </div>
</div>
{% endblock %}
```

### `miembros/ver_miembros.html`

```html
{% extends "base.html" %}
{% block title %}Miembros{% endblock %}
{% block content %}
<h2>Miembros</h2>
<a class="btn btn-primary mb-3" href="{% url 'agregar_miembro' %}">Agregar Miembro</a>
<table class="table table-striped">
  <thead>
    <tr><th>#</th><th>Nombre</th><th>Email</th><th>Inscripción</th><th>Acciones</th></tr>
  </thead>
  <tbody>
    {% for miembro in miembros %}
    <tr>
      <td>{{ forloop.counter }}</td>
      <td>{{ miembro.nombre }} {{ miembro.apellido }}</td>
      <td>{{ miembro.email }}</td>
      <td>{{ miembro.fecha_inscripcion }}</td>
      <td>
        <a class="btn btn-sm btn-warning" href="{% url 'actualizar_miembro' miembro.pk %}">Editar</a>
        <a class="btn btn-sm btn-danger" href="{% url 'borrar_miembro' miembro.pk %}">Borrar</a>
      </td>
    </tr>
    {% empty %}
    <tr><td colspan="5">No hay miembros aún.</td></tr>
    {% endfor %}
  </tbody>
</table>
{% endblock %}
```

### `miembros/agregar_miembros.html`

```html
{% extends "base.html" %}
{% block title %}Agregar Miembro{% endblock %}
{% block content %}
<h2>Agregar Miembro</h2>
<form method="post">{% csrf_token %}
  {{ form.as_p }}
  <button class="btn btn-success" type="submit">Guardar</button>
  <a class="btn btn-secondary" href="{% url 'ver_miembros' %}">Cancelar</a>
</form>
{% endblock %}
```

### `miembros/actualizar_miembros.html`

```html
{% extends "base.html" %}
{% block title %}Actualizar Miembro{% endblock %}
{% block content %}
<h2>Actualizar Miembro: {{ miembro }}</h2>
<form method="post">{% csrf_token %}
  {{ form.as_p }}
  <button class="btn btn-primary" type="submit">Actualizar</button>
  <a class="btn btn-secondary" href="{% url 'ver_miembros' %}">Cancelar</a>
</form>
{% endblock %}
```

### `miembros/borrar_miembro.html`

```html
{% extends "base.html" %}
{% block title %}Borrar Miembro{% endblock %}
{% block content %}
<h2>¿Eliminar miembro?</h2>
<p>¿Seguro que deseas eliminar a <strong>{{ miembro }}</strong>?</p>
<form method="post">{% csrf_token %}
  <button class="btn btn-danger" type="submit">Sí, eliminar</button>
  <a class="btn btn-secondary" href="{% url 'ver_miembros' %}">Cancelar</a>
</form>
{% endblock %}
```

---

## Paso 16 — CSS básico (archivo: `app_gym/static/app_gym/css/styles.css`)

```css
body {
  font-family: "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  background: #fafafa;
  color: #333;
}
.navbar-brand { font-weight: 700; }
.footer { position: relative; bottom: 0; width: 100%; }
.container img { max-width: 100%; height: auto; }
.table th, .table td { vertical-align: middle; }
```

---

## Paso 17 — Registrar modelos en el admin (archivo: `app_gym/admin.py`)

```python
from django.contrib import admin
from .models import Miembro, Clase, Empleado

@admin.register(Miembro)
class MiembroAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'apellido', 'email', 'membresia_activa', 'fecha_inscripcion')
    search_fields = ('nombre', 'apellido', 'email')

@admin.register(Clase)
class ClaseAdmin(admin.ModelAdmin):
    list_display = ('nombre_clase', 'horario', 'duracion_minutos', 'cupo_maximo')

@admin.register(Empleado)
class EmpleadoAdmin(admin.ModelAdmin):
    list_display = ('nombre', 'apellido', 'puesto', 'email')
```

---

## Paso 18 — Crear migraciones y aplicar

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## Paso 19 — Crear superuser (opcional)

```bash
python manage.py createsuperuser
```

---

## Paso 20 — Ejecutar servidor

```bash
python manage.py runserver 8033
```

Abrir en el navegador:

```
http://127.0.0.1:8033/
```

---

## Paso 21 — Comandos Git para subir el proyecto a GitHub

```bash
git init
git add .
git commit -m "Proyecto GYM - estructura inicial y CRUD Miembro"
git branch -M main
git remote add origin https://github.com/tu_usuario/tu_repo.git
git push -u origin main
```

---

## Paso 22 — Siguientes mejoras sugeridas (elige una)

1. Validaciones personalizadas en `forms.py`.
2. Paginación / búsqueda para la lista de miembros.
3. Implementar CRUD completos para **Clase** y **Empleado** (mismo patrón).
4. Preparar `requirements.txt` y `.gitignore`.

---

Si quieres, ahora te devuelvo **este mismo README** pero ya copiado dentro de un archivo listo para descargar, o lo adapto para que incluya validaciones y CRUD completos para *Clase* y *Empleado*. ¿Cuál prefieres que haga ahora? 💪
