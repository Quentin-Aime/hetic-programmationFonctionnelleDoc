# INTRODUCTION À LA PROGRAMMATION FONCTIONNELLE
*https://flaviocopes.com/javascript-functional-programming/#introduction-to-functional-programming*
> I do not own anything that is said below, this is merely just a translation from this article.

La programmation fonctionnelle est un paradigme de programmation doté de quelques techniques particulières.

Dans les langages de programmation, vous trouverez des langages de programmation purement fonctionnels ainsi que des langages de programmation qui supportent les techniques de programmation fonctionnelle.

Parmi les langages de programmation purement fonctionnels les plus connues ont retrouve l' Haskell, le Clojure ainsi que Scala.

Et parmi les plus populaires qui supportent les technique de programmation fonctionnelles, on croise le JavaScript, le Python, le Ruby ainsi que bien d'autres.

La programmation fonctionnelle n'est pas un nouveau concept, il remonte en fait aux années 30, à la naissance du lambda-calcul, et à influencer de nombreux autres langages de programmation.

Ces derniers temps, elle est en train de revenir au goût du jour. C'est donc le moment idéal d'en apprendre davantage à ce sujet.

À travers ce document, nous allons nous intéresser aux principaux aspects de la programmation fonctionnelle, en utilisant différents exemples en JavaScript.

# LES FONCTIONS

Dans un langage de programmation fonctionnelle, celles-ci sont des citoyens de première classe.

## ELLES PEUVENT ÊTRE ASSIGNÉES À DES VARIABLES

```javascript=
const f = (m) => console.log(m)
f('Test')
```

Puisqu'elles peuvent être assignées à une variable, elle peuvent donc être ajoutées à des objets.

```javascript=
const obj = {
  f(m) {
    console.log(m)
  }
}
obj.f('Test')
```

mais également à des tableaux

```javascript=
const a = [
  m => console.log(m)
]
a[0]('Test')
```

## ELLES PEUVENT ÊTRE UTILISÉES EN TANT QU'ARGUMENT POUR D'AUTRES FONCTIONS

```javascript=
const f = (m) => () => console.log(m)
const f2 = (f3) => f3()
f2(f('Test'))
```

## ELLES PEUVENT ÊTRE LA VALEUR DE RETOUR D'AUTRES FONCTIONS

```javascript=
const createF = () => {
  return (m) => console.log(m)
}
const f = createF()
f('Test')
```

# LES FONCTIONS D'ORDRE SUPÉRIEUR

Le terme anglaisétant trop important, car retrouvé dans de nombreuses documentation, je me permets de le repréciser ici :
`HIGHER ORDER FUNCTIONS`

Il s'agit des fonctions qui acceptent ou retournent d'autres fonctions.

Par exemple, dans la librairie standard de JavaScript:
`Array.map()`, `Array.filter()`, et `Array.reduce()`

# PROGRAMMATION DÉCLARATIVE

Vous pourriez avoir entendu parler de "programmation déclarative".
Pour une question de contexte, la "programmation imperative" est l'opposé de cela.

## DÉCLARATIVE VS IMPÉRATIVE

Une approche impérative consiste à indiquer à la machine les différentes étapes à suivre pour effectuer un travail.

À l'inverse, une approche déclarative consiste à indiquer à la machine le résultat attendu, et de la laisser comprendre d'elle-même les différents détails.

Vous commencez à réfléchir à cette dernière lorque vous avez suffisamment de niveaux d'abstraction pour arrêter de penser à des constructions de bas niveaux, et que vous vous concentrez davantage sur des aspects plus élevé de l'UI.

Certains peuvent affirmer que le C est plus déclaratif que l'assembleur, et serait juste.

L'HTML est déclaratif, si vous l'avez donc utilisé depuis 1995, vous avez en fait construits des UI déclaratives depuis plus de 20 ans.

Le JavaScript peut prendre ces 2 approches.

Par exemple une approche déclarative serait d'éviter d'utiliser des `loops`, mais de passer plutôt par des `map`, `reduce` et `filter`. Ceux-ci étant plus abstraits et se concentrent moins sur les différentes étapes à fournir à la machine.

# L'IMMUTABILITÉ

En programmation fonctionnelle la donnée ne change jamais. Elle est immutable.

Une variable ne peut jamais être changée. Pour la modifier, il faut créer une nouvelle variable.

Plutôt que de changer un tableau de valeur, pour y ajouter une nouvelle donnée, il faut créer un nouveau tableau en concaténant l'ancien ainsi que la nouvelle valeur.

Les objets quant à eux ne sont jamais directement modifiés non plus, mais plutôt copié avant d'avoir leur donnée(s) changée(s).

## CONST

