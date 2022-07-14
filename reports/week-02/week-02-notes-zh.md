# Week-02 工作笔记

> notes will be translated into English and inclueded in my weekly blog.

## 进一步了解 SirMorded

SirMorded 是调度组件。

根据文档介绍[^1]的运行命令是 `sirmordred -c mordred-simple.cfg`，这与 sirmordred 的开发环境运行模块的方式时不同的：

```python
micro.py --raw --enrich --cfg ./setup.cfg --backends git cocom
micro.py --panels --cfg ./setup.cfg
```

那么，这两种方式在实际效果上有哪些区别呢？

后来通过晔晖和我自己的代码阅读，发现 micro 是一个方便把不同阶段（比如`--raw --enrich --panels`）独立开来运行的调试运行。

实际上在使用 `poetry` 安装项目依赖以后，`sirmordred` 作为已经装入环境的软件包可以直接调用。

## 学习 `micro.py` 的参数列表

实际上就在这个文件内部用 `argparse` [^3]做了一些简单的 flag + 变量配置。并且因为设置了 `parser = argparse.ArgumentParser(add_help=False)` 所以默认情况下是展示不了的。

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

所以前面的几个 task 参数都是不用带值的 flag，标志一下某些任务是否执行。

但是很奇怪的是，默认应该存在的 `--help` 没有像官方文档里那样列出说明。（因为 `argparse.ArgumentParser(add_help=False)` 配置了无 help 参数，虽然我看不懂这是为什么）

Edit: 后来我决定开启一个 issue 经行讨论[^2]。


[^1]: [A GrimoireLab dashboard in one step](https://chaoss.github.io/grimoirelab-tutorial/sirmordred/dashboard/)
[^2]: [Turn on the help arg option in micro.py](https://github.com/chaoss/grimoirelab-sirmordred/issues/560)
[^3]: [Argparse Tutorial &#8212; Python 3.10.5 documentation](https://docs.python.org/3/howto/argparse.html)
