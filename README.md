# Rapport Go (Golang)

Ce document rassemble tout ce qu’il faut savoir sur Go (Golang), ainsi que les points intéressants découverts à travers des exercices et projets. Réalisé en groupe de 3-4 personnes, incluant des exercices sur Exercism (5 points individuels + jusqu’à 2 points bonus).

---


## Introduction

### Introduction à Go

Go (ou Golang) est un langage de programmation open source développé par Google en 2007 et rendu public en 2009.
Il est conçu pour être simple, rapide, efficace et particulièrement adapté au développement de logiciels systèmes, serveurs web, applications cloud et outils réseau.

### À propos des bases

Go est un langage de programmation compilé et typé statiquement. Sa syntaxe est similaire à celle du C. On l'appelle souvent **Golang** en raison de son ancien nom de domaine, `golang.org`, mais son nom véritable est **Go**.

Le concept de base présente trois fonctionnalités majeures du langage :
- Les **packages**
- Les **fonctions**
- Les **variables**

---

### Packages

Les applications Go sont organisées en **paquets**.
Un paquet est un ensemble de fichiers sources situés dans un même répertoire.
Tous les fichiers sources d'un répertoire doivent porter le même nom de paquet.

- Par convention, le nom du paquet correspond au dernier répertoire du chemin d'importation.
  Par exemple : les fichiers du paquet `math/rand` commencent par l'instruction :
  ```go
  package rand
  ```

- Lors de l'importation d'un paquet, seules les entités (fonctions, types, variables, constantes) dont le nom commence par une majuscule sont exportées (accessibles).

- Le style de nommage recommandé en Go est :
`camelCase` pour les identifiants internes au paquet.
`PascalCase` pour les identifiants exportés (accessibles entre les paquets).


## Philosophie de Go

- Simplicité & Lisibilité
- Performance
- Productivité
- Concurrence facile

---

## Syntaxe et Concepts Clés

### Fichier et package

```go
package main

func main() {
    // point d’entrée
}
```

### Variables et Types

Go est un langage **statiquement typé** : chaque variable doit avoir un type connu lors de la compilation.

#### Déclaration explicite

Vous pouvez déclarer une variable en précisant explicitement son type :

```go
var nombre int // Typage explicite
```

#### Déclaration implicite
Vous pouvez également déclarer une variable sans préciser son type, le compilateur le déduira :

```go
var nombre = 42 // Typage implicite
```

#### Déclaration courte
Vous pouvez utiliser la syntaxe courte pour déclarer et initialiser une variable :

```go
nombre := 42 // Déclaration courte
```

### Constantes

Les **constantes** contiennent une valeur fixe qui ne peut pas être modifiée pendant l’exécution du programme.

Elles sont définies avec le mot-clé `const` et peuvent être des nombres, des caractères, des chaînes ou des booléens :

```go
const pi = 3.14 // Déclaration d'une constante
```

### Types de base
- `int`, `float64`, `bool`, `string`
- `byte` (alias pour `uint8`)
- `rune` (alias pour `int32`, représente un caractère Unicode)

### Mise en Forme des Chaînes (fmt)
Go offre le package `fmt`, très pratique pour formater les entrées et sorties.
La fonction la plus courante est `Sprintf`, qui utilise des verbes comme `%s` pour insérer des valeurs dans une chaîne et retourne la chaîne formatée.

```go
import "fmt"

food := "taco"
fmt.Sprintf("Bring me a %s", food)
// Résultat : "Bring me a taco"
```

### Formatage des nombres à virgule flottante
`Sprintf` propose plusieurs verbes pour formater les nombres à virgule flottante :

- `%g` : représentation compacte
- `%e` : notation exponentielle
- `%f` : format fixe

On peut aussi contrôler la largeur du champ et la précision des chiffres :

```go
fmt.Sprintf("%.2f", 3.14159) // "3.14"
fmt.Sprintf("%10.2f", 3.14159) // "      3.14" (10 caractères de large, 2 décimales)
```

- Autres fonctions `fmt`
Le package `fmt` propose aussi :
- `Println` : imprime directement les arguments.
- `Printf` : formate les arguments (comme Sprintf) puis les imprime.


### Tableaux et Slices

```go
var arr [5]int // Tableau de 5 entiers
slice := []int{1, 2, 3} // Slice d'entiers
```
### Maps

```go
m := make(map[string]int) // Création d'une map
m["key"] = 42 // Ajout d'une entrée
```

### Comparaisons

En Go, les nombres et les chaînes peuvent être comparés grâce à des **opérateurs relationnels et d’égalité** :

| Comparaison       | Opérateur |
|-------------------|-----------|
| égal              | `==`      |
| différent         | `!=`      |
| inférieur         | `<`       |
| inférieur ou égal | `<=`      |
| supérieur         | `>`       |
| supérieur ou égal | `>=`      |

Le résultat d’une comparaison est toujours un booléen (`true` ou `false`).

Exemple numérique :

```go
a := 3

a != 4 // true
a > 5  // false
```

Ces opérateurs fonctionnent aussi sur les chaînes de caractères (ordre lexicographique) :

```go
"apple" < "banana"  // true
"apple" > "banana"  // false
```

### Instructions Conditionnelles (if)
Les conditions en Go utilisent le type `bool` (`true` ou `false`) et sont similaires aux autres langages.

- Exemple simple:

```go
var value string

if value == "val" {
    return "was val"
}
```

- Chaînage avec `else if` et `else`:
```go
if value == "val1" {
    return "was val1"
} else if value == "val2" {
    return "was val2"
} else {
    return "was something else"
}
```

