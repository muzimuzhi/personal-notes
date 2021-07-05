# Python


## Language

### Decorator
  * Glossary - [decorator][python-decorator]

 | Reference                                          | First joined version                 | Python Enhancement Proposals |
 | -------------------------------------------------- | ------------------------------------ | ---------------------------- |
 | [Function decorator][function-decorator-reference] | [2.4.1][function-decorator-whatsnew] | [PEP 318][pep-318]           |
 | [Class decorator][class-decorator-reference]       | [2.6][class-decorator-whatsnew]      | [PEP 3129][pep-3129]         |

[python-decorator]: https://docs.python.org/3/glossary.html#term-decorator
[function-decorator-reference]: https://docs.python.org/3/reference/compound_stmts.html#function-definitions
[class-decorator-reference]: https://docs.python.org/3/reference/compound_stmts.html#class-definitions
[function-decorator-whatsnew]: https://docs.python.org/3/whatsnew/2.4.html#pep-318-decorators-for-functions-and-methods
[class-decorator-whatsnew]: https://docs.python.org/3/whatsnew/2.6.html#pep-3129-class-decorators
[pep-318]: https://www.python.org/dev/peps/pep-0318/
[pep-3129]: https://www.python.org/dev/peps/pep-3129/


## Utility

### `pip`

Upgrade all outdated pip packages ([homepage](https://github.com/defjaf/pip_upgrade_outdated))
```bash
pip3 install pip-upgrade-outdated
pip_upgrade_outdated -3
```

Sync installed pip packages ([doc](https://pip.pypa.io/en/stable/reference/pip_freeze/))
```bash
# optional, in case "pip3" is still linked to the old version
brew unlink python@3.9 && brew link python@3.9
pip3 freeze [--path /usr/local/lib/python3.8/site-packages] > requirements.txt
pip3 install -r requirements.txt
```

### `pyenv`

* Install `pyenv` itself: `brew info pyenv`
* Set PATH: `PATH=$(pyenv root)/shims:$PATH` ([ref](https://opensource.com/article/20/4/pyenv))
* Install another version of Python: `PYTHON_BUILD_HOMEBREW_OPENSSL_FORMULA=openssl@1.0 pyenv install 3.4.4` ([ref](https://github.com/pyenv/pyenv/issues/950#issuecomment-624252667))
* Locally set Python version: `cd <path> && pyenv local 3.4.4`


## Standard Libraries

 - Full list: https://docs.python.org/3/library/index.html

### `subprocess`

 - Doc: https://docs.python.org/3/library/subprocess.html
```python
import subprocess

# run shell command `ls -l` and capture the output
result = subprocess.run(['ls', '-l'], capture_output=True)
result.stdout.decode('utf-8')
```

## Third-party Packages

* [`colorama`](https://github.com/tartley/colorama) - Print colored terminal text.
