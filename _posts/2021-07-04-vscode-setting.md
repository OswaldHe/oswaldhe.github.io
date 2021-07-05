---
layout: single
title:  "VSCode Linter Config for Python"
category: "python"
---

VSCode allows both explicit setting config file and implicit setting by typing "`command + ,`", which sometimes confusing (for example, which root is the implicit setting applied to or which setting should
we use to enable some functionality). Here is a guide for how to config VSCode for python syntax highlight and type checking.

### Import Path

If you are not working at the root directory (the directory that you opened in `open project` tab) or you are not using the python in your local machine (for example, in a virtual env using `pyenv`), Pylance sometimes complains about the missing imports for librarys that has been already installed. To help Pylance find the library, first ensure that the version of the python is the same as the one you are working with. You can change which python you are using in the bottom bar of VSCode and to check which python you are using, simply type:
```bash
which python
```
Then, add this setting to `./vscode/setting.json`:
```
"python.analysis.useImportHeuristic": true
```
It looks like gray that vscode doesn't really use it, but it would work as this config is embedded in VSCode, even though it does not exposed to the user. 

Now you can check whether you provide the correct import, but how about type checking. As we know, Python 3 provide explicit type annotation to provide a better experience of how to use the function
and potentially prevent misuse of variables (although the intepreter won't enforce type checking). An example of type hint looks like this:
```python
def test_sample(
    id: int = 0, # default
    name: str,
    isValid: bool,
    child: Child, # custom type also works
) -> Result : # return type
    pass
```
In order to perform a type checking in VSCode, we can use `mypy` extension in VSCode. You can download it from the VSCode store. After installation, install `mypy` using `pip` and set the `dmypy` executable using:
```
"mypy.dmypyExecutable": "/path/to/your/dmypy/exec"
```
as well as enable Pylance to lint your codes in setting:
- python.languageServer = Pylance
- python.linting = enabled

Though `mypy` is useful, it cannot check everything. For example, if you use `types-attrs` to create a new class, `mypy` cannot check the parameters.