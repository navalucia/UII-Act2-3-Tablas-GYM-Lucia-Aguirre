🏋️‍♀️ Proyecto GYM

Lenguaje: Python
Framework: Django
Editor: Visual Studio Code

📁 1. Crear carpeta del proyecto
mkdir UIII_GYM_0433
cd UIII_GYM_0433

💻 2. Abrir VS Code sobre la carpeta
code .

🧭 3. Abrir terminal en VS Code

En VS Code, ir a:

Ver → Terminal

🌐 4. Crear entorno virtual .venv

Desde la terminal de VS Code:

python -m venv .venv

⚙️ 5. Activar entorno virtual
.venv\Scripts\activate


(Si usas Linux/Mac)

source .venv/bin/activate

🐍 6. Activar intérprete de Python

En VS Code:
Presiona Ctrl + Shift + P → Escribe Python: Select Interpreter → selecciona el entorno .venv.

📦 7. Instalar Django
pip install django

🏗️ 8. Crear proyecto sin duplicar carpeta
django-admin startproject backend_Gym .

🚀 9. Ejecutar servidor en el puerto 8033
python manage.py runserver 8033

🌍 10. Copiar y pegar el link en el navegador
http://127.0.0.1:8033/

🧩 11. Crear aplicación app_gym
python manage.py startapp app_gym

🧠 12. Crear modelo en app_gym/models.py
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
    miembros_inscritos = models.ManyToManyField(Miembro, related_name='clases_inscritas', blank=True)

    def __str__(self):
        return self.nombre_clase

# ==========================================
# MODELO: EMPLEADO
# ==========================================
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

🧱 12.5 Realizar migraciones
python manage.py makemigrations
python manage.py migrate

👥 13. Trabajar con el modelo: Miembro
🧭 14. Crear funciones en views.py

Funciones:

inicio_gym

agregar_miembro

actualizar_miembro

realizar_actualizacion_miembro

borrar_miembro

(No se usa forms.py)

🗂️ 15. Crear carpeta templates dentro de app_gym
app_gym/templates/

🧾 16. Crear archivos HTML dentro de templates

base.html

header.html

navbar.html

footer.html

inicio.html

🎨 17. Agregar Bootstrap a base.html

Incluir links de Bootstrap CSS y JS en la plantilla base.

🧭 18. En navbar.html

Incluir:

Sistema de Administración Gym

Inicio

Menú “Miembro” con submenú:

Agregar miembro

Ver miembro

Actualizar miembro

Borrar miembro

Menú “Clase” con submenú

Menú “Empleado” con submenú
Agregar íconos a las opciones principales (no en los submenús).

🧑‍💻 19. En footer.html

Incluir:

Derechos de autor ©
Fecha del sistema
Creado por Lucia Nava, CBTIS 128


Mantener fijo al final de la página.

🖼️ 20. En inicio.html

Colocar información del sistema + una imagen de un gimnasio tomada de internet.

📁 21. Crear subcarpeta para miembros
app_gym/templates/miembros/

🧩 22. Crear archivos HTML de CRUD de miembros

agregar_miembros.html

ver_miembros.html (mostrar tabla con botones Ver, Editar, Borrar)

actualizar_miembros.html

borrar_miembro.html

⚠️ 23. No utilizar forms.py
🔗 24. Crear urls.py en app_gym

Enlazar las rutas a las funciones de views.py (CRUD de miembros).

🧩 25. Registrar la app en settings.py

En INSTALLED_APPS:

'app_gym',

🌐 26. Configurar urls.py del proyecto principal

En backend_Gym/urls.py:

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_gym.urls')),
]

🗃️ 27. Registrar modelos en admin.py
from django.contrib import admin
from .models import Miembro, Clase, Empleado

admin.site.register(Miembro)
admin.site.register(Clase)
admin.site.register(Empleado)


Luego ejecutar:

python manage.py makemigrations
python manage.py migrate

⚙️ 28. Por ahora, solo trabajar con el modelo Miembro

Dejar pendientes los modelos Clase y Empleado.

🎨 29. Usar colores suaves y modernos

Diseño limpio, atractivo y minimalista.

🚫 30. No validar entrada de datos.
🏗️ 31. Crear estructura completa de carpetas y archivos desde el inicio.
✅ 32. Proyecto totalmente funcional.
🔁 33. Ejecutar servidor en el puerto 8033
python manage.py runserver 8033


Luego abrir:

http://127.0.0.1:8033/


📅 Proyecto realizado por: Lucia Nava – CBTIS 128
🧠 Materia: Programación Web con Python y Django
🎯 Objetivo: Sistema CRUD básico de administración para gimnasio
