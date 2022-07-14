# Week-04 工作笔记

> notes will be translated into English and inclueded in my weekly blog.

## 安装 gitee data source 过程

### 第一阶段：尝试安装 gitee data source

这里我尝试安装了四个项目到我当前的虚拟环境中，但是我没有照着 gitee 的教程构建 grimoirelab （其中包括 `--install_venv /root/ivenv`）而是按照我在 sirmordred 中搭建好的虚拟环境。

安装依赖最好深入了解一下 pip install 命令：<https://pip.pypa.io/en/stable/cli/pip_install/#cmdoption-e>

此时安装的结果就是：

```log
Traceback (most recent call last):
  File "/root/miniconda3/envs/grimoirelab-sirmordred/bin/sirmordred", line 2, in <module>
    from sirmordred.bin.sirmordred import main
  File "/root/grimoirelab-sirmordred/sirmordred/bin/sirmordred.py", line 39, in <module>
    from sirmordred.config import Config
  File "/root/grimoirelab-sirmordred/sirmordred/config.py", line 30, in <module>
    from sirmordred.task import Task
  File "/root/grimoirelab-sirmordred/sirmordred/task.py", line 28, in <module>
    from grimoire_elk.elk import get_ocean_backend
  File "/root/miniconda3/envs/grimoirelab-sirmordred/lib/python3.7/site-packages/grimoire_elk/elk.py", line 27, in <module>
    from perceval.backend import find_signature_parameters, Archive
ModuleNotFoundError: No module named 'perceval.backend'
```

尝试修复 1： 把所有 Yehui 的仓库文件夹加入到 project structure 中。（无果，没有起作用）

尝试修复 2： 我试着手动下载了 `/root/sources/grimoirelab-perceval`, 命令是 `pip install .`

然后爆出了一系列版本不兼容的问题：

```log
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
sirmordred 0.3.1 requires grimoirelab-toolkit>=0.3, but you have grimoirelab-toolkit 0.2.0 which is incompatible.
pyopenssl 22.0.0 requires cryptography>=35.0, but you have cryptography 3.4.8 which is incompatible.
pygithub 1.55 requires pyjwt>=2.0, but you have pyjwt 1.7.1 which is incompatible.
perceval-weblate 0.2.1 requires grimoirelab-toolkit>=0.3, but you have grimoirelab-toolkit 0.2.0 which is incompatible.
perceval-weblate 0.2.1 requires perceval>=0.19, but you have perceval 0.17.4 which is incompatible.
perceval-puppet 0.2.1 requires grimoirelab-toolkit>=0.3, but you have grimoirelab-toolkit 0.2.0 which is incompatible.
perceval-puppet 0.2.1 requires perceval>=0.19, but you have perceval 0.17.4 which is incompatible.
perceval-opnfv 0.2.1 requires grimoirelab-toolkit>=0.3, but you have grimoirelab-toolkit 0.2.0 which is incompatible.
perceval-opnfv 0.2.1 requires perceval>=0.19, but you have perceval 0.17.4 which is incompatible.
perceval-mozilla 0.3.1 requires grimoirelab-toolkit>=0.3, but you have grimoirelab-toolkit 0.2.0 which is incompatible.
perceval-mozilla 0.3.1 requires perceval>=0.19, but you have perceval 0.17.4 which is incompatible.
grimoireelk 0.77.0 requires requests==2.26.0, but you have requests 2.28.0 which is incompatible.
grimoireelk 0.77.0 requires statsmodels<0.10.0,>=0.9.0, but you have statsmodels 0.13.2 which is incompatible.
grimoireelk 0.77.0 requires urllib3==1.26.5, but you have urllib3 1.26.9 which is incompatible.
grimoire-elk 0.102.0 requires grimoirelab-toolkit>=0.3, but you have grimoirelab-toolkit 0.2.0 which is incompatible.
grimoire-elk 0.102.0 requires perceval>=0.19, but you have perceval 0.17.4 which is incompatible.
grimoire-elk 0.102.0 requires requests==2.26.0, but you have requests 2.28.0 which is incompatible.
grimoire-elk 0.102.0 requires urllib3==1.26.5, but you have urllib3 1.26.9 which is incompatible.
graal 0.3.1 requires grimoirelab-toolkit>=0.3, but you have grimoirelab-toolkit 0.2.0 which is incompatible.
graal 0.3.1 requires perceval>=0.19, but you have perceval 0.17.4 which is incompatible.
cereslib 0.3.0 requires grimoirelab-toolkit<0.4.0,>=0.3.0, but you have grimoirelab-toolkit 0.2.0 which is incompatible.
grimoire-elk-gitee 0.1.0 requires requests==2.26.0, but you have requests 2.28.0 which is incompatible.
grimoire-elk-gitee 0.1.0 requires urllib3==1.26.5, but you have urllib3 1.26.9 which is incompatible.
```

