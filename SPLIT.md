# Splitting a component

## Requirements

* Python
* git
* [git-filter-repo](https://github.com/newren/git-filter-repo)

## Splitting

### Creating new component repository

Create a repository from the original, select the component, and rewrite the
commit history.

```sh
# do not skip this! you MUST clone the repository.
git clone git@github.com:UncleRus/esp-idf-lib eil_${component_name}
cd eil_${component_name}
# select the component path and examples for the component, replace "#\d+"
# with the original repo.
git-filter-repo --path components/${component_name} --path examples/${component_name} --message-callback 'return re.sub(br"\s#(\d+)", br" UncleRus/esp-idf-lib#\1", message)'
```

Copy the following files from *this* repository to new component repository.
They are supposed to work without modifications.

* `.gitignore`

Copy the following files from *this* repository to new component repository.

* `.github`

Modify `publish_esp_component_registory.yml`. `name` in job `Upload component
to the registry` must be modified. Add more jobs if the repository has more
than one component.

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

### Creating an account at ESP Registry

Create an account of ESP Registry. The repository is not open to public yet.
Ask espressif employees at
[Issue #4](https://github.com/espressif/idf-component-manager/issues/4). You
need to tell them that you need a name space of your own, and `esp-idf-lib`.

### Creating an API token

Create a token at [ESP Registry](https://components.espressif.com/). Login to
the registry, and click your profile icon, select [Tokens]. The [Description]
should be something like `github-action-publish`. Select `write:components` in
[Scope of the token]. Set [Expiration]. Click [Create]. Copy the token value,
which you will need later.

Add GitHub Actions Secret. Visit [Settings] -> [Secrets and variables] under
[Security]. Select [Actions]. Add `IDF_COMPONENT_API_TOKEN` by clicking [New
repository secret]. Use the API token from ESP Registry.

Add `README.md` to `components/${component_name}`.

## Resources

* [ESP Registry](https://components.espressif.com/)
* [IDF Component Manager](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/tools/idf-component-manager.html)
* [Packaging ESP-IDF Components](https://espressif-docs.readthedocs-hosted.com/projects/idf-component-manager/en/latest/guides/packaging_components.html)
