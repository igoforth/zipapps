
# Changelogs

- 2021.04.24
  - remove conflict command args with `pip`
    - remove `--compile` arg, it will only work for `pip`
    - `-c` will not be removed, `pip install -c` could use `pip install --constraint` instead
  - Refactor for OOP(Object-oriented programming)
    - Will be used as the **first stable version** and end experimental phase
    - remove `Config` class
    - show version info and other logs in stderr
    - `compiled` should not be True while `unzip` is null
    - a successful message has been added to the tail of logs: `Successfully built`
- 2021.04.23
  - lazy install mode:
    - fix bug: python version conflict
    - remove repeated installation
      - `pip install` will execute only once for same `pip_args_md5`
      - `pip_args_md5` comes from pip_args string, including bytes of related files(like `requirements.txt`)
  - logs of `pip install` will be redirected, from `stdout` to `stderr`
- 2021.04.22
  - update the lazy_install mode (`-d`)
    - simple use case:
      - 1. `python -m zipapps -d six`
      - 2. `python app.pyz -c "import six;print(six.__file__)"`
    - it will not `rmtree` the `_zipapps_lazy_pip` folder for new build
      - if you need to upgrade those requirements, use `-U` arg of pip
      - lazy_install mode will install requirements with `pip install` while first running
        - so you can just run like this to init its installation(`-m` not set): `python3 app.pyz -V`
    - for now, `-d` is to server for the cross-platform and cross-python-version app
      - pip target path is separated from different python version and system platform
        - python version accurity can be reset with `-pva 5`, defaults to 2, means 3.8.3 and 3.8.4 are same `3.8`
- 2021.04.11
  - add `--sys-paths` to include new paths of sys.path
    - support TEMP/HOME/SELF prefix, separated by commas
    - sometimes be used for separating pyz code from requirements
      - requirements often be installed by `pip install xxx -t $SOME_PATH`
- 2021.04.01
  - use `ensurepip` instead of install `pip` while running with `lazy-install`
  - `unzip_path` has been set to `SELF/zipapps_cache` by default when `lazy_install` is `True`
- 2021.03.31
  - use `sys.stderr.write` instead of `warnings.warn`
  - support `-d` for lazy pip install
    - example: `python3 -m zipapps -d bottle -r requirements.txt`
    - the `bottle` and other packages of `requirements.txt` will be install while first running
    - this is useful for cross-platform distributions, which means `pyz` file size is much smaller
- 2021.03.02
  - fix auto-unzip nonsense folders while `-u AUTO` but no need to unzip any files
- 2021.01.29
  - fix packaging zipapps self
    - `python3 -m zipapps -m zipapps.__main__:main -a zipapps -o zipapps.pyz`
  - add `zipapps.pyz` to `release` page
- 2021.01.28
  - fix bug: run `.py` file with run_path missing sys.argv
    - `python3 app.pyz xxx.py -a -b -c`
- 2021.01.11
  - add `--zipapps` arg while building pyz files
    - to activate some venv pyz with given paths while running it
    - also support `TEMP/HOME/SELF` prefix, these internal variables are still runtime args.
- 2020.12.27
  - Combile multiple `pyz` files, do like this:
    - python3 -m zipapps -o six.pyz six
    - python3 -m zipapps -o psutil.pyz -u AUTO psutil
    - python3 six.pyz --zipapps=psutil.pyz -c "import six,psutil;print(six.__file__, psutil.__file__)"
- 2020.12.23
  - `--unzip` support **auto-check** by `-u AUTO`, alias for `--unzip=AUTO_UNZIP`
  - fix `run_module` bug while running `./app.pyz -m module`
- 2020.12.21
  - now will not run a new subprocess in most cases.
    - using `runpy.run_path` and `runpy.run_module`
    - and using `subprocess.run` instead of `subprocess.call`
- 2020.12.13
  - `--unzip` support complete path
  - `--unzip` support **auto-check** by `--unzip=AUTO_UNZIP`
- 2020.11.23
  - add `activate_zipapps` to activate zipapps `PYTHONPATH` easily
- 2020.11.21
  - reset unzip_path as the parent folder to unzip files
    - so the cache path will be like `./zipapps_cache/app/` for `app.pyz`,
    - this is different from old versions.
  - add environment variable `ZIPAPPS_CACHE` for arg `unzip_path`
  - add environment variable `ZIPAPPS_UNZIP` for arg `unzip`