- Initialisation dans la condition:
Une instruction d'initialisation peut précéder la condition :

```go
if value, err := getValue(); err == nil {
    fmt.Println("Value:", value)
} else {
    fmt.Println("Error:", err)
}
```

> Note : La variable `value` n'est accessible qu'à l'intérieur du bloc `if`.

### Fonctions

Les **fonctions** en Go acceptent zéro ou plusieurs paramètres. Chaque paramètre doit avoir un type explicite – il n’y a pas d’inférence de type pour les paramètres.

Les fonctions renvoient des valeurs grâce au mot-clé `return`.

Pour appeler une fonction, on indique son nom et on lui passe les arguments nécessaires.

Exemple :

```go
package greeting

// Hello est une fonction exportée (publique).
func Hello(name string) string {
    return hi(name)
}

// hi est une fonction interne (privée au package).
func hi(name string) string {
    return "hi " + name
}
```

### Commentaires de documentation

En Go, les **commentaires** jouent un rôle important pour la documentation du code.
Ils sont utilisés par la commande `godoc`, qui extrait ces commentaires pour créer la documentation des packages Go.
Un commentaire de documentation doit être une phrase complète qui commence par le nom de l’élément décrit et se termine par un point.

Les commentaires doivent précéder les **packages** ainsi que les identifiants exportés : fonctions, méthodes, variables, constantes, et structs.

#### Exemple de variable au niveau du package :

```go
// TemperatureCelsius représente une température en degrés Celsius.
var TemperatureCelsius float6
```

#### Commentaires de package
Ils doivent être écrits directement avant la clause du package (package x) et commencer par Package x … :

```go
// Package math fournit des fonctions mathématiques de base.
package math
```

#### Commentaires de fonction
```go
// Add retourne la somme de deux entiers.
func Add(a int, b int) int {
    return a + b
}
```

### Pointeurs

```go
func increment(x *int) {
    *x++
}
```

### Structs

```go
type Person struct {
    Name string
    Age  int
}

func (p *Person) Greet() {
    fmt.Println("Hello, my name is", p.Name)
}
```

### Interfaces

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

### Concurrence : Goroutines & Channels

```go
go doSomething()

ch := make(chan int)
go func() { ch <- 42 }()
value := <-ch
```

## Standard Library

- net/http → serveur web
- fmt → formatage et impression
- os → système de fichiers
- testing → framework de test intégré
- encoding/json → manipulation JSON

## Gestion des Dépendances
```bash
go mod init <module_name>
go get <package>
go mod tidy
```

## Compilation et Déploiement
```bash
go build
GOOS=windows GOARCH=amd64 go build
```

## Bonnes Pratiques & Points Remarquables

### Organisation du code
- Packages clairs et petits
- go fmt pour le formatage automatique

### Gestion explicite des erreurs

```go
result, err := doSomething()
if err != nil {
    // gérer l’erreur
}
```

### Tests Unitaires

```go
func TestAdd(t *testing.T) {
    if add(2, 3) != 5 {
        t.Error("Expected 5")
    }
}
```

```bash
go test ./...
```

## Points Intéressants Observés en Pratique

- Vitesse de compilation impressionnante
- Composition via interfaces et structs plutôt que l’héritage
- Concurrence native avec des goroutines légères


### Slices

Les **slices** en Go sont similaires aux listes ou tableaux dynamiques dans d’autres langages.
Ils contiennent des éléments d’un type spécifique.

- Les **arrays** ont une taille fixe.
- Les **slices** sont dynamiques et basés sur les arrays.

Déclaration :

```go
var empty []int                // un slice vide
withData := []int{0,1,2,3,4,5} // un slice pré-rempli


Accès et modification :

```go
withData[1] = 5
x := withData[1] // x vaut maintenant 5
```

Création de sous-slices :

```go
newSlice := withData[2:4] // []int{2,3}
newSlice := withData[:2]  // []int{0,1}
newSlice := withData[2:]  // []int{2,3,4,5}
newSlice := withData[:]   // []int{0,1,2,3,4,5}
```

Ajout d’éléments avec `append` :

```go
a := []int{1, 3}
a = append(a, 4, 2) // => []int{1,3,4,2}
```

Fusion de slices :

```go
nextSlice := []int{100,101,102}
newSlice  := append(withData, nextSlice...) // => []int{0,1,2,3,4,5,100,101,102}
```

---

#### Fonctions Variadiques

En général, les fonctions Go attendent un nombre fixe d’arguments.
Mais Go permet aussi les **fonctions variadiques** (nombre variable d’arguments).

Une fonction variadique utilise le préfixe `...` sur le dernier paramètre :

```go
func find(a int, b ...int) {
    // ...
}
```

Exemples d’appels :

```go
find(5, 6)
find(5, 6, 7)
find(5)
```

⚠️ Le paramètre variadique doit toujours être le **dernier** paramètre.

---

#### Exemple complet

```go
func find(num int, nums ...int) {
    fmt.Printf("type of nums is %T\n", nums)

    for i, v := range nums {
        if v == num {
            fmt.Println(num, "found at index", i, "in", nums)
            return
        }
    }

    fmt.Println(num, "not found in", nums)
}
```

func main() {
    find(89, 90, 91, 95)
    // type of nums is []int
    // 89 not found in [90 91 95]

    find(45, 56, 67, 45, 90, 109)
    // type of nums is []int
    // 45 found at index 2 in [56 67 45 90 109]

    find(87)
    // type of nums is []int
    // 87 not found in []
}
```

Lorsqu’un slice existe déjà et qu’il faut le passer à une fonction variadique, il faut utiliser `...` :

