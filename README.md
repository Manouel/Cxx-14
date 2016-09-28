# C++14
### Nouveautés et fonctionnalités

- [make_unique](#make_unique)
- [Littéraux binaires](#binary_literals)
- [Séparateur de chiffres](#digit_separators)
- [deprecated](#deprecated)
- [Lambdas génériques](#generic_lambdas)
- [Déduction des types de retour](#return_type_deduction)
- [Capture étendue dans les lambdas](#lambda_capture)
- [Accès aux tuples par type](#tuple_addressing)

---

#### make_unique <a id="make_unique"></a>

Tout comme les `shared_ptr`, les `unqiue_ptr` possèdent désormais également leur fonction de création `make_unique` qui permet d'éviter des fuites mémoires dans certains cas.

```cpp
std::unique_ptr<int> ptr = std::make_unique<int>(42);
```

---

#### Littéraux binaires <a id="binary_literals"></a>

Le C++14 introduit une notation pour les littéraux binaires, comme il en existait pour les bases octales ou hexadécimales. Pour cela, il suffit d'utiliser le préfixe `0b` ou `0B`.

```cpp
int a = 034;      // Octal
int b = 0xC3A;    // Hexadécimal

int n = 0b11;     // Binaire

std::cout << n << std::endl;    // Affiche 3
```

---

#### Séparateur de chiffres <a id="digit_separators"></a>

Pour faciliter la lecture des nombres, il est maintenant possible d'utiliser des apostrophes pour séparer les chiffres. Celles-ci peuvent être placées n'importe où sur des entiers ou des nombres à virgule de n'importe quelle base.

```cpp
const int billion = 1'000'000'000;
const double pi   = 3.141'592'653'59;
const int n       = 0b0101'0111'1010;
```

---

#### deprecated <a id="deprecated"></a>

Il existe un nouvel attribut `[[deprecated]]` permettant de déprécier des fonctionnalités. Il est possible d'y associer ou non un message.

```cpp
[[deprecated]]
void foo() {}

[[deprecated("Cette fonction est dépréciée")]]
void bar() {}
```

Il permet de générer à la compilation un avertissement lorsque ces fonctionnalités sont utilisées dans le code.

---

#### Lambdas génériques <a id="generic_lambdas"></a>

En C++11, les paramètres des fonctions lambdas devaient tous êtres spécifiés explicitement avec des types concrets. Désormais, il est possible d'utiliser l'inférence de type avec `auto`.

```cpp
auto lambda = [](auto a, auto b) { return a + b; }
```

---

#### Déduction des types de retour <a id="return_type_deduction"></a>

En C++11, la déduction du type de retour d'une fonction n'était autorisée que sur les lambdas. Pour les autres fonctions, il fallait compléter le retour `auto` par une spécification de type comme dans les exemples suivants.

```cpp
auto f() -> int {}                // int
auto g() -> decltype(1) {}        // int
auto h() -> decltype(foo()) {}    // type de retour de foo()
```

Désormais, il est possible d'utiliser la déduction de type de retour sur toutes les fonctions en utilisant uniquement `auto`. 

```cpp
auto square(int n) 
{
    return n * n;
}
```

Cependant, si plusieurs expressions de retour sont utilisées dans la fonction, ils doivent tous être du même type. De plus, comme la déduction se fait à partir de ou des expressions de retour, la fonction ne peut pas être utilisée avant d'avoir été définie. Sa signature ne suffit pas au compilateur lors de l'utilisation, et la définition doit être disponible.
De plus, dans le cas des fonctions récursives, l'appel récursif doit se trouver après au moins un retour dans le corps de la fonction, auquel cas la déduction provoquera une erreur.

```cpp
auto foo(int i)
{
  if (i == 1)
    return i;
  else
    return foo(i-1) + i;    // Ok, déduction de type int faite au dessus
}

auto bar(int i)
{
  if (i != 1)
    return bar(i-1) + i;    // Erreur, déduction impossible
  else
    return i;
}
```

---

#### Capture étendue dans les lambdas <a id="lambda_capture"></a>

Les fonctions lambdas introduites en C++11 permettent de capturer des variables, par copie ou référence.

```cpp
int a = 1;

// Capture par copie
auto lambda = [a](int b) { return a + b; };

// Capture par référence
auto l2 = [&a](int n) {
    if (n > a)
        a = n;
};

a = 2;
std::cout << lambda(3) << std::endl;    // Affiche 4
```

A noter que ici, la valeur est capturée lors de la création de la lambda et ne change pas ensuite.

Ici, il n'est donc pas possible de passer directement une valeur dans la capture, il faut au préalable créer une variable. De plus, il n'est pas possible non plus d'utiliser la sémantique de mouvement, et donc de passer des objets non copiables comme `unique_ptr`.

En C++14, les variables capturées n'ont pas besoin d'être créées avant, et leur type est déduit par le compilateur.

```cpp
auto lambda = [a = 1](int b) { return a + b; };


// Création d'un weak_ptr directement dans la lambda
std::shared_ptr<int> sr = std::make_shared<int>(3);
auto l2 = [wr = std::weak_ptr<int>(sr)]() { foo(wr); };


auto timer = [val = system_clock::now()] { return system_clock::now() - val; };
timer();   // Temps depuis la création du timer
```

Il est donc maintenant possible de passer des variables par déplacement.

```cpp
auto p = std::make_unique<int>(10);

auto lambda = [p = std::move(p)] { return *p; }
```

---

#### Accès aux tuples par type <a id="tuple_addressing"></a>

En C++11, il est possible d'accéder aux éléments d'un tuple via leur index. C++14 introduit l'accès par type. S'il existe plus d'un élément du type demandé, cela provoque une erreur de compilation.

```cpp
std::tuple<std::string, std::string, int> t("foo", "bar", 7);

// C++11
std::cout << std::get<2>(t) << std::endl;          // 7
// C++14
std::cout << std::get<int>(t) << std::endl;        // 7

std::cout << std::get<std::string>(t) << std::endl;  // Erreur de compilation : ambiguité
```
