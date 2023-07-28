# Splitting a component

## Requirements

* Python
* git
* [git-filter-repo](https://github.com/newren/git-filter-repo)

Create a repository from the original, select the component, and rewrite the
commit history.

```sh
git clone git@github.com:UncleRus/esp-idf-lib eil_${component_name}
cd eil_${component_name}
# select the component path and examples for the component, replace "#\d+"
# with the original repo.
git-filter-repo --path components/${component_name} --path examples/${component_name} --message-callback 'return re.sub(br"\s#(\d+)", br" UncleRus/esp-idf-lib#\1", message)'
```

Copy the following files from *this* repository to new component repository.
They are supposed to work without modifications.

* `.gitignore`
* `.github`

Create `idf_component.yml` under `components/${component_name}`. Remember to
modify the content.

```yaml
---

version: "0.0.1"
description: Common support library for esp-idf-lib
url: https://esp-idf-lib.readthedocs.io/en/latest/
issues: https://github.com/trombik/eil_helpers/issues
repository: https://github.com/trombik/eil_helpers
dependencies:
  idf: ">=4.3"
```

Add `README.md` to `components/${component_name}`.

## Resources

* [ESP Registry](https://components.espressif.com/)
* [IDF Component Manager](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/tools/idf-component-manager.html)
* [Packaging ESP-IDF Components](https://espressif-docs.readthedocs-hosted.com/projects/idf-component-manager/en/latest/guides/packaging_components.html)
