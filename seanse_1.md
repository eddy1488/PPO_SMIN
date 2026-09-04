
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