这我就不懂为什么版本会有那么多不兼容。

尝试修复 3：`export PYTHONPATH=/root/miniconda3/envs/grimoirelab-sirmordred/lib/python3.7/site-packages`

总之这个方案也无效。

到这一步我发现最初的例子也运行不起来了，出现了大问题，所以我重新设置了虚拟环境，准备重新下载 python 依赖。

### 第二阶段：重装 venv

安装 requirement.txt 的时候，我发现爆出如下问题：

```log
ERROR: Could not find a version that satisfies the requirement grimoireelk (unavailable) (from versions: none)
ERROR: No matching distribution found for grimoireelk (unavailable)
```

其实问题就在于把 `grimoire-elk @ git+https://github.com/chaoss/grimoirelab-elk@master` 错写成了 `grimoireelk @ git+https://github.com/chaoss/grimoirelab-elk@master`

Edit：后来我也为此发起了一个帖子来讨论。[see this issue](https://github.com/chaoss/grimoirelab-sirmordred/issues/558)

重新安装的流程：

- Python interpreter 中先 add 一个虚拟环境，然后用虚拟环境作为解释器。
- 打开 terminal 输入 pip list 检查一下确定只有几个基础依赖。
- 分别运行 `pip install -r requirements.txt` 和 `pip install .`

这样做测试下来非常稳定，没什么问题。（删掉的办法也很简单，就是解释器先换成空，然后删掉当前目录的 venv 文件夹）

重整 conda 环境：

- [Delete Conda Environment (7 commands)](https://iq.opengenus.org/delete-conda-environment/)
- 关键步骤：`conda env remove -n corrupted_env`
- 找到 python interpreter 设置，add 一个 conda 的环境。
  - 注意，这里 conda 会默认创建一个环境在 root 目录下，但其实可以通过修改指定到 项目目录的 venv。
  - 就算在系统目录下也没问题，只是我觉得这样方便找到环境目录，符合我的习惯。
  - 参考 [Configure a Conda virtual environment | PyCharm](https://www.jetbrains.com/help/pycharm/conda-support-creating-conda-virtual-environment.html)

所以到这一步我算是回到了起点，虽然下载了文件但没有任何安装。

下面我再按照教程安装四个依赖，这次没有意外，全部正常替换和安装：

```log
# pip list

grimoire-elk        0.101.1     /root/repos/grimoirelab-elk
grimoire-elk-gitee  0.1.0       /root/repos/grimoirelab-elk-gitee
grimoirelab-panels  0.0.61      /root/repos/grimoirelab-sigils
perceval-gitee      0.1.0       /root/repos/grimoirelab-perceval-gitee
```

尝试 4 根据 <https://github.com/chaoss/grimoirelab-perceval/issues/791> 我们最新的讨论，要删除掉一些不必要的`__init__.py`文件。

之前我没有问题是因为依赖是直接从 git 仓库下载的最新版。而 Yehui 的下游仓库还是原来的版本，所以不太适配。前面遇到的 backend 的问题就是这样导致的。删掉对应的文件就可以修复。

### 第三阶段：观察报错

#### 报错 1：No such file or directory

下面运行 `sirmordred -c setup-gitee.cfg` 出现新报错：

```log
sirmordred -c setup-gitee.cfg
Traceback (most recent call last):
  File "/root/grimoirelab-sirmordred/venv/bin/sirmordred", line 5, in <module>
    main()
  File "/root/grimoirelab-sirmordred/sirmordred/bin/sirmordred.py", line 65, in main
    logger = setup_logs(logs_dir, debug_mode)
  File "/root/grimoirelab-sirmordred/sirmordred/bin/sirmordred.py", line 93, in setup_logs
    fh = logging.FileHandler(fh_filepath)
  File "/root/grimoirelab-sirmordred/venv/lib/python3.7/logging/__init__.py", line 1087, in __init__
    StreamHandler.__init__(self, self._open())
  File "/root/grimoirelab-sirmordred/venv/lib/python3.7/logging/__init__.py", line 1116, in _open
    return open(self.baseFilename, self.mode, encoding=self.encoding)
FileNotFoundError: [Errno 2] No such file or directory: '/tmp/logs/all.log'
```

看起来就是缺了 log 文件，我手动 touch 了一个，马上解决。

#### 报错 2：Cannot connect to Elasticsearch

```log
Can not access Elasticsearch service. Exiting sirmordred ...

日志文件 /tmp/logs/all.log 里面是这么说的：

2022-07-09 00:42:29,731 - sirmordred.sirmordred - ERROR - Cannot connect to Elasticsearch: http://localhost:9200
```

我怀疑是我的 docker 服务挂了，再看一眼。

docker 服务没有挂掉，测试 es 的方法：

```shell
curl https://localhost:9200/_search -k
```

我通过本地测试发现，其实是因为这个接口需要账号密码，并且 http 协议是不行的。

仔细一想，可能就是配置的问题，我检查了一下 micro 和 gitee 的 setup，发现确实配置有区别，复制了正确的 socket 问题解决。

#### 报错 3：too many 403 error responses

```log

2022-07-09 01:52:18,022 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 17455
2022-07-09 01:52:54,011 - grimoire_elk.elk - ERROR - Error feeding raw from gitee (https://gitee.com/mindspore/mindspore): HTTPSConnectionPool(host='gitee.com', port=443): Max retries exceeded with url: /api/v5/users/zhunaipan (Caused by ResponseError('too many 403 error responses'))
Traceback (most recent call last):
  File "/root/grimoirelab-sirmordred/venv/lib/python3.7/site-packages/requests/adapters.py", line 449, in send
    timeout=timeout
  File "/root/grimoirelab-sirmordred/venv/lib/python3.7/site-packages/urllib3/connectionpool.py", line 859, in urlopen
    **response_kw
  File "/root/grimoirelab-sirmordred/venv/lib/python3.7/site-packages/urllib3/connectionpool.py", line 859, in urlopen
    **response_kw
  File "/root/grimoirelab-sirmordred/venv/lib/python3.7/site-packages/urllib3/connectionpool.py", line 859, in urlopen
    **response_kw
  [Previous line repeated 2 more times]
  File "/root/grimoirelab-sirmordred/venv/lib/python3.7/site-packages/urllib3/connectionpool.py", line 836, in urlopen
    retries = retries.increment(method, url, response=response, _pool=self)
  File "/root/grimoirelab-sirmordred/venv/lib/python3.7/site-packages/urllib3/util/retry.py", line 574, in increment
    raise MaxRetryError(_pool, url, error or ResponseError(cause))
urllib3.exceptions.MaxRetryError: HTTPSConnectionPool(host='gitee.com', port=443): Max retries exceeded with url: /api/v5/users/zhunaipan (Caused by ResponseError('too many 403 error responses'))

During handling of the above exception, another exception occurred:

```

首先看最前面的报错：

> Error feeding raw from gitee (<https://gitee.com/mindspore/mindspore>):
> HTTPSConnectionPool(host='gitee.com', port=443):
> Max retries exceeded with url: /api/v5/users/zhunaipan (Caused by ResponseError('too many 403 error responses'))

HTTP 403 响应代码意味着禁止客户端访问有效的 URL。服务器获知请求，但由于客户端问题，无法满足请求。因素有很多这里看不出来。

但我尝试过 `curl https://gitee.com/mindspore/mindspore`，至少网络没有链接不通畅。

这里我实在没有想法了，所以我求助了 Yehui。得到了两个解决办法：

- 设置 `bulk_size`
- 配置 gitee api token
- 选择更小的仓库比如 `https://gitee.com/openeuler/docs`

这个问题也成功解决了。

#### 报错 4：Can't get some gitee PR commits

经过观察，获取草稿或者已关闭的 PR 会报出 ERROR。

下面的问题是如何解决 panels 没有更新的问题。

```log
2022-07-09 20:33:17,611 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 81
2022-07-09 20:33:18,315 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 73
2022-07-09 20:36:44,584 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 1214
2022-07-09 20:38:23,232 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 1314
2022-07-09 20:38:24,016 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 1326
2022-07-09 20:41:28,918 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 1478
2022-07-09 20:41:29,611 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 1479
2022-07-09 20:41:30,354 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 1480
2022-07-09 20:41:31,729 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 1482
2022-07-09 20:45:29,567 - perceval.backends.gitee.gitee - ERROR - Can't get gitee pull request commits with PR number 1698
Collection for gitee:pull: finished after 00:14:08 hours
```

这里我询问了 Yehui，我得知这是符合预期的结果。

## 检查 ElasticSearch 数据情况

`curl -k https://admin:admin@localhost:9200/_cat/indices?v`

```c++
health status index                   uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .kibana_2               uN3MqUCWRh-SNr8P3fZtaQ   1   0        374            0      471kb          471kb
yellow open   git-aoc_chaoss_enriched _uX8H_M-StWoUSj57KyUdA   5   1       6543         1348      6.9mb          6.9mb
yellow open   gitee_issues-raw        SVl0RySMTjyyxLWGZodJFQ   5   1       9910          431    226.4mb        226.4mb
yellow open   gitee_pulls-raw         IgSlcqzLSAuo6CXaQUPiTw   5   1        845            0     10.9mb         10.9mb
yellow open   gitee_repo-raw          PG1p6_o-SdGIuFRhf1yU7A   5   1          3            0     56.9kb         56.9kb
green  open   searchguard             iugDY0yLTHi7BNef5kxFfA   1   0          5            0     35.9kb         35.9kb
yellow open   stackexchange_enriched  JvCxCyEFRVqwAVGY9cgQtA   5   1       2432          548      5.3mb          5.3mb
yellow open   git_chaoss              20Fba_IoTNSSpL2UmulKhw   5   1       1765            2      2.8mb          2.8mb
yellow open   git_demo_raw            bYhIM2TCTgOFvNfb5Pe1ZQ   5   1      59000            0    106.9mb        106.9mb
yellow open   git-onion_enriched      UwZGn89GSdOURHzHKz1X8A   5   1        560            0    387.1kb        387.1kb
yellow open   stackexchange_raw       oHYpE3tJTmK2U0bPqoGGow   5   1       1056            5      7.2mb          7.2mb
yellow open   .grimoirelab-sigils     a1ceyVdqRky3AZcVCi-CWw   5   1         58            2    122.1kb        122.1kb
green  open   .kibana_1               B8gTj2VaRIubCVf_1dCJsg   1   0        374           59    557.7kb        557.7kb
yellow open   git_chaoss_enriched     7Kcm8qYyS72VHfIicPY7yg   5   1       1765           67        5mb            5mb
```

从上面的返回结果可以看出，gitee 的数据已经出现在我的 es index 中。
