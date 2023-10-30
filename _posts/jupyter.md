# config file 생성
jupyter notebook --generate-config
jupyter_notebook_config.py

# 비밀번호 생성
jupyter server password
jupyter_server_config.json

# 외부 접속 허용하기
c.NotebookApp.allow_origin = '*'

# 포트 설정
c.NotebookApp.port = '8888'

# 서버 실행하기
jupyter notebook --config jupyter_notebook_config.py