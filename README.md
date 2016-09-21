# Cxx-14
### Nouveautés et fonctionnalités

- [make_unique](#make_unique)
- [Littéraux binaires](#binary_literals)

---

#### make_unique <a id="make_unique"></a>

Tout comme les `shared_ptr`, les `unqiue_ptr` possèdent désormais également leur fonction de création `make_unique` qui permet d'éviter des fuites mémoires dans certains cas.

```cpp
std::shared_ptr<int> ptr = std::make_unique<int>(42);
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