```go
list := []int{1, 2, 3}
find(1, list...) // la fonction "find" utilise les éléments de list directement
```

### Switch

Comme dans d’autres langages, Go fournit l’instruction `switch` pour simplifier l’écriture des longues chaînes de conditions `if ... else if`.

#### Syntaxe de base

```go
operatingSystem := "windows"

switch operatingSystem {
case "windows":
    // faire quelque chose si l'OS est Windows
case "linux":
    // faire quelque chose si l'OS est Linux
case "macos":
    // faire quelque chose si l'OS est macOS
default:
    // si aucun des cas ne correspond
}
```

---

#### Switch sans expression

En Go, la valeur après `switch` peut être omise. Chaque `case` peut alors contenir une condition booléenne :

```go
age := 21

switch {
case age > 20 && age < 30:
    // si age est entre 20 et 30
case age == 10:
    // si age est égal à 10
default:
    // pour tous les autres cas
}
```

### Structs en Go

---

#### Définition

En Go, un **struct** est une collection de **champs nommés** (name: type).
Chaque champ a un nom unique dans le struct.
Les structs sont comparables aux **classes** en programmation orientée objet.

Exemple de définition :

```go
type Shape struct {
    name string
    size int
}


Les champs qui commencent par une lettre minuscule sont **privés** au package.
Ceux qui commencent par une majuscule sont **exportés** (accessibles dans d’autres packages).

---

#### Création d’instances

Création d’un struct avec initialisation des champs (dans n’importe quel ordre) :

```go
s := Shape{
    name: "Square",
    size: 25,
}
```

Modification des champs avec l’opérateur `.` :

```go
s.name = "Circle"
s.size = 35
fmt.Printf("name: %s size: %d\n", s.name, s.size)
// Output: name: Circle size: 35
```

Si un champ n’est pas initialisé, il prendra sa **valeur zéro** (0 pour int, "" pour string, etc.) :

```go
s := Shape{name: "Circle"}
fmt.Printf("name: %s size: %d\n", s.name, s.size)
// Output: name: Circle size: 0
```

---

#### Initialisation sans noms de champs

On peut initialiser un struct sans préciser les noms des champs, mais **l’ordre** doit être respecté :

```go
s := Shape{
    "Oval",
    20,
}
```

⚠️ Cette méthode est fragile : elle peut causer des erreurs si un nouveau champ est ajouté dans le struct.

---

#### Exemple : modification de struct

Si un champ est ajouté au struct :

```go
type Shape struct {
    name        string
    description string // nouveau champ
    size        int
}
```

L'initialisation sans noms de champs cassera :

```go
s := Shape{
    "Circle",
    20,
}
// Erreurs de compilation car la deuxième valeur est un int attendu comme string.
```

---

#### Fonctions `New`

En Go, il est courant de créer des fonctions pour **initialiser** des structs.
Ces fonctions sont appelées `NewXxx` par convention, mais ce sont des fonctions normales (pas des constructeurs).

Exemple :

```go
func NewShape(name string) Shape {
    return Shape{
        name: name,
        size: 100, // valeur par défaut
    }
}

NewShape("Triangle")
// => Shape { name: "Triangle", size: 100 }
```

Avantages des fonctions `New` :

* validation des valeurs fournies
* gestion des valeurs par défaut
* accès aux champs privés (si la fonction est dans le même package que le struct)

### Introduction au package `math/rand`

Le package **`math/rand`** permet de générer des nombres pseudo-aléatoires en Go.

---

#### Générer des entiers aléatoires

Pour obtenir un entier aléatoire compris entre 0 et 99 :

```go
n := rand.Intn(100) // n est un int aléatoire, 0 <= n < 100


---

#### Générer des nombres à virgule flottante

Pour un nombre flottant aléatoire compris entre 0.0 et 1.0 :

```go
f := rand.Float64() // f est un float64 aléatoire, 0.0 <= f < 1.0
```

---

#### Mélanger les éléments d’un slice

Le package propose aussi une méthode pour **mélanger** un slice :

```go
x := []string{"a", "b", "c", "d", "e"}

rand.Shuffle(len(x), func(i, j int) {
    x[i], x[j] = x[j], x[i]
})
```

---

#### Graine (Seed)

⚠️ Les nombres générés ne sont pas vraiment aléatoires : ils dépendent d’une **graine** (seed).

* En Go **1.20+**, la graine est choisie aléatoirement à chaque exécution → séquences différentes à chaque fois.
* Dans les versions plus anciennes, la graine par défaut est 1. Pour avoir des séquences différentes, il fallait initialiser la graine manuellement avec l’heure actuelle :

```go
rand.Seed(time.Now().UnixNano())
```

### Syntaxe générale de la boucle `for`

La boucle `for` est l’une des instructions les plus courantes pour exécuter **répétitivement** un bloc de code.
Elle se compose du mot-clé `for`, d’un **en-tête** et d’un **bloc de code** entouré par `{}`.

#### Structure

```go
for init; condition; post {
  // corps de la boucle
}
```


* **`init`** : exécuté **une seule fois** avant le début de la boucle.
* **`condition`** : expression booléenne ; tant qu’elle vaut `true`, la boucle continue.
* **`post`** : exécuté à la **fin de chaque itération**.

⚠️ Contrairement à d’autres langages, il n’y a **pas de parenthèses** `()` autour de l’en-tête.
En revanche, les accolades `{}` sont obligatoires pour le corps de la boucle.

---

#### Exemple

```go
for i := 1; i < 10; i++ {
    fmt.Println(i)
}
```

Dans cet exemple :

* `i := 1` : initialisation
* `i < 10` : condition de continuation
* `i++` : incrémentation après chaque itération

