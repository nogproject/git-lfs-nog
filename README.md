# Git LFS Nog -- README
By spr
<!--@@VERSIONINC@@-->

# Introduction

Git LFS Nog contains supporting tools to share workspaces between researchers
at ZIB and external partners.

`git-lfs-nog` is a Git extension to manage Git LFS data with Nog.

# Windows

Use Ubuntu on Windows.  Do not use MSYS Git.

# Getting started

Download Git LFS as described on <https://git-lfs.github.com> and install it
with the option to skip automatic download:

```
git lfs install --skip-smudge --skip-repo
```

Your `~/.gitconfig` should contain the following section:

```
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge --skip -- %f
    process = git-lfs filter-process --skip
    required = true
```

Clone Git LFS Nog and apply the steps below in the working copy:

```bash
cd git-lfs-nog
```

Initialize a Python 3 virtual environment that will be exclusively used for Git
LFS Nog in the sub-directory `local/venv` of the working copy:

On Linux, use a specific directory for the Linux release:

```bash
venvdir="venv-$(lsb_release -i -s)-$(lsb_release -r -s)" && echo "${venvdir}"
```

Otherwise use generic directory:

```bash
venvdir=venv
```

```bash
python3 -m venv local/${venvdir}
 # or
virtualenv -p python3 local/${venvdir}
```

```bash
./local/${venvdir}/bin/pip3 install -r requirements.txt
```

Configure your shell to use `git-lfs-nog`.  To do so, either add
`git-lfs-nog/bin` to your `PATH`, or if `${HOME}/bin` is in your path, create
a symlink there:

```bash
ln -s "$(pwd)/bin/git-lfs-nog" ~/bin
```

Configure the environment variables for Nog access.  Create an API key at
<https://nog.zib.de/settings#apikeys>, create a nogcache path, and configure
your shell as described at
<https://nog.zib.de/nog/doc/files/tutorial-nogpy.md>.

Confirm that Git LFS Nog can fetch content:

```bash
$ git lfs-nog -h
Usage:
  git-lfs-nog
  ...

$ cat test/tracktest.dat
version https://git-lfs.github.com/spec/v1
oid sha256:...
size ...

$ git lfs ls-files
...
2ef1e3e810 - test/tracktest.dat

$ git lfs-nog init
$ git lfs-nog fetch -- .
...
lfs-nog: Fetched `test/tracktest.dat`.

$ cat test/tracktest.dat
This is the file content ...

$ git lfs ls-files
...
2ef1e3e810 * test/tracktest.dat
```
