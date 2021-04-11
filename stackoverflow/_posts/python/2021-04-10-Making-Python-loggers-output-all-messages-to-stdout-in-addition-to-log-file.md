---
layout: post
title: Making Python loggers output all messages to stdout in addition to log file
image: /assets/img/stackoverflow/logger.png
description: >
  Is there a way to make Python logging using the `logging` module automatically output things to stdout in addition to the log file where they are supposed to go?
hide_description: true
hide_image: true
comments: true
more_posts: posts.md
featured: true
last_modified_at: 2021-04-11
categories: [stackoverflow, python]
tags: [python, logging, error-logging]
---

Making Python loggers output all messages to stdout in addition to log file

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/14058453/making-python-loggers-output-all-messages-to-stdout-in-addition-to-log-file) by [Ben](https://stackoverflow.com/users/4563947/ben)

Is there a way to make Python logging using the `logging` module automatically output things to stdout in addition to the log file where they are supposed to go? For example, I'd like all calls to `logger.warning`, `logger.critical`, `logger.error` to go to their intended places but in addition always be copied to `stdout`. This is to avoid duplicating messages like:

~~~py
# file: 'question.py'
mylogger.critical("something failed")
print "something failed"
~~~

## [Answer](https://stackoverflow.com/a/63338866/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

For more detailed explanations - great documentation at that [link][1].
For example: It's easy, you only need to set up two loggers.  

```py
# file: 'answer.py'
import sys
import logging

logger = logging.getLogger('')
logger.setLevel(logging.DEBUG)
fh = logging.FileHandler('my_log_info.log')
sh = logging.StreamHandler(sys.stdout)
formatter = logging.Formatter('[%(asctime)s] %(levelname)s [%(filename)s.%(funcName)s:%(lineno)d] %(message)s', datefmt='%a, %d %b %Y %H:%M:%S')
fh.setFormatter(formatter)
sh.setFormatter(formatter)
logger.addHandler(fh)
logger.addHandler(sh)

def hello_logger():
    logger.info("Hello info")
    logger.critical("Hello critical")
    logger.warning("Hello warning")
    logger.debug("Hello debug")

if __name__ == "__main__":
    hello_logger()
```

Output - terminal:
```sh
# file: 'terminal'
[Mon, 10 Aug 2020 12:44:25] INFO [TestLoger.py.hello_logger:15] Hello info
[Mon, 10 Aug 2020 12:44:25] CRITICAL [TestLoger.py.hello_logger:16] Hello critical
[Mon, 10 Aug 2020 12:44:25] WARNING [TestLoger.py.hello_logger:17] Hello warning
[Mon, 10 Aug 2020 12:44:25] DEBUG [TestLoger.py.hello_logger:18] Hello debug
```

Output - in file:

[![log in file][2]][2]

Output
{:.figcaption}

***
### UPDATE: color terminal

Package:
```sh
# file: 'terminal'
pip install colorlog
```

Code:

```py
# file: 'answer.py'
import sys
import logging
import colorlog

logger = logging.getLogger('')
logger.setLevel(logging.DEBUG)
fh = logging.FileHandler('my_log_info.log')
sh = logging.StreamHandler(sys.stdout)
formatter = logging.Formatter('[%(asctime)s] %(levelname)s [%(filename)s.%(funcName)s:%(lineno)d] %(message)s', datefmt='%a, %d %b %Y %H:%M:%S')
fh.setFormatter(formatter)
sh.setFormatter(colorlog.ColoredFormatter('%(log_color)s [%(asctime)s] %(levelname)s [%(filename)s.%(funcName)s:%(lineno)d] %(message)s', datefmt='%a, %d %b %Y %H:%M:%S'))
logger.addHandler(fh)
logger.addHandler(sh)

def hello_logger():
    logger.info("Hello info")
    logger.critical("Hello critical")
    logger.warning("Hello warning")
    logger.debug("Hello debug")
    logger.error("Error message")

if __name__ == "__main__":
    hello_logger()
```

output:
[![enter image description here][3]][3]

Output
{:.figcaption}


### Recommendation:

[Complete logger configuration][4] from `INI` file, which also includes setup for `stdout` and `debug.log`:
- `handler_file`
   - `level=WARNING`
- `handler_screen`
   - `level=DEBUG`


  [1]: https://docs.python.org/3/howto/logging.html
  [2]: https://i.stack.imgur.com/edeFh.png
  [3]: https://i.stack.imgur.com/Q5gAi.png
  [4]: https://stackoverflow.com/questions/38537905/set-logging-levels/65975273#65975273