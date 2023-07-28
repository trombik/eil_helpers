# Splitting a component

## Requirements

* Python
* git
* [git-filter-repo](https://github.com/newren/git-filter-repo)

```console
git clone git@github.com:trombik/esp-idf-lib eil_${component_name}
cd eil_${component_name}
# select the component path and examples for the component, replace "#\d+"
# with the original repo.
git-filter-repo --path components/${component_name} --path examples/${component_name} --message-callback 'return re.sub(br"\s#(\d+)", br" UncleRus/esp-idf-lib#\1", message)'
```
