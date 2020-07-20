---
layout: post
title:  "Apscheduler(Python 스케쥴링 라이브러리)"
comments: true
categories: [Programming Language, Python, Library, Apscheduler]
shortinfo: "스케쥴링이 필요한 어플리케이션을 제작할 경우, 가장 유용하게 사용하는 라이브러리이다."
tags: [Programming Language, Python, Scheduler, Library, Apscheduler]
---

### Apscheduler

Apscheduler란?
- Python code에 대한 Scheduling을 구성해주는 library

Template code
```python
from apscheduler.jobstores.base import JobLookupError
from apscheduler.schedulers.blocking import BlockingScheduler
from apscheduler.schedulers.background import BackgroundScheduler
from .logger import Logger
import time
import datetime

logger = Logger(method='scheduler', dir='log', filename='scheduler.log')

class Scheduler(object):
    def __init__(self):
        logger.set_log('INFO', "Scheduler is initialized", extra={'classAndFunc': self.__init__.__qualname__})
        self.sched = BackgroundScheduler({
            'apscheduler.timezone': 'Asia/Seoul',
            'apscheduler.job_defaults.max_instances': '7'
        })
        self.sched.start()
        self.job_id = ''

    def __del__(self):
        self.shutdown()

    def shutdown(self):
        logger.set_log('DEBUG', "Shutdown", extra={'classAndFunc': self.shutdown.__qualname__})
        self.sched.shutdown()

    def kill_scheduler(self, job_id):
        try:
            self.sched.remove_job(job_id)
        except JobLookupError as e:
            logger.set_log('INFO', "Failed to stop Scheduler : {0}".format(e), extra={'classAndFunc': self.kill_scheduler.__qualname__})
        return

    def run_job(self, type, job_id, function, sleep_time=0, *args, **kwargs):
        if sleep_time != 0:
            time.sleep(sleep_time)
        try:
            result = function(*args, **kwargs)
            logger.set_log('DEBUG', "{0} Scheduler p_id[{1}] : {2} // Sleep time : {3}".format(type, job_id, time.localtime().tm_sec, sleep_time), extra={'classAndFunc': self.run_job.__qualname__})
        except Exception as e:
            result = None
            logger.set_log('INFO', "Failed to run function : {0}".format(e), extra={'classAndFunc': self.run_job.__qualname__})

    def scheduler(self, type: str, job_id: str, sleep_time=0, function=None, seconds: int=1, year: str=None, month: str=None, day: str=None, week: str=None, day_of_week: str='mon-fri', hour: str=None, minute: str=None, second: str=None, *args, **kwargs):
        logger.set_log('INFO', "{0} Scheduler start".format(type), extra={'classAndFunc': self.scheduler.__qualname__})
        if type == 'interval':
            self.sched.add_job(self.run_job, type, seconds=seconds, id=job_id, args=(type, job_id, function, sleep_time) + args, kwargs=kwargs)
        elif type == 'cron':
            self.sched.add_job(self.run_job, type, 
            year=year, month=month, day=day, week=week, 
            day_of_week=day_of_week, 
            hour=hour, minute=minute, second=second, 
            id=job_id, args=(type, job_id, function, sleep_time) + args, kwargs=kwargs)
        #elif type == 'cron_current':
        ###    self.sched.add_job(self.run_job, type, day_of_week='mon-fri', hour='9-15', second='*/5', id=job_id, args=(type, job_id, code, postfix, sleep_time))
if __name__=="__main__":
    def hello(name):
        print(name)
        print("Hello!")

    scheduler = Scheduler()
    scheduler.scheduler(a="Taelim", type='cron', job_id='1', second='*/1', sleep_time=0, function=hello)

    while True:
        logger.set_log('INFO', "{0} Scheduler is running...".format(type), extra={'classAndFunc': 'main'})
        time.sleep(600)
```

*args, **kwargs를 넘기려면?
```python
### add_job method에 parameter 넘길 시, args는 tuple 합연산으로, kwargs는 kwargs 변수로 넘기면(또는 다른 kwargs 변수와 합친 상태로) 된다
self.sched.add_job(self.run_job, type, 
            year=year, month=month, day=day, week=week, 
            day_of_week=day_of_week, 
            hour=hour, minute=minute, second=second, 
            id=job_id, args=(type, job_id, function, sleep_time) + args, kwargs=kwargs)
```

### 참조 링크
[Apscheduler란](https://tomining.tistory.com/138)