# 최초 접속 경로 변경법

다음의 Command 입력
```bash
(base) C:\Users\User>jupyter notebook --generate-config
Writing default config to: C:\Users\User\.jupyter\jupyter_notebook_config.py

(base) C:\Users\User>
```
위의 결과로, jupyter_notebook_config.py 수정하면 된다

아래의 설정 코드를 입력
```bash
## The directory to use for notebooks and kernels.
c.NotebookApp.notebook_dir = 'D:\Python_Project'
```

# 화면 width 조정

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