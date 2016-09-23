# C++14
### Nouveautés et fonctionnalités

- [make_unique](#make_unique)
- [Littéraux binaires](#binary_literals)
- [Séparateur de chiffres](#digit_separators)
- [deprecated](#deprecated)
- [Lambdas génériques](#generic_lambdas)
- [Déduction des types de retour](#return_type_deduction)

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