La boucle affichera les nombres de 1 à 9 inclus.

### Fonctions en Go

---

#### Introduction

Une **fonction** permet de regrouper du code réutilisable.
Elle est définie avec le mot-clé `func`, un nom, et une liste de paramètres (zéro ou plusieurs) explicitement typés.

---

#### Paramètres

- Tous les paramètres doivent être **explicitement typés**.
- Il n’y a pas de valeurs par défaut pour les paramètres.

Exemple :

```go
import "fmt"

// Sans paramètres
func PrintHello() {
    fmt.Println("Hello")
}

// Avec deux paramètres
func PrintGreetingName(greeting string, name string) {
    fmt.Println(greeting + " " + name)
}

// Déclaration groupée pour les mêmes types
func PrintGreetingNameCompact(greeting, name string) {
    fmt.Println(greeting + " " + name)
}
```


---

#### Paramètres vs Arguments

* **Paramètres** : noms des variables dans la signature de la fonction.
* **Arguments** : valeurs concrètes passées lors de l’appel de la fonction.

```go
PrintGreetingName("Hello", "Katrina") // "Hello" et "Katrina" sont des arguments
```

---

#### Valeurs de retour

Les fonctions peuvent retourner zéro, une ou plusieurs valeurs.
Elles doivent aussi être **explicitement typées**.

* Retour unique (sans parenthèses) :

```go
func Hello(name string) string {
    return "Hello " + name
}
```

* Retours multiples (parenthèses et valeurs séparées par des virgules) :

```go
func HelloAndGoodbye(name string) (string, string) {
    return "Hello " + name, "Goodbye " + name
}
```

---

#### Appel de fonctions

```go
import "fmt"

// Sans paramètres
func PrintHello() {
    fmt.Println("Hello")
}
PrintHello()

// Un paramètre, un retour
func Hello(name string) string {
    return "Hello " + name
}
greeting := Hello("Dave")

// Plusieurs paramètres et retours
func SumAndMultiply(a, b int) (int, int) {
    return a+b, a*b
}
sum, product := SumAndMultiply(a, b)
```

---

#### Valeurs de retour nommées et "naked return"

Les valeurs de retour peuvent être nommées dans la signature :

```go
func SumAndMultiplyThenMinus(a, b, c int) (sum, mult int) {
    sum, mult = a+b, a*b
    sum -= c
    mult -= c
    return // "naked return" qui retourne sum et mult
}
```

---

#### Passage par valeur vs par référence

En Go, tous les arguments sont passés **par valeur** (une copie est faite).
Exemple :

```go
val := 2
func MultiplyByTwo(v int) int {
    v = v * 2
    return v
}
newVal := MultiplyByTwo(val)
// newVal = 4, mais val reste 2
```

---

#### Pointeurs (passage par référence)

Pour modifier directement la donnée (pas la copie), on utilise des **pointeurs** :

```go
func HandlePointers(x, y *int) {
    // logique utilisant les valeurs référencées par x et y
}
```

Les pointeurs sont identifiés par le `*` devant le type.

---

#### Exceptions (slices et maps)

Les **slices** et **maps** font exception :

* Ils se comportent **comme des pointeurs** même sans `*` explicite.
* Les modifications faites dans la fonction affectent directement la donnée originale.

---


### Introduction aux nombres à virgule flottante

Un **nombre à virgule flottante** est un nombre qui peut comporter zéro ou plusieurs chiffres après la virgule.
Exemples : -2.4, 0.1, 3.14, 16.984025 et 1024.0.

La **précision** détermine combien de chiffres peuvent être stockés après la virgule.

---

#### Types de flottants en Go

Go offre deux types principaux :

- **`float32`** : 32 bits (~6-9 chiffres de précision)
- **`float64`** : 64 bits (~15-17 chiffres de précision, utilisé par défaut)

---

#### Choix du type

- Go utilise par défaut `float64` pour les nombres flottants.
- Pour utiliser `float32`, il faut :
  - déclarer explicitement la variable en `float32`,
  - ou recevoir une valeur `float32` d'une fonction,
  - ou utiliser la fonction `float32()` pour convertir.

---

#### Exemple de précision

```go
var pi32 float32 = 3.14159265359
var pi64 float64 = 3.14159265359

fmt.Println(pi32) // ~3.141593 (arrondi)
fmt.Println(pi64) // 3.14159265359
```


#### Le package `math`

Le package `math` fournit de nombreuses fonctions utiles pour les calculs mathématiques avec les nombres flottants.

### Introduction à la gestion du temps (`time`)

En Go, le type **`Time`** représente un instant précis.
Il permet d’accéder, de comparer et de manipuler la date et l’heure via ses méthodes, mais aussi à l’aide de fonctions du package `time`.

---

#### Récupérer la date et l’heure actuelles

```go
import "time"

now := time.Now()
````

---

#### Parsing de dates avec `time.Parse`

Pour convertir une chaîne en `Time`, on utilise **`time.Parse(layout, value)`**.
La particularité : le `layout` (format attendu) s’écrit avec des valeurs d’un **timestamp spécial** :

```
Mon Jan 2 15:04:05 -0700 MST 2006
```

Exemple :

```go
import "time"

func parseTime() time.Time {
    date := "Tue, 09/22/1995, 13:00"
    layout := "Mon, 01/02/2006, 15:04"

    t, err := time.Parse(layout, date) // time.Time, error
    if err != nil {
        panic(err)
    }
    return t
}

// => 1995-09-22 13:00:00 +0000 UTC
```

---

#### Formatage de dates avec `Time.Format`

Pour convertir un `Time` en **chaîne** :

```go
import (
    "fmt"
    "time"
)

