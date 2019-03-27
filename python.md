Notes on Python
===============

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
