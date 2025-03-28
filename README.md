# Sub python venv with mise

This repository aim to reproduce a misleading function of mise.

When creating a subfolder of [`venv`](https://docs.python.org/3/library/venv.html)
in another [`venv`](https://docs.python.org/3/library/venv.html) directory mise
only activate the first [`venv`](https://docs.python.org/3/library/venv.html)
and never the second one.

My version of mise:

```sh
$ mise version
              _                                        __
   ____ ___  (_)_______        ___  ____        ____  / /___ _________
  / __ `__ \/ / ___/ _ \______/ _ \/ __ \______/ __ \/ / __ `/ ___/ _ \
 / / / / / / (__  )  __/_____/  __/ / / /_____/ /_/ / / /_/ / /__/  __/
/_/ /_/ /_/_/____/\___/      \___/_/ /_/     / .___/_/\__,_/\___/\___/
                                            /_/
2025.3.2 linux-x64 (2025-03-07)
mise WARN  mise version 2025.3.10 available
mise WARN  To update, run mise self-update
```

## Reproducing

After cloning the projet please go into it and execute:

```sh
$ mise trust && mise install
```

From now you will see that the `.venv` folder that has been created is used:

```sh
$ which python
<absolute_path>/mise-sub-python-venv-example/.venv/bin/python
```

Now go to the subfolder and enable mise:

```sh
$ cd python && mise trust && mise install
```

The `.venv` folder is created but not used:

```sh
$ which python
<absolute_path>/mise-sub-python-venv-example/.venv/bin/python
```

Instead this should be `<absolute_path>/mise-sub-python-venv-example/python/.venv/bin/python`
if venv was correctly activated.

I don't know if this is a bug or if this is related directly with `mise` but
this is really annoying when using different python environment in complex 
repositories.

Note: I would like to say that the problem occur even if mise
[activate_aggressive](https://mise.jdx.dev/configuration/settings.html#activate_aggressive) is enabled.