func main() {
    t := time.Date(1995, time.September, 22, 13, 0, 0, 0, time.UTC)
    formattedTime := t.Format("Mon, 01/02/2006, 15:04")
    fmt.Println(formattedTime)
}
// => Fri, 09/22/1995, 13:00
```

---

#### Options de layout (formatage)

* **Année** : `2006` ; `06`
* **Mois** : `Jan` ; `January` ; `01` ; `1`
* **Jour** : `02` ; `2` ; `_2` (sans zéro)
* **Jour de semaine** : `Mon` ; `Monday`
* **Heure** : `15` (24h) ; `3` ; `03` (AM/PM)
* **Minute** : `04` ; `4`
* **Seconde** : `05` ; `5`
* **AM/PM** : `PM`
* **Jour de l’année** : `002` ; `__2`

---

#### Méthodes de `Time`

Exemples :

* `Time.Hour()`
* `Time.Month()`

---

#### Autres fonctionnalités de `time`

* `Duration` : durée écoulée
* Fuseaux horaires
* Timers

Ces concepts seront abordés plus en détail plus tard.

---

### Introduction aux Maps

En Go, un **map** est une structure intégrée qui associe des **clés** à des **valeurs**.
C’est similaire aux **dictionnaires** (Python), **hash tables** ou **associative arrays**.

---

#### Définition

La syntaxe générale :

```go
map[KeyType]ElementType
````

* Les **clés** sont **uniques** : assigner la même clé écrase la valeur précédente.

---

#### Création d’un map

* Avec un **littéral** :

```go
foo := map[string]int{}
```

* Avec la fonction **`make`** :

```go
foo := make(map[string]int)
```

---

#### Opérations sur un map

* **Ajouter ou mettre à jour** un élément :

```go
foo["bar"] = 42
foo["bar"] = 73 // met à jour la valeur de "bar"
```

* **Lire** une valeur :

```go
baz := foo["bar"]
```

* **Supprimer** un élément :

```go
delete(foo, "bar")
```

---

#### Vérifier l’existence d’une clé

Si on lit la valeur d’une clé qui **n’existe pas**, Go renvoie la valeur **zéro** du type de la valeur (0 pour `int`, `""` pour `string`, etc.).

Pour savoir si la clé existe vraiment :

```go
value, exists := foo["baz"]
// Si "baz" n’existe pas :
// value = 0 (valeur zéro de int), exists = false
```

---

### Parcourir des collections : `range` en Go

---

#### Itération sur un `slice`

En Go, on peut itérer sur un **slice** de deux manières :
- En utilisant une boucle `for` avec un index.
- En utilisant `range`, qui simplifie l’itération.

Avec `range`, chaque itération retourne deux valeurs :
1. l’index (ou la clé dans un `map`)
2. une copie de l’élément à cet index/clé.

Exemple avec un **slice** :

```go
xi := []int{10, 20, 30}
for i, x := range xi {
    fmt.Println(i, x)
}
// Résultat :
// 0 10
// 1 20
// 2 30
```

---

#### Itération sur un `map`

Les **maps** en Go n’ont pas d’ordre !
Lorsqu’on les parcourt avec `range`, l’ordre des paires clé/valeur est **aléatoire**.

Exemple :

```go
hash := map[int]int{9: 10, 99: 20, 999: 30}
for k, v := range hash {
    fmt.Println(k, v)
}
// Exemple de sortie :
// 99 20
// 999 30
// 9 10
```

