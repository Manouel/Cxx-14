# C++14
### Nouveautés et fonctionnalités

- [make_unique](#make_unique)
- [Littéraux binaires](#binary_literals)
- [Séparateur de chiffres](#digit_separators)
- [deprecated](#deprecated)

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
