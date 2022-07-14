# Coding Period 1 week-02

## What I have done

- create `grimoirelab-conversion-rate` GitHub organization.
- [How to run grimoirelab gitee? - grimoirelab-gitee/grimoirelab Wiki (github.com)](https://github.com/grimoirelab-gitee/grimoirelab/wiki/How-to-run-grimoirelab-gitee%3F)

## Problem I ran into

The Gitee setup process is different from the dev setup of SirMordered.

And the Gitee fork is about 50 commits behind the upstream.

I tried to install the package manually into the SirMordred and every attempt failed.

I think I need further dissuasion after the meeting.

## Learn more about SirMorded

According to the documentation [^1] the run command is `sirmordred -c mordred-simple.cfg`, which is different from the way sirmordred's development environment runs the module.

```python
micro.py --raw --enrich --cfg . /setup.cfg --backends git cocom
micro.py --panels --cfg . /setup.cfg
```

So, what are the differences between these two approaches in terms of actual results?

After reading through Yerhui and my own code, I found that micro is a debugging run that facilitates running different stages (like `--raw --enrich --panels`) independently.

In fact, after installing project dependencies with `poetry`, `sirmordred` can be called directly as a package already installed in the environment.

## Learn the parameter list of `micro.py`

It actually does some simple flag + variable configuration with `argparse` [^3] inside this file. And because `parser = argparse.ArgumentParser(add_help=False)` is set, it is not displayed by default.

```python
usage: micro.py [-h] [--raw] [--enrich] [--identities-load]
                [--identities-merge] [--panels] [--cfg CFG_PATH]
                [--backends [BACKEND_SECTIONS [BACKEND_SECTIONS ...]]]
                [--repos [REPOS_TO_CHECK [REPOS_TO_CHECK ...]]]
                [--logs-dir LOGS_DIR]

optional arguments:
  -h, --help            show this help message and exit
  --raw                 Activate raw task
  --enrich              Activate enrich task
  --identities-load     Activate load identities task
  --identities-merge    Activate merge identities task
  --panels              Activate panels task
  --cfg CFG_PATH        Configuration file path
  --backends [BACKEND_SECTIONS [BACKEND_SECTIONS ...]]
                        Backend sections to execute
  --repos [REPOS_TO_CHECK [REPOS_TO_CHECK ...]]
                        Limit which repositories are processed (list of URLs)
  --logs-dir LOGS_DIR   Logs Directory
```

> action：Note that we now specify a new keyword, `action`, and give it the value `"store_true"`. This means that, if the option is specified, assign the value `True` to `args.verbose`. Not specifying it implies `False`.[^3]

So the first few task arguments are not flagged with a value, which flags whether certain tasks are executed or not.

But it is strange that `--help`, which should be there by default, is not listed as it is in the official documentation. (because `argparse.ArgumentParser(add_help=False)` is configured with no help parameter, although I don't see why)

Edit: Later I decided to open an issue for discussion [^2].

[^1]: [A GrimoireLab dashboard in one step](https://chaoss.github.io/grimoirelab-tutorial/sirmordred/dashboard/)
[^2]: [Turn on the help arg option in micro.py](https://github.com/chaoss/grimoirelab-sirmordred/issues/560)
[^3]: [Argparse Tutorial &#8212; Python 3.10.5 documentation](https://docs.python.org/3/howto/argparse.html)
