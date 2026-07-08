# Zsh kernel for Jupyter

![](https://raw.githubusercontent.com/dahn-zk/zsh-jupyter-kernel/master/screenshots/example.png)

a simple Z Shell Jupyter kernel powered by Python, IPyKernel, Pexpect,
and enthusiasm — turn your scripts into notebooks!

## installation

install the python package from [pypi](https://pypi.org/project/zsh-jupyter-kernel/) using available package manager like `pip`, `pipenv` or `conda`.

(optional) by default the installation script will install the package *and* the kernel. the kernel location will be the same as the python environment from which the installation is done. check `python -m zsh_jupyter_kernel.install -h` for possible installation options if you want to install the kernel in a different environment or to change display name.

### pipenv

```sh
pip install zsh_jupyter_kernel
```

### install kernel file

see the help command for details
```sh
python -m zsh_jupyter_kernel.install --help
```

### conda

```sh
conda create -n zsh-jupyter-kernel
conda activate zsh-jupyter-kernel
conda install -c conda-forge zsh-jupyter-kernel
python -m zsh_jupyter_kernel.install --sys-prefix
```

## usage

expect the kernel to work as usual including your zsh configs *except without stdin (user input) support and prompt line* and in the boundaries of the current jupyter limitations like not all ansi codes will work in certain cases ('\r') and in general things that expect terminal will have unexpected behavior.
see below why user input is not supported.

## technical overview

the kernel launches zsh as if it was a regular process launched from your terminal with a few minor settings to make sure it works with jupyter. there is slight chance it wont work with super complicated zshrc setups, but it works with majority of configs including oh-my-zsh.

### how does code execution work

the kernel configures zsh prompt string to its own custom value.
when a user requests a cell execution, the code is sent to the kernel.
then the kernel puts the frontend on hold, sends the code to zsh process, and waits for the prompt string to release the frontend and let the user request more code execution.

### code completion

code completion is powered by quite a non-trivial script that involves multiple steps, including spawning another temporary zsh process and capturing completion options into a data structure that jupyter frontend understands.

### code inspection

```sh
pip install uv
uv sync
uv run python -m zsh_jupyter_kernel.install --sys-prefix
uv run pytest
```
