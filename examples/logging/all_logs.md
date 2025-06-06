# Logging "All" Logs

This example captures all logs sent to the Python logger.  This is helpful if you don't know the logger name
for the loggers config or if you simply want to make sure you collect ALL logs.  But be warned, ALL logs might
be a lot of logs:

```python
LOG_LEVEL = "DEBUG" if DEBUG else "INFO"
LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "formatters": {
        "normal": {
            "format": "%(asctime)s.%(msecs)03d %(levelname)-7s %(name)s:  %(message)s",
            "datefmt": "%H:%M:%S",
        },
    },
    "handlers": {
        "normal_console": {
            "level": "DEBUG",
            "class": "logging.StreamHandler",
            "formatter": "normal",
        },
    },
    "loggers": {
        "django": {
            "handlers": ["normal_console"],
            "level": "INFO"
        },
        "nautobot": {
            "handlers": ["normal_console"],
            "level": LOG_LEVEL,
        },
    },
    "root": {
        "handlers": ["normal_console"],
        "level": LOG_LEVEL,
    },
}
```

The important addition is the `LOGGING["root"]` entry here.  You can skip the `LOGGING["loggers"]` section
entirely if you wish.

## Example Logs

```no-highlight
20:38:28.967 INFO    django.server: "GET / HTTP/1.1" 200 29409
20:38:29.174 INFO    django.server: "GET /static/debug_toolbar/js/toolbar.js HTTP/1.1" 304 0
20:38:29.194 INFO    django.server: "GET /static/debug_toolbar/css/toolbar.css HTTP/1.1" 304 0
20:38:29.677 INFO    django.server: "GET /static/debug_toolbar/css/print.css HTTP/1.1" 304 0
20:38:29.775 INFO    django.server: "GET /static/debug_toolbar/js/utils.js HTTP/1.1" 304 0
20:38:33.433 INFO    django.server: "GET /login/?next=/ HTTP/1.1" 200 18335
20:38:41.121 INFO    nautobot.auth.login: User admin successfully authenticated
20:38:42.053 INFO    django.server: "POST /login/ HTTP/1.1" 302 0
```
