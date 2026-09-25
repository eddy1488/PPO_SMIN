# Guia Maestra de POO en C++: Clases, Memoria y Buenas Practicas

Esta guia es un resumen estructurado de los conceptos fundamentales de la Programacion Orientada a Objetos en C++. Sirve como referencia rapida para resolver dudas de sintaxis, diseno de clases y gestion de memoria.

---

## 1. Modularizacion: Archivos `.h` vs `.cpp`

Dividir el codigo separa la **interfaz** (que hace) de la **implementacion** (como lo hace).

* **Archivo `.h` (Header / Cabecera):** Define los atributos privados y declara las firmas de los metodos. Usa guardas de inclusion (`#ifndef`, `#define`, `#endif`).


---

## 1. Archivo de Cabecera: `NombreClase.h`

```cpp
#ifndef NOMBRECLASE_H
#define NOMBRECLASE_H

// Incluye aquí otras librerías u objetos si los necesitas
// #include <iostream>
// #include "OtraClase.h"

class NombreClase {

    private:
        // 1. ATRIBUTOS PRIVADOS
        // Variables internas que solo la clase puede modificar directamente
        int atributo1;
        float atributo2;

    public:
        // 2. CONSTRUCTORES
        // Constructor por defecto (opcional)
        NombreClase();

        // Constructor con parámetros
        NombreClase(int param1, float param2);

        // 3. GETTERS (Lectura - Siempre llevan 'const' al final)
        int getAtributo1() const;
        float getAtributo2() const;

        // 4. SETTERS (Escritura - NO llevan 'const')
        void setAtributo1(int nuevoValor1);
        void setAtributo2(float nuevoValor2);

        // 5. MÉTODOS DE CONSULTA / LECTURA (Llevan 'const' al final)
        void affichage() const;
        bool esIgual(const NombreClase& otroObjeto) const;

        // 6. MÉTODOS QUE MODIFICAN EL OBJETO (NO llevan 'const')
        void modificarValores(int val1, float val2);
};

#endif // NOMBRECLASE_H

* **Archivo `.cpp` (Source / Fuente):** Aqui se escribe la logica real de las funciones usando el operador de resolucion de ambito `::`.

### Cuándo usar y cuándo NO:
* **USAR:** Siempre que crees una clase nueva. Mantendra tu proyecto escalable y limpio.
* **NO USAR:** En scripts rapidos de un solo archivo para pruebas desechables.

---

## 2. Constructores y Listas de Inicializacion

El constructor da vida al objeto. La forma en que asignas los valores iniciales importa mucho.

### A. Asignacion clasica (Dentro de las llaves)
Util para tipos primitivos (`int`, `float`), pero menos eficiente para objetos.
```cpp
Point::Point(float px, float py) {
    X = px; 
    Y = py;
}

```

```cpp
#include <iostream>
#include "NombreClase.h" // ¡OBLIGATORIO! Incluir siempre su propia cabecera

using namespace std;

// 1. CONSTRUCTORES

// Implementación del constructor por defecto (valores base)
NombreClase::NombreClase() : atributo1(0), atributo2(0.0f) {
    // Código adicional de inicialización si es necesario
}

// Implementación del constructor con parámetros usando Lista de Inicialización
NombreClase::NombreClase(int param1, float param2) : atributo1(param1), atributo2(param2) {
    // Si usas la lista de arriba, las llaves suelen quedar vacías
}

// 2. GETTERS

int NombreClase::getAtributo1() const {
    return atributo1;
}

float NombreClase::getAtributo2() const {
    return atributo2;
}

// 3. SETTERS

void NombreClase::setAtributo1(int nuevoValor1) {
    // Aquí puedes agregar validaciones antes de cambiar el valor
    atributo1 = nuevoValor1;
}

void NombreClase::setY(float nuevoValor2) {
    atributo2 = nuevoValor2;
}

// 4. MÉTODOS DE CONSULTA Y LÓGICA

void NombreClase::affichage() const {
    cout << "Atributo 1: " << atributo1 << " | Atributo 2: " << atributo2 << endl;
}

bool NombreClase::esIgual(const NombreClase& otroObjeto) const {
    // Compara el objeto actual (atributo1, atributo2) con el recibido por referencia
    return (atributo1 == otroObjeto.getAtributo1() && atributo2 == otroObjeto.getAtributo2());
}

