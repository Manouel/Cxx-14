# Cxx-14
### Nouveautés et fonctionnalités

- [make_unique](#make_unique)

---

#### make_unique <a id="make_unique"></a>

Tout comme les `shared_ptr`, les `unqiue_ptr` possèdent désormais également leur fonction de création `make_unique` qui permet d'éviter des fuites mémoires dans certains cas.

```cpp
std::shared_ptr<int> ptr = std::make_unique<int>(42);
```
