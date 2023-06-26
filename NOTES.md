

## Declarando tipos¶

Acabas de ver el lugar principal para declarar los type hints. Como parámetros de las funciones.
Este es también el lugar principal en que los usarías con **FastAPI**.

### Tipos simples

Puedes declarar todos los tipos estándar de Python, no solamente str.

Por ejemplo, puedes usar:

- int
- float
- bool
- bytes

```py
def get_items(item_a: str, item_b: int, item_c: float, item_d: bool, item_e: bytes):
    return item_a, item_b, item_c, item_d, item_d, item_e
```

### Tipos con sub-tipos

Existen algunas estructuras de datos que pueden contener otros valores, como *dict*, *list*, *set* y *tuple*. Los valores internos pueden tener su propio tipo también.
Para declarar esos tipos y sub-tipos puedes usar el módulo estándar de Python *typing*.
Él existe específicamente para dar soporte a este tipo de type hints.

#### Listas

Por ejemplo, vamos a definir una variable para que sea una *list* compuesta de *str*.
De *typing*, importa *List* (con una L mayúscula):

```py
from typing import List

def process_items(items: List[str]):
    for item in items:
        print(item)

```

Declara la variable con la misma sintáxis de los dos puntos (:).
Pon *List* como el tipo.
Como la lista es un tipo que permite tener un "sub-tipo" pones el sub-tipo en corchetes *[]*:
```py
from typing import List

def process_items(items: List[str]):
    for item in items:
        print(item)
```

Esto significa: la variable *items* es una *list* y cada uno de los ítems en esta lista es un *str*.
Con esta declaración tu editor puede proveerte soporte inclusive mientras está procesando ítems de la lista.
Sin tipos el autocompletado en este tipo de estructura es casi imposible de lograr.

Observa que la variable *item* es unos de los elementos en la lista *items*.
El editor aún sabe que es un *str* y provee soporte para ello.

#### Tuples y Sets

Harías lo mismo para declarar *tuples* y *sets*:

```py
from typing import Set, Tuple

def process_items(items_t: Tuple[int, int, str], items_s: Set[bytes]):
    return items_t, items_s
```

Esto significa:

- La variable *items_t* es un *tuple* con 3 ítems, un *int*, otro *int*, y un *str*.
- La variable *items_s* es un *set* y cada uno de sus ítems es de tipo *bytes*.

#### Diccionarios (Dicts)

Para definir un *dict* le pasas 2 sub-tipos separados por comas.
El primer sub-tipo es para los *keys* del *dict*.
El segundo sub-tipo es para los *valores* del *dict*:

```py
from typing import Dict

def process_items(prices: Dict[str, float]):
    for item_name, item_price in prices.items():
        print(item_name)
        print(item_price)

```
Esto significa:

- La variable *prices* es un *dict*:
    - Los keys de este *dict* son de tipo *str* (Digamos que son el nombre de cada ítem).
    - Los valores de este *dict* son de tipo *float* (Digamos que son el precio de cada ítem).

#### Clases como tipos

También puedes declarar una clase como el tipo de una variable.
Digamos que tienes una clase *Person* con un nombre:
```py
from typing import Optional

def say_hi(name: Optional[str] = None):
    if name is not None:
        print(f"Hey {name}!")
    else:
        print("Hello World")

```

Entonces puedes declarar una variable que sea de tipo *Person*:
```py
from typing import Optional

def say_hi(name: Optional[str] = None):
    if name is not None:
        print(f"Hey {name}!")
    else:
        print("Hello World")
```

Una vez más tendrás todo el soporte del editor:

## Modelos de Pydantic

Pydantic es una library de Python para llevar a cabo validación de datos.
Tú declaras la "forma" de los datos mediante clases con atributos.
Cada atributo tiene un tipo.

Luego creas un instance de esa clase con algunos valores y Pydantic validará los valores, los convertirá al tipo apropiado (si ese es el caso) y te dará un objeto con todos los datos.
Y obtienes todo el soporte del editor con el objeto resultante.
Tomado de la documentación oficial de Pydantic:
```py
class Person:
    def __init__(self, name: str):
        self.name = name

def get_person_name(one_person: Person):
    return one_person.name
```

Información

Para aprender más sobre [Pydantic mira su documentación](https://pydantic-docs.helpmanual.io/).

**FastAPI** está todo basado en Pydantic.

Vas a ver mucho más de esto en práctica en el [Tutorial - User Guide](https://fastapi.tiangolo.com/es/tutorial/).

## Type hints en FastAPI

**FastAPI** aprovecha estos type hints para hacer varias cosas.
Con **FastAPI** declaras los parámetros con type hints y obtienes:

- **Soporte en el editor**.
- **Type checks**.

...y **FastAPI** usa las mismas declaraciones para:

- **Definir requerimientos**: desde request path parameters, query parameters, headers, bodies, dependencies, etc.
- **Convertir datos**: desde el request al tipo requerido.
- **Validar datos**: viniendo de cada request:
        Generando errores automáticos devueltos al cliente cuando los datos son inválidos.
- **Documentar la API usando OpenAPI**:
        que en su caso es usada por las interfaces de usuario de la documentación automática e interactiva.

Puede que todo esto suene abstracto. Pero no te preocupes que todo lo verás en acción en el [Tutorial - User Guide](https://fastapi.tiangolo.com/es/tutorial/).

Lo importante es que usando los tipos de Python estándar en un único lugar (en vez de añadir más clases, decorator, etc.) **FastAPI** hará mucho del trabajo por ti.