void NombreClase::modificarValores(int val1, float val2) {
    atributo1 = val1;
    atributo2 = val2;
}
```
### B. Lista de Inicializacion (Con dos puntos `:`)

Le dice al compilador que construya las variables **antes** de entrar a las llaves.

```cpp
Point::Point(float px, float py) : X(px), Y(py) {}

```

### Cuándo usar y cuándo NO:

* **USAR LISTAS DE INICIALIZACION:** Obligatorio cuando tu clase tiene objetos de otras clases como atributos (ejemplo: un `Point` dentro de un `Segment`) o cuando tienes atributos `const`.
* **NO USAR ASIGNACION CLASICA:** Si el objeto que intentas crear no tiene un constructor por defecto (vacio), C++ lanzara un error si intentas asignarlo dentro de las llaves `{}`.

---

## 3. Composicion de Objetos

Es el principio de construir clases complejas usando clases mas simples. (Ejemplo: Un `Segment` hecho de dos `Point`).

```cpp
class Segment {
    private:
        Point p1; // Objeto miembro
        Point p2; // Objeto miembro
    public:
        // Obligatorio usar lista de inicializacion aqui
        Segment(const Point& origen, const Point& extremo) : p1(origen), p2(extremo) {}
};

```

### ¿Qué pasa en la memoria?

Al pasar los objetos y guardarlos en `p1` y `p2`, se activa el **Constructor de Copia**. C++ hace una copia exacta. Si modificas `p1` dentro del segmento, el punto original declarado fuera **no se ve afectado**.

---

## 4. El doble uso de `const` y las Referencias (`&`)

Este es el concepto mas importante para escribir codigo C++ profesional y seguro.

Mira esta firma de metodo:

```cpp
bool egalite(const Point& otro_punto) const;

```

### A. El primer `const` y el ampersand (`const Point&`)

* **`&` (Referencia):** En lugar de hacer una copia del objeto (lo cual gasta memoria y tiempo), le pasas la direccion original.
* **`const`:** Actua como un candado. Le promete al compilador que no modificara el objeto recibido por referencia.

### B. El segundo `const` (Al final de la funcion)

* **Significado:** Le promete al compilador que este metodo es de **solo lectura** para el objeto que lo llama. No alterara sus propios atributos (`X` o `Y`).

### Cuándo usar y cuándo NO:

* **USAR REFERENCIAS CONSTANTES (`const &`):** Siempre que pases objetos a una funcion (como `Point`, `String`, etc.).
* **NO USAR REFERENCIAS:** Con tipos primitivos pequenos (`int`, `float`, `bool`), pasarlos por valor (copia normal) es suficiente.
* **USAR `const` AL FINAL:** En todos los metodos **Getters**, comparaciones (`esIgual`), calculos (`distance`) y funciones de mostrar (`affichage`).
* **NO USAR `const` AL FINAL:** En los **Setters** (`setX`, `setY`) o funciones como `translation`, ya que su proposito explicito es modificar el objeto.

---

## 5. Encapsulamiento (Getters, Setters y `this`)

* **Constructores:** Nacimiento del objeto (se usa 1 vez).
* **Setters:** Cambiar el estado del objeto durante su vida util (infinitas veces). Permiten validar reglas logicas (ejemplo: `if (radio > 0)`).
* **El puntero `this`:** Solo es necesario si el nombre del parametro choca con el nombre del atributo. Si los nombres son distintos, C++ lo asume automaticamente.

```cpp
// Con colision de nombres (Necesitas 'this')
void Point::setX(float X) { this->X = X; }

// Sin colision de nombres (Codigo mas limpio)
void Point::setX(float nuevo_X) { X = nuevo_X; }

```

---

## 6. Arreglos de Objetos y Comparaciones (Logica de Conjuntos)

Cuando creas un arreglo de objetos personalizados (ejemplo: `Point tab[100]`), **no puedes usar el operador `==**` directamente, porque C++ no sabe que hace a dos puntos iguales por defecto.

Debes crear tu propio metodo de comparacion y usarlo al iterar:

```cpp
bool EnsemblePoint::test(const Point& p) const {
    for (int i = 0; i < nb; i++) {
        if (tab[i].egalite(p)) { // Uso de metodo propio en lugar de '=='
            return true; 
        }
    }
    return false;
}

```

```

```
