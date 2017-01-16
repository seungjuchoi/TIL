# python logging
## 간단한 방법
```python
import logging
logging.warning('Watch out!')  # will print a message to the console
logging.info('I told you so')  # will not print anything
logging.warning('%s before you %s', 'Look', 'leap!')
```

```python
import logging
logging.basicConfig(filename='example.log',level=logging.DEBUG)
logging.debug('This message should go to the log file')
logging.info('So should this')
logging.warning('And this, too')
```

파일로 저장하는 간단한 방법. 그러나 Stream으로도 보내고 싶다면 아래 방법을 고려해야 한다. 여기서 stream은 console을 의미한다.

```python
import logging
logging.basicConfig(format='%(levelname)s:%(message)s', level=logging.DEBUG)
logging.debug('This message should appear on the console')
logging.info('So should this')
logging.warning('And this, too')
```
format을 기억하자.

## Handler 사용 방법
```python
import logging

# create logger
logger = logging.getLogger('simple_example')
logger.setLevel(logging.DEBUG)

# create console handler and set level to debug
ch = logging.StreamHandler()
ch.setLevel(logging.DEBUG)

# create formatter
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

# add formatter to ch
ch.setFormatter(formatter)

# add ch to logger
logger.addHandler(ch)

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')
```
같은 방법으로 fileHandler를 등록하면 파일과 stream으로 동시에 써진다. Formatter를 잘 활용하면 debugging에 도움이 많이 될 것이다.

마지막으로 conf를 파일로 저장해서 불러와 간편하게 쓰는 방법도 있다.
위에 관련 내용들은 https://docs.python.org/3/howto/logging.html 에서 다시 보자.
