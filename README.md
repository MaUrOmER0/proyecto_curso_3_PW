## Proyecto Curso 3
Para este proyecto desarrollaremos un proyecto para administrar productos en una tienda. Aquí está el esquema que seguiremos:
### Procedimiento

La actividad se calificará por una serie de puntos asociados a cada actividad propuesta, en total son 50 puntos para una nota de 5, las actividades a realizar son: 

- **Modelos**:
Crearemos un modelo `Producto` con los siguientes atributos:
    - **nombre** (String): Nombre del producto.
    - **descripcion** (Text): Breve descripción del producto.
    - **precio**(Decimal): Precio del producto.
    - **stock** (Integer): Cantidad disponible.
    - **fecha_creacion** (Date): Fecha en que se añadió el producto.
- **Serializadores**: El serializador transformará los datos del modelo Producto en formato JSON para que puedan ser enviados y recibidos por la API.
- **Vistas y URL**: Crearemos un conjunto de vistas (CRUD: Crear, Leer, Actualizar, Eliminar) y configuraremos las rutas para interactuar con los productos desde el cliente o herramientas como Postman.

**1. Crear el Modelo Producto**  
En el archivo models.py de la carpeta api, definimos el modelo:
```
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=100)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    fecha_creacion = models.DateField(auto_now_add=True)

    def __str__(self):
        return self.nombre
```
**2. Realizar Migraciones**  
Ejecutamos los comandos para crear la base de datos con el modelo:
```python manage.py makemigrations
python manage.py migrate
```
**3. Crear el Serializador**  
En el archivo `serial.py`, creamos un serializador para el modelo `Producto`:
```
from rest_framework import serializers
from .models import Producto

class ProductoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Producto
        fields = '__all__'
```
**4. Crear las Vistas**  
En el archivo `views.py`, creamos un ViewSet para el modelo `Producto`:
```
from django.shortcuts import render
from rest_framework import viewsets
from .models import Producto
from .serial import ProductoSerializer

# Create your views here.

class ProductoViewSet(viewsets.ModelViewSet):
    queryset = Producto.objects.all()
    serializer_class = ProductoSerializer
```
**5. Configurar las rutas**  
En el archivo `urls.py`, configuramos las rutas para la API:
```
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from api import views

router = DefaultRouter()
router.register(r'productos', viewset=views.ProductoViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
```
**6. Probamos la API**  
**1. Desde la Interfaz de Django REST Framework:**
 * inicia el servidor con `python manage.py runserver`
 * Ve a `http://localhost:8000/productos/` para probar los métodos GET y POST.

**2.Desde postman**  
Podemos probar los siguientes metodos:
* **GET**:Ayuda a obtener todos los productos o un producto específico `(/productos/<id>/).`
* **POST**: Ayuda a Crear un nuevo producto.  
* **PUT:** Actualiza un producto existente.
* **DELETE:** Elimina un producto.