
// Seanse 1
---

void (int a, int b, int z = 5) YES
void (int a, int z = 5, int b) NO

---
#Variables locales

#defin c1 1.25

#defin c2 1.12

#defin c3 c1 + c2 #(2.37)

int = 3*c3 == 3*c1+c2
---

//boucles
for (int i = 0; i<20,i++){ (20 veces)
  ...
}


---
// structure
es un type sin **typedef**
struct INDIVIDU{
  int age
  char nom[20]
}

INDIVIDU x;
x.age = 25

---
// Allocation dynamique

INDIVIDU* p;
p= new INDIVIDU (new hace guarda toda la instancia en p, y va crear un apuntador hacia (o desde nose) p)
p>age=25

// Crear un tableu
int* t;
t = new int[100]
t[0] = 5;


#liberation
delete p;
delete [] t;

orden: primero borrar la estructura (o array)  y luego las variables con sus apuntadores

/ Afichages et saises.

int = x 12
char = ch[50]
strcpy(ch,"bonyurt")
cout<<"entier"<<x<<"chaine"<<ch<<endl
      |              |             |
le flux sortie     variable    retour a la ligne
la terminal

int x
char ch[50]
cin>>x>>ch range le mots ecrit sur le flux dans la variable



/ se puede imprimir un INDIVIDU.
---
1) Passage por valeur: int f(INDIVIDU p);

2) Passage por adresse: int f(INDIVIDU* p);

q.age = 25
f(q)
g(*q)

diferencia?

1) du modification au p ne modifie pas q

q es copie a p

2) si je modifi le contennu pointe par p, q est modifie
3) q mest past copie dans p

mejor hacer:

3) int g(const INDIVIDU* p)

(responder porque)

// References

int g(INDIVIDU & p); p mest pas une variable locale

int g(const INDIVIDU& p) tput modification de p est interdite


int* p;              // puntero normal
                      // puede cambiar el valor apuntado, y puede reapuntar

const int* p;        // puntero a algo constante
                      // NO puede cambiar el valor apuntado
                      // SI puede reapuntar a otra cosa

int* const p;        // puntero constante
                      // SI puede cambiar el valor apuntado
                      // NO puede reapuntar (queda fijo a esa dirección)

const int* const p;  // todo constante
                      // no puede hacer ninguna de las dos cosas



INDIVIDU p;              // copia el objeto, se puede modificar la copia
const INDIVIDU p;        // copia el objeto, NO se puede modificar la copia
INDIVIDU& p;              // no copia, referencia al original, se puede modificar el original
const INDIVIDU& p;        // no copia, referencia al original, NO se puede modificar





## Puntero — definición

Un puntero es una variable normal cuyo contenido es una dirección de memoria. Como es una variable normal, se puede reasignar, dejar vacía, o apuntar a otra cosa.

```
int x = 5;
int y = 8;
int* p = &x;   // p guarda la dirección de x

p = &y;        // p ahora guarda la dirección de y
               // x sigue valiendo 5, no se tocó nada de x
```

## Referencia — definición

Una referencia no es una variable con contenido propio: es un segundo nombre para una variable que ya existe. No ocupa una casilla de memoria aparte, y una vez creada no se puede "reapuntar" a otra cosa.

```
int x = 5;
int& r = x;    // r y x son la misma casilla, con dos nombres

r = 8;         // esto cambia x directamente, porque son lo mismo
```

## Diferencia central entre puntero y referencia

```
int x = 5, y = 8;

int* p = &x;
p = &y;        // valido: p ahora apunta a otra cosa

int& r = x;
r = y;         // esto NO hace que r "apunte" a y
               // copia el valor de y (8) dentro de x
               // x ahora vale 8, r sigue siendo un alias de x
```

Un puntero se puede reasignar a otra dirección. Una referencia, una vez ligada a una variable, queda ligada a esa variable para siempre; cualquier asignación posterior modifica el valor, no el vínculo.


## * — también tiene dos significados distintos

Definición 1: parte del tipo, al declarar un puntero. Dice "esta variable va a guardar una dirección".
```
int* p;   // p es un puntero a int, todavía vacío
```

Definición 2: operador de dereferencia. Se usa sobre un puntero ya existente para ir a la dirección que guarda y leer o escribir ahí.
```
p = &x;
*p = 10;  // ve a donde apunta p (o sea, a x) y pon 10 ahí
```

Misma regla para distinguirlos: pegado al tipo en la declaración (`int*`) es "esto es un puntero". Pegado a una variable ya declarada (`*p`) es "dame lo que hay en la dirección que guarda p".

## & — tiene dos significados distintos

Definición 1: operador "dirección de". Se usa sobre una variable ya existente para obtener su dirección.
```
int x = 5;
&x        // esto da la dirección donde vive x, no su valor
```

Definición 2: parte del tipo, al declarar una referencia. Aquí no significa "dirección de", significa "esto es un alias".
```
int& r = x;   // r es otro nombre para x, no una dirección
```

Cómo distinguirlos: si `&` está pegado al tipo en una declaración (`int&`), es una referencia. Si está pegado a una variable ya declarada (`&x`), es el operador dirección de.