C'est la raison pour laquelle en le mot clef `const` apporté par l'ES6 est autant utilisé en JavaScript, afin de **forcer** l'immutabilité des variables.

## OBJECT.ASSIGN()

L'ES6 nous a également fourni la méthode clef `Object.assign()` pour créer des objets

```javascript=
const redObj = { color: 'red' }
const yellowObj = Object.assign({}, redObj, {color: 'yellow'})
```

## CONCAT()

Pour ajouter des valeurs à un tableau en JavaScript on utilise généralement la méthode `push()` sur ce tableau. Cependant cette méthode apporte une mutation au tableau original, ce n'est donc pas orienté programmation fonctionnelle.

À la place il faut utiliser la méhode `concat()`

```javascript=
const a = [1, 2]
const b = [1, 2].concat(3)
// b = [1, 2, 3]
```

ou bien le **spread operator**

```javascript=
const c = [...a, 3]
// c = [1, 2, 3]
```

## FILTER()

Il en va de même si l'on veut retirer une valeur d'un tableau. Au lieu de `pop()`et `slice()`, qui modifie le tableau original, il faut utiliser `array.filter()`

```javascript=
const d = a.filter((v, k) => k < 1)
// d = [1]
```

# LES FONCTIONS PURES

Celles-ci :
- ne change jamais aucun des paramètres passés en **référence** (en JS, les tableaux et objets): ceux-ci doivent être considérés comme immutable. Elle peuvent cependant changer les paramètres copiés par valeur
- possède une valeur de retour non influencée par autre chose que ces paramètres. Les mêmes paramètres auront toujours le même résultat
- ne change jamais quelque chose situé en dehors de son scope, durant son execution

# LA TRANSFORMATION DE LA DONNÉE

Puisque l'immutabilité est un concept si important et l'une des fondations de la programmation fonctionnelle, vous vous demandez sûrement comment modifier vos données.

C'est simple: **la donnée est modifié en créant des copies de celle-ci**.

Les fonctions, en particulier, changent la donnée en en retournant des copies.

Parmi celles-ci on retrouve notamment **map** et **reduce**.

## ARRAY.MAP()

Appeler `Array.map()` sur un tableau va en créer un nouveau avec le résultat d'une fonction exécutée sur chacune des valeurs du tableau original

```javascript=
const a = [1, 2, 3]
const b = a.map((v, k) => v * k)
// b = [0, 2, 6]
```

## ARRAY.REDUCE()

Appeler `Array.reduce()` sur un tableau nous permet de transformer celui-ci en n'importe quel autre entité, incluant les structures de données, les fonctions, les booléens, et les objets.

Pour cela, vous avez besoin d'une fonction qui calcule le résultat, ainsi qu'un point de départ.

```javascript=
const a = [1, 2, 3]
const sum = a.reduce((partial, v) => partial + v, 0)
// sum = 6
```

```javascript=
const o = a.reduce((obj, k) => { obj[k] = k; return obj }, {})
// o = {1: 1, 2: 2, 3: 3}
```

# LA RÉCURSIVITÉ

La récursivité est une fonctionnalité clef de la programmation fonctionnelle. Quand une fonction s'appelle elle-même, il s'agit de fonction récursive.

La suite de Fibonacci est l'exemple classique de la récursivité : n = (n-1 + n-2).
Ici avec sa solution totalement inneficace, mais bien à lire, pour 2^n

```javascript=
var f = (n) => n <= 1 ? 1 : f(n-1) + f(n-2)
```

# COMPOSITION

La composition est une autre fonctionnalité clef de la programmation fonctionnelle.

**La composition est la manière de générer une fonction d'ordre supérieur, en combinant des fonctions plus simple**

## COMPOSER EN JS

Un des moyens les plus répandus pour composer des fonctions en simple JavaScript est de les chaîner entre elles:

```javascript=
obj.doSomething()
   .doSomethingElse()
```

On peut également passer une fonction dans une autre fonction:

```javascript=
obj.doSomething(doThis())
```

## COMPOSER À L'AIDE DE `LODASH`

Plus généralement, 'composer' consiste à rassembler plusieurs fonctions entre elles afin d'exécuter une opération plus compliquée.

`lodash/fp` est livré avec une implémentation de `compose`: on exécute une liste de fonctions, en y passant en premier notre argument à modifier, puis **chaque fonction hérite de l'argument retourné par la fonction précédentes**. Vous noterez que l'on a pas besoin de stocker de valeurs intermédiaires.

```javascript=
import { compose } from 'lodash/fp'

const slugify = compose(
  encodeURIComponent,
  join('-'),
  map(toLowerCase),
  split(' ')
)

slufigy('Hello World') // hello-world
```


