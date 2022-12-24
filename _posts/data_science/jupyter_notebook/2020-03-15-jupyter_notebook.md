---
layout: post
title:  "Jupyter notebook 설치 및 사용법"
comments: true
categories: [Data Science, Jupyter Notebook]
shortinfo: "Jupyter notebook에 관한 내용이 포함되어 있습니다."
tags: [Data Science, Data Analysis, Jupyter Notebook, Python]
---

### 최초 접속 경로 변경법

다음의 Command 입력
```bash
(base) C:\Users\User>;jupyter notebook --generate-config
Writing default config to: C:\Users\User\.jupyter\jupyter_notebook_config.py
```
위의 결과로, jupyter_notebook_config.py 수정하면 된다

아래의 설정 코드를 입력
```bash
#### The directory to use for notebooks and kernels.
c.NotebookApp.notebook_dir = 'D:\Python_Project'
```

### 화면 width 조정

custom.css 직접 수정
```css
/* 아래 경로 중 한 곳에 custom.css로 파일 생성 후 저장 */
/* ~/.ipython/profile_default/static/custom/custom.css (IPython) */
/* ~/.jupyter/custom/custom.css (Jupyter) */
.container { width:100% !important; }
```

실행 cell에서 수정
```python
from IPython.core.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))
```

### Server 설정

```bash
### Secure notebook server
jupyter notebook --generate-config

### Prepare hashed password
### In IPython mode
In [1]: from notebook.auth import passwd 
In [2]: passwd() Enter password: Verify password: 
Out[2]: 'sha1:f24baff49ac5:863dd2ae747212ede58125302d227f0ca7b12bb3'

### Create OpenSSL certification
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

### Modify configure file
vi /root/.jupyter/jupyter_notebook_config.py
### In vi editor
c.NotebookApp.password = u'sha1:f24baff....' 
c.NotebookApp.certfile = u'/home/사용자/mycert.pem'
c.NotebookApp.open_browser = False
c.NotebookApp.notebook_dir = u'/home/사용자/자료폴더/'
c.NotebookApp.ip = '*'
c.NotebookApp.port_retries = 8888

### Make service for initializing after boot
sudo vi /etc/systemd/system/jupyter.service
### In vi editor
[Unit] Description=Jupyter Notebook Server

[Service]
Type=simple
PIDFile=/run/jupyter.pid
User=<username>
ExecStart=/home/<username>/.local/bin/jupyter-notebook
WorkingDirectory=/your/working/dir

[Install]WantedBy=multi-user.target

### Enable service
systemctl daemon-reload
systemctl enable jupyter.service
systemctl start jupyter.service
```
### Asyncio 사용 시 발생하는 문제 해결법(Only in jupyter notebook)
https://markhneedham.com/blog/2019/05/10/jupyter-runtimeerror-this-event-loop-is-already-running/

### 참조 링크

[Jupyter notebook server 설정 방법-1](https://yongbeomkim.github.io/jupyter-server/)

[Jupyter notebook server 설정 방법-2](https://goodtogreate.tistory.com/entry/IPython-Notebook-%EC%84%A4%EC%B9%98%EB%B0%A9%EB%B2%95)

[Jupyter notebook server 설정 방법-3](https://medium.com/@GuruAtWork/jupyter-notebook-adding-certificates-for-ease-of-use-447f476b9112)