# 최초 접속 경로 변경법

다음의 Command 입력
```
(base) C:\Users\User>jupyter notebook --generate-config
Writing default config to: C:\Users\User\.jupyter\jupyter_notebook_config.py

(base) C:\Users\User>
```
위의 결과로, jupyter_notebook_config.py 수정하면 된다
아래의 설정 코드를 입력
```
## The directory to use for notebooks and kernels.
c.NotebookApp.notebook_dir = 'D:\Python_Project'
```