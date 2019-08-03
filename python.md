# Python

# `pip`

## Pip cache

Cache location:

* Unix: `~/.cache/pip` and it respects the `XDG_CACHE_HOME` directory.
* macOS: `~/Library/Caches/pip`.
* Windows `<CSIDL_LOCAL_APPDATA>\pip\Cache`.

To disable the cache, use the flag `--no-cache-dir`

sources:

* https://pip.pypa.io/en/latest/reference/pip_install/#caching

# Tips

There is a built-in web server embed in Python3 that can server a folder:


```bash
python3 -m http.server 8000  # serve the current folder on every interfaces

# python3.4: bind
# Python 3.7: directory
python -m http.server --bind 127.0.0.1 --directory /tmp/
```

sources:

* https://docs.python.org/3/library/http.server.html
