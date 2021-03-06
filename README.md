# keras GridSearch Cacheable

## Descripción
Cuando está desarrollando un componente en **KERAS** y **Tensorflow** y requiere hacer búsqueda de parámetros mediante GridSearch puede ser muy tardado y requerir mucho reproceso al ejecutar su código.

Para dar solución a esto se creó el **keras GridSearch Cacheable** con el objetivo de extender las funcionalidades de cacheo de **SK-Learn** a **KERAS**. 

## Instalación

##### En Google Colaboratory:

Ejecute el siguiente fragmento de código 
```
def downloadDriveFile(file_id,file_name,file_extension):
  !wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id='$file_id -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id="$file_id -O "$file_name"."$file_extension" && rm -rf /tmp/cookies.txt

downloadDriveFile("1G9uWCkxwyE-qISaYbXYtnM21wT1kz11X","CacheableKeras","py")
from cacheable_keras.cacheable import KerasCacheable
```

##### En General
```
TODO: Pendiente por agregar a pip
```

## Uso

#### Extienda la clase en su componente:

```
from CacheableKeras import KerasCacheable

class YourComponent(BaseEstimator, TransformerMixin, KerasCacheable):
    def __init__(self, ...):
```
*Nota: Este ejemplo es para un **TransformerMixin** perfectamente puede usarse en **RegressorMixin** o **ClassifierMixin** segun sea el objetivo de su componente.*

#### Defina Funciones Personalizadas:
Puede decir las funciones que quiere que su modelo ejecute sobrescribiendo el método **get_custom_objects** así:

```
def get_custom_objects(self):
    return { 'custom_loss': self.custom_loss }
```
*Nota: Este método debe retornar un diccionario **{ ‘function_name’ : self.reference_function }** 
y la reference_function debe estar definida en **YourComponent**. 
En caso de necesitar más de una función personalizada agréguela al diccionario.*

#### Personalice la configuración:
**keras GridSearch Cacheable** trabaja por defecto con **TODOS** los parámetros de entrada **YourComponent**. 
Sin embargo, si por algún motivo no desea cachear todos los parámetros de entrada sobrescriba el método **get_params_cacheable** así: 

```
def get_params_cacheable(self):
    return ['Parameter 1','Parameter 2',... 'Parameter N'] 
```
*Nota: Tenga en cuidado al realizar esta modificación, si no coloca **TODOS** los parámetros obligatorios para **YourComponent** al cargar el modelo creado desde su manejador de cache se retornará un error.*


###### Producto desarrollado bajo el curso de posgrado de aprendizaje de máquina avanzado impartido por el profesor Andrés Marino Álvarez – Universidad Nacional  
###### Daniel Espinosa - 2020