ℹ️ La sortie peut différer à chaque exécution car les **maps sont non ordonnées**.
Pour plus de détails, voir [Go Language Spec: range clause](https://go.dev/ref/spec#RangeClause).

---

#### Omettre les variables inutilisées

En Go, une variable non utilisée provoque une erreur de compilation.
Si vous n’utilisez pas une des deux valeurs (`index` ou `value`), vous pouvez utiliser `_` pour l’ignorer.

* **Ignorer l’index** et ne garder que la valeur :

```go
xi := []int{10, 20, 30}
for _, x := range xi {
    fmt.Println(x)
}
// Résultat :
// 10
// 20
// 30
```

* **Ignorer la valeur** et ne garder que l’index :

```go
xi := []int{10, 20, 30}
for i := range xi {
    fmt.Println(i)
}
// Résultat :
// 0
// 1
// 2
```

---

### Types personnalisés non-struct

En plus des `structs`, Go permet de créer des **types alias** pour des types intégrés (comme `string`, `int`, `[]string`, etc.) et d’y ajouter des méthodes pour les enrichir.

---

#### Exemple de type `string` enrichi

```go
type Name string

func SayHello(n Name) {
    fmt.Printf("Hello %s\n", n)
}

n := Name("Fred")
SayHello(n)
// Résultat : Hello Fred
```

---

#### Exemple de type `slice` enrichi

```go
type Names []string

func SayHello(n Names) {
    for _, name := range n {
        fmt.Printf("Hello %s\n", name)
    }
}

n := Names([]string{"Fred", "Bill"})
SayHello(n)
// Résultat :
// Hello Fred
// Hello Bill
```

Ces types personnalisés offrent une **meilleure organisation** du code et une **possibilité d’extension** via des méthodes.

### Introduction aux pointeurs en Go

Comme dans beaucoup d'autres langages, Go propose des **pointeurs**. Pour les débutants, cela peut sembler un peu mystérieux, mais une fois qu’on a compris, c’est assez direct. Les pointeurs sont fondamentaux en Go, donc ça vaut le coup de bien les comprendre.

Avant de plonger dans les détails, voyons pourquoi on les utilise :

* **Optimisation de la mémoire** : lorsqu’on a de grandes quantités de données, les copier pour les passer entre fonctions est inefficace. Avec les pointeurs, on partage directement l’adresse en mémoire, réduisant ainsi l’empreinte mémoire.
* **Modification directe** : en passant des pointeurs entre fonctions, on modifie la donnée originale. Ainsi, les changements sont immédiatement visibles par les autres parties du programme.

---

#### Variables et mémoire

Prenons une variable entière classique :

```go
var a int
```

Go réserve un emplacement mémoire pour stocker sa valeur. Par exemple :

```go
a = 3
```

La valeur 3 est maintenant stockée en mémoire à l’emplacement réservé pour `a`.

---

#### Définir un pointeur

Un **pointeur** stocke l’adresse mémoire d’une valeur. Pour déclarer un pointeur vers un entier :

```go
var p *int
```

Ici, `p` peut contenir l’adresse d’un entier. Par défaut, un pointeur non initialisé vaut `nil` (pas d’adresse).

---

#### Obtenir l’adresse d’une variable (`&`)

Pour obtenir l’adresse mémoire de `a` :

```go
var a int = 2
var p *int = &a
```

Maintenant, `p` contient l’adresse de `a`.

---

#### Lire/modifier la valeur à l’adresse (`*`)

Pour accéder à la valeur pointée (déréférencement) :

```go
var a int = 2
var p *int = &a

var b int = *p // b == 2
```

Pour **modifier la valeur à l’adresse** :

```go
var a int = 2
var pa *int = &a

*pa = *pa + 2 // incrémente la valeur pointée par pa

fmt.Println(a) // Affiche : 4
```

Ici, `*pa` modifie directement la valeur de `a` !

⚠️ Important : toujours vérifier qu’un pointeur n’est pas `nil` avant de le déréférencer, sinon le programme plantera :

```go
var p *int
fmt.Println(*p) // panic: invalid memory address or nil pointer dereference
```

---

#### Pointeurs vers des `struct`

Les pointeurs ne sont pas limités aux types de base. Exemple avec un `struct` :

```go
type Person struct {
    Name string
    Age  int
}

var peter Person = Person{Name: "Peter", Age: 22}
var p *Person = &peter
```

On peut aussi directement créer un pointeur sur un nouveau `struct` :

```go
var p *Person = &Person{Name: "Peter", Age: 22}
```

Avec un pointeur vers un `struct`, on n’a pas besoin de déréférencer pour accéder aux champs :

```go
fmt.Println(p.Name) // Go déréférence automatiquement
```

---

#### Slices et maps sont déjà des pointeurs

Les **slices** et les **maps** sont des structures particulières qui utilisent déjà des pointeurs dans leur implémentation. Du coup, on n’a généralement **pas besoin de créer des pointeurs** pour eux.

Exemple : une fonction qui incrémente l’âge dans un `map` :

```go
func incrementPeterAge(m map[string]int) {
    m["Peter"] += 1
}
```

Même sans pointeur explicite, la modification est visible après l’appel :

```go
ages := map[string]int{"Peter": 21}
incrementPeterAge(ages)
fmt.Println(ages) // Résultat : map[Peter:22]
```

⚠️ Attention avec `append` sur les slices : comme il crée souvent un nouveau slice, les modifications peuvent ne pas se répercuter à l’extérieur de la fonction (sujet plus avancé).

Voilà pour un tour d’horizon complet sur les pointeurs en Go !

### Introduction aux méthodes en Go

En Go, une **méthode** est simplement une fonction spéciale avec un **récepteur** (`receiver`). Le récepteur apparaît dans la signature, entre le mot-clé `func` et le nom de la méthode :

```go
func (receiver Type) MethodName(parameters) (retour) {
    // corps de la méthode
}
```

💡 Important : le type du récepteur doit être défini **dans le même package** que la méthode.

---

#### Exemple de méthode sur un `struct`

```go
package person

type Person struct {
    Name string
}

func (p Person) Greetings() string {
    return fmt.Sprintf("Welcome %s!", p.Name)
}
```

Appel d’une méthode avec la **notation pointée** (comme en POO) :

```go
p := Person{Name: "Bronson"}
fmt.Println(p.Greetings())
// Résultat : Welcome Bronson!
```

---

#### Avantages des méthodes

Les méthodes permettent d’**éviter les conflits de noms** :
le même nom de méthode peut exister sur plusieurs types différents.

---

#### Méthodes sur différents types

Exemple avec deux types différents (`rect` et `circle`) et la même méthode `area` :

```go
import "math"

type rect struct {
    width, height int
}

func (r rect) area() int {
    return r.width * r.height
}

type circle struct {
    radius int
}

func (c circle) area() float64 {
    return math.Pi * math.Pow(float64(c.radius), 2)
}
```

---

#### Récepteurs : valeur ou pointeur

* Les **récepteurs par valeur** (comme `r rect`) reçoivent une **copie** de l’instance.
  Les modifications ne sont donc **pas visibles** à l’extérieur de la méthode.

* Les **récepteurs par pointeur** (comme `r *rect`) reçoivent l’**adresse**.
  Les modifications **affectent directement l’instance originale**.

---

#### Exemple de méthode avec un pointeur récepteur

```go
type rect struct {
    width, height int
}

func (r *rect) squareIt() {
    r.height = r.width
}

r := rect{width: 10, height: 20}
fmt.Printf("Width: %d, Height: %d\n", r.width, r.height)
// Résultat : Width: 10, Height: 20

r.squareIt()
fmt.Printf("Width: %d, Height: %d\n", r.width, r.height)
// Résultat : Width: 10, Height: 10
```

Ici, la méthode `squareIt` a un récepteur par pointeur (`*rect`) :
les changements faits dans la méthode sont visibles **hors de la méthode**.

---

### Introduction aux runes en Go

Le type **rune** en Go est un alias pour `int32`. Cela signifie qu'une rune stocke un entier signé sur 32 bits.
La différence avec un simple `int32` ? La valeur stockée dans une rune représente **un caractère Unicode unique**.

---

#### Unicode et points de code

**Unicode** est un standard qui attribue un **numéro unique** à chaque caractère (lettres, symboles, emoji…).
Ce numéro est appelé un **point de code Unicode**.

Exemples :

| Caractère | Point de code | Valeur décimale |
| --------- | ------------- | --------------- |
| 0         | U+0030        | 48              |
| A         | U+0041        | 65              |
| a         | U+0061        | 97              |
| ¿         | U+00BF        | 191             |
| π         | U+03C0        | 960             |
| 🧠        | U+1F9E0       | 129504          |

---

#### UTF-8 et Go

Go encode ses fichiers sources en **UTF-8**, un encodage qui représente chaque point de code en 1 à 4 octets.
Puisqu'une rune peut occuper jusqu’à 4 octets, elle est basée sur `int32` (32 bits = 4 octets).

---

#### Utilisation des runes

Déclarer une rune :

```go
myRune := '¿'
```

Afficher le type :

```go
fmt.Printf("Type : %T\n", myRune) // int32
```

Afficher la **valeur entière** (décimale) :

```go
fmt.Printf("Valeur : %v\n", myRune) // 191
```

Afficher le **caractère Unicode** :

```go
fmt.Printf("Caractère : %c\n", myRune) // ¿
```

Afficher le **point de code Unicode** :

```go
fmt.Printf("Point de code : %U\n", myRune) // U+00BF
```

---

#### Runes et chaînes de caractères

Les **chaînes de caractères** (`string`) en Go sont encodées en UTF-8 et contiennent donc des caractères Unicode.
Quand on **itère sur une chaîne avec `range`**, Go convertit chaque séquence en rune.
Ainsi, même si une chaîne est un tableau de **bytes**, `range` parcourt les **runes**.

Exemple :

```go
myString := "❗hello"
for index, char := range myString {
    fmt.Printf("Index: %d\tCaractère: %c\tPoint de code: %U\n", index, char, char)
}
```

Sortie :

```
Index: 0	Caractère: ❗	Point de code: U+2757
Index: 3	Caractère: h	Point de code: U+0068
Index: 4	Caractère: e	Point de code: U+0065
Index: 5	Caractère: l	Point de code: U+006C
Index: 6	Caractère: l	Point de code: U+006C
Index: 7	Caractère: o	Point de code: U+006F
```

---

#### Différence entre la longueur en bytes et le nombre de caractères

La fonction `len` retourne la **taille en bytes** d'une chaîne.
Pour compter le **nombre de runes (caractères)**, on utilise `utf8.RuneCountInString`.

```go
import "unicode/utf8"

myString := "❗hello"
lengthBytes := len(myString)
lengthRunes := utf8.RuneCountInString(myString)

fmt.Printf("Longueur en bytes: %d - Nombre de runes: %d\n", lengthBytes, lengthRunes)
// Résultat : Longueur en bytes: 8 - Nombre de runes: 6
```

---

Les runes sont essentielles pour travailler proprement avec les chaînes Unicode en Go, car elles respectent le découpage réel des caractères, même pour les symboles ou emoji !

### Introduction à `regexp` en Go

Le package **`regexp`** de Go offre un support pour les expressions régulières (regex).
La syntaxe des regex est similaire à celle de Perl, Python, et d’autres langages.

Les **chaînes et les motifs** sont traités comme des textes en UTF-8.
💡 Les backticks `` ` `` sont particulièrement utiles pour écrire des regex en Go, car ils désactivent l’interprétation des backslashes (`\`) comme caractères spéciaux.

---

#### Compilation des motifs

Avant d’utiliser une regex, on doit **compiler le motif** (conversion en une représentation interne).

* Cela se fait **une seule fois** avec la fonction `regexp.Compile` (retourne une `Regexp` et une erreur si le motif est invalide).
* Ou avec `regexp.MustCompile` (panique si le motif est invalide, donc à utiliser seulement si on est sûr que le motif est correct).

Exemple :

```go
re, err := regexp.Compile(`(a|b)+`)
fmt.Println(re, err) // (a|b)+ <nil>

re = regexp.MustCompile(`[a-z]+\d*`)
```

---

#### Méthodes principales

Le package `regexp` fournit de nombreuses méthodes, regroupées sous le type `Regexp`.
Elles ont souvent des noms formés sur le modèle suivant :
`Find(All)?(String)?(Submatch)?(Index)?`

Quelques méthodes essentielles :

✅ **`MatchString`** : teste si une chaîne contient une correspondance.

```go
re := regexp.MustCompile(`[a-z]+\d*`)
b := re.MatchString(" abc!") // true
b := re.MatchString("123 456") // false
```

✅ **`FindString`** : renvoie la **première correspondance** trouvée.

```go
s := re.FindString("12abc34(ef)") // "abc34"
```

✅ **`FindStringSubmatch`** : renvoie un **slice** avec la correspondance globale et celles des groupes capturants.

```go
sl := re.FindStringSubmatch("12abc34(ef)")
// []string{"abc34", "34"}
```

✅ **`ReplaceAllString`** : remplace toutes les correspondances par une chaîne de remplacement.

```go
s := re.ReplaceAllString(" abc!", "X") // " X!"
```

✅ **`Split`** : découpe une chaîne en sous-chaînes séparées par le motif.

```go
sl := re.Split("12abc34(ef)", 2) // []string{"12", "(ef)"}
```

---

#### Conseils et remarques

* Les regex sont des **outils puissants** pour traiter et valider des chaînes.
* `MustCompile` est pratique mais doit être utilisé uniquement si le motif est **garanti valide**, sinon le programme plantera.
* Le package `regexp` comprend plus de **40 fonctions et méthodes** ! Pour les découvrir en détail, consulte la [documentation officielle](https://pkg.go.dev/regexp).



### Introduction aux interfaces en Go

---

#### Définition d’une interface

Une interface est un **ensemble de signatures de méthodes**.
Exemple :

```go
type Counter interface {
    Add(increment int)
    Value() int
}
```

Les noms des paramètres (`increment` ici) sont optionnels, mais souvent plus lisibles.

Les interfaces en Go n’utilisent pas le suffixe `Interface` ou `I` (pas `ICounter`), mais souvent un nom en `er` (ex. `Reader`, `Writer`).

---

#### Implémentation d’une interface

Tout type qui **définit les méthodes** de l’interface **implémente implicitement** l’interface.
Il n’y a **pas** de mot-clé `implements` en Go.

Exemple :

```go
type Stats struct {
    value int
}

func (s Stats) Add(v int) {
    s.value += v
}

func (s Stats) Value() int {
    return s.value
}

func (s Stats) SomeOtherMethod() {
    // Méthodes supplémentaires qui ne font pas partie de l'interface
}
```

---

#### Utiliser l’interface

Un **type interface** peut contenir toute valeur qui implémente ses méthodes.

```go
func SetUpAnalytics(counter Counter) {
    // ...
}

stats := Stats{}
SetUpAnalytics(stats) // fonctionne car Stats implémente Counter
```

Un type peut implémenter plusieurs interfaces, simplement en définissant les méthodes nécessaires.

---

#### L’interface vide

Go possède une **interface vide** spéciale, `interface{}` (ou `any` à partir de Go 1.18).
Elle ne contient **aucune méthode** et est implémentée par **tous les types**.
Utile pour accepter n’importe quelle valeur dans une fonction.

```go
func PrintAny(v interface{}) {
    fmt.Println(v)
}
```

### Introduction aux valeurs par défaut (zero value) en Go

En Go, il n’existe pas de `null`, `empty` ou `undefined`.
Lorsqu’une variable est déclarée sans valeur explicite, elle prend automatiquement la **valeur par défaut** (zero value) de son type.

---

#### Valeurs par défaut des types primitifs

| Type      | Valeur par défaut  |
| --------- | ------------------ |
| booléen   | `false`            |
| numérique | `0`                |
| string    | `""` (chaîne vide) |

---

#### Valeur `nil` pour les types plus complexes

Le mot-clé `nil` représente l’absence de valeur pour :

* Pointeurs
* Fonctions
* Interfaces
* Slices
* Channels
* Maps

| Type      | Valeur par défaut |
| --------- | ----------------- |
| pointeur  | `nil`             |
| fonction  | `nil`             |
| interface | `nil`             |
| slice     | `nil`             |
| channel   | `nil`             |
| map       | `nil`             |

---

#### Zero value pour les structs

Les structs n’apparaissent pas dans la table car leur zero value dépend de leurs **champs**.
Un struct est à son zero value lorsque tous ses champs sont à leur zero value respective.

```go
type MyStruct struct {
    Flag   bool
    Number int
    Name   string
}

var s MyStruct
fmt.Printf("%+v\n", s) // {Flag:false Number:0 Name:""}
```

### Introduction à l’interface `Stringer` en Go

`Stringer` est une interface standard en Go, définie pour fournir une **représentation textuelle** lisible des valeurs.

---

#### Définition de l’interface

```go
type Stringer interface {
    String() string
}
```

Tout type qui souhaite personnaliser l’affichage de ses valeurs doit définir une méthode `String()` qui retourne cette représentation sous forme de chaîne.

---

#### Utilité

De nombreux packages standards (comme `fmt`) vérifient si un type implémente l’interface `Stringer` pour déterminer comment afficher sa valeur.

---

#### Exemple : distances géographiques

Imaginons un programme qui gère des distances en kilomètres et en miles.
Types définis :

```go
type DistanceUnit int

const (
    Kilometer DistanceUnit = 0
    Mile      DistanceUnit = 1
)

type Distance struct {
    number float64
    unit   DistanceUnit
}
```

Sans méthode `String()`, `fmt` affichera ces types avec le **format par défaut** :

```go
mileUnit := Mile
fmt.Sprint(mileUnit) // "1"

dist := Distance{number: 790.7, unit: Kilometer}
fmt.Sprint(dist) // "{790.7 0}"
```

Pas très lisible !

---

#### Implémenter l’interface `Stringer`

Pour un affichage plus clair, on implémente `String()` pour les deux types :

```go
func (sc DistanceUnit) String() string {
    units := []string{"km", "mi"}
    return units[sc]
}

func (d Distance) String() string {
    return fmt.Sprintf("%.1f %v", d.number, d.unit)
}
```

---

#### Résultat

Les fonctions de `fmt` utilisent automatiquement les méthodes `String()` :

```go
kmUnit := Kilometer
fmt.Println(kmUnit) // "km"

mileUnit := Mile
fmt.Println(mileUnit) // "mi"

dist := Distance{
    number: 790.7,
    unit: Kilometer,
}
fmt.Println(dist) // "790.7 km"
```

