# Coding Period 1 week-04

## Installing gitee data source process

### Phase 1: Trying to install gitee data source

Here I tried to install four projects into my current virtual environment, but instead of following the gitee tutorial for building grimoirelab (which includes `-install_venv /root/ivenv`), I followed the virtual environment I built in sirmordred.

Installing the dependencies is best done with a deep dive into the pip install command: <https://pip.pypa.io/en/stable/cli/pip_install/#cmdoption-e>

The result of this installation is:

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

Try fix 1: Add all Yehui's repository folders to the project structure. (No luck, it didn't work)

Try fix 2: I tried to manually download `/root/sources/grimoirelab-perceval`, the command is `pip install .`

A series of version incompatibilities were then reported.

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

I don't understand why there are so many incompatibilities in the versions.

Try fix 3: `export PYTHONPATH=/root/miniconda3/envs/grimoirelab-sirmordred/lib/python3.7/site-packages`

Anyway, this solution is also invalid.

At this point I realized that the initial example wasn't working either, and there was a big problem, so I reset the virtual environment and prepared to download the python dependencies again.

### Phase 2: reinstall venv

When installing requirement.txt, I found the following problem popping up.

```log
ERROR: Could not find a version that satisfies the requirement grimoireelk (unavailable) (from versions: none)
ERROR: No matching distribution found for grimoireelk (unavailable)
```

The problem is that `grimoire-elk @ git+https://github.com/chaoss/grimoirelab-elk@master` is misspelled as `grimoireelk @ git+https://github.com/chaoss/ grimoirelab-elk@master`

Edit: I later started a issue to discuss this as well. [see this issue](https://github.com/chaoss/grimoirelab-sirmordred/issues/558)

The reinstallation process:

- Add a virtual environment to the Python interpreter, and then use the virtual environment as the interpreter.
- Open terminal and type pip list to check that there are only a few base dependencies.
- Run `pip install -r requirements.txt` and `pip install .`

This is very stable and works without any problems. (It's also easy to remove the interpreter by replacing it with an empty one and then deleting the venv folder in the current directory)

So at this point I'm back to start, having downloaded the files but not installing anything.

I followed the tutorial to install four more dependencies, and this time there were no surprises, all replaced and installed normally:

```log
# pip list

grimoire-elk        0.101.1     /root/repos/grimoirelab-elk
grimoire-elk-gitee  0.1.0       /root/repos/grimoirelab-elk-gitee
grimoirelab-panels  0.0.61      /root/repos/grimoirelab-sigils
perceval-gitee      0.1.0       /root/repos/grimoirelab-perceval-gitee
```

Try to fix 4: According to <https://github.com/chaoss/grimoirelab-perceval/issues/791> our latest discussion, remove some unnecessary `__init__.py` files.

I had no problem before because the dependencies were downloaded directly from the latest version of the git repository. Yehui's downstream repository is still the same version, so it doesn't fit well. The backend problem I encountered earlier was the result of this. You can fix it by deleting the corresponding file.

### Phase 3: Watch for running errors

#### Error 1: No such file or directory

Running `sirmordred -c setup-gitee.cfg` below gives a new error.

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

It looked like the log file was missing, so I touched one manually and it was fixed immediately.

#### Error 2: Unable to connect to Elasticsearch

```log
Can not access Elasticsearch service. Exiting sirmordred ...

in `/tmp/logs/all.log` ：

2022-07-09 00:42:29,731 - sirmordred.sirmordred - ERROR - Cannot connect to Elasticsearch: http://localhost:9200
```

I suspect that my docker service is hanging, take another look.

The docker service didn't hang, and the way to test es is.

```shell
curl https://localhost:9200/_search -k
```

I found out through local testing that the interface actually requires an account password, and that the http protocol is not working.

I checked the setup of micro and gitee, and found that there is indeed a difference in the configuration, and copied the correct socket.

#### Error 3: too many 403 error responses

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

First look at the top error report.

> Error feeding raw from gitee (<https://gitee.com/mindspore/mindspore>):
> HTTPSConnectionPool(host='gitee.com', port=443):
> Max retries exceeded with url: /api/v5/users/zhunaipan (Caused by ResponseError('too many 403 error responses'))

The HTTP 403 response code means that the client is prohibited from accessing a valid URL, and the server is aware of the request, but cannot fulfill it due to client issues. There are many factors that are not visible here.

But I tried `curl https://gitee.com/mindspore/mindspore` and at least the network didn't break.

Here I really ran out of ideas, so I asked Yehui for help and got these solutions.

- set `bulk_size`
- Configure gitee api token
- Choose a smaller repository like `https://gitee.com/openeuler/docs`

This problem was also solved successfully.

#### Error 4: Can't get some gitee PR commits

After observing this, getting draft or closed PRs will report an ERROR.

The following question is how to solve the problem of panels not being updated.

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

Here I asked Yehui, and I was told that this was the expected result.

## Check ElasticSearch data status

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

As you can see from the results returned above, the gitee data is already in my es index.
