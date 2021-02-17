# Anaconda installation using Windows subsystem for Linux 2

![img/Jupyter/Untitled.png](img/Jupyter/Untitled.png)

---

## 1. Ubuntu 설치 @ Windows 10 – WSL2

2020. 05. windows 10 update`를 통해 **Windows subsystem for Linux 2 (WSL2)**가 정식 설치가 가능해졌습니다. **WSL2**를 이용하면 Windows 10 환경에서 Linux 를 구동할 수 있고 구축된 Linux 환경에서 **Ubuntu**를 설치하여 python code 개발 통합환경인 **Anaconda**를 설치할 수 있습니다. 우선 아래 링크를 참조하여 **WSL2** 및 **Ubuntu** 설치를 진행합니다.

WSL2 활성화 및 Linux 설치: [[링크]](https://www.44bits.io/ko/post/wsl2-install-and-basic-usage)

`Error: 0x8037012`  에러발생 시 문제 해결: [[링크]](https://stackoverflow.com/questions/62340566/fix-wslregisterdistribution-failed-with-error-0x80370102)

## 2. Visualization: X11 서버 설치

가상머신에서 시각화를 위해서 GUI 프로그램인 **VcXsrv**를 설치하도록 합니다: [[링크]](https://sourceforge.net/projects/vcxsrv/)

설치 후에 생성된 **Xlaunch**를 실행하고 아래와 같은 설정을 입력합니다.

![img/Jupyter/Untitled%201.png](img/Jupyter/Untitled%201.png)

![img/Jupyter/Untitled%202.png](img/Jupyter/Untitled%202.png)

![img/Jupyter/Untitled%203.png](img/Jupyter/Untitled%203.png)

설정을 완료하면 백그라운드에 **Xlaunch**가 실행됩니다. 이를 확인하고 **Ubuntu** terminal에서 GUI가 제대로 작동하는지 확인하기 위해 **xeyes**를 실행시켜 봅니다. 우선 **x11** 프로그램을 **Ubuntu** 환경에 설치하겠습니다.

```bash
apt-get install x11-apps
```

설치가 완료되면 다음과 같은 명령어를 시도해봅니다.

```bash
xeyes
```

정상적으로 설치가 진행이 되었다면 다음과 같은 프로그램이 실행되는 것을 확인할 수 있습니다.

![img/Jupyter/Untitled%204.png](img/Jupyter/Untitled%204.png)

그러나 xeyes가 실행되지 않는다면 다음과 같은 조치를 취합니다.

```bash
vi ~/.bashrc
```

위 명령어를 구동하면 vim 에디터를 실행되며 `bashrc`의 코드가 확인할 수 있습니다. `bashrc`에는 bash 환경이 처음 실행될때 자동으로 동작되는 명령어들이 작성되어있습니다. 이의 마지막 줄에 아래의 코드를 추가합니다. 이에 대한 자세한 방법은 아래과 같습니다. (기본적인 vim 에디터 사용법 [[링크]](https://devhints.io/vim)).

- `i` 를 통해 입력 모드로 전환
- 방향키를 통해 코드를 추가할 맨 아래줄로 이동
- 아래 코드작성

```bash
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
```

- `Esc` 를 통해 명령어 모드로 전환
- `:wq` 를 통해 저장 후 종료

마지막으로 아래 명령어를 통해 설정한 `bashrc` 파일을 bash 환경에 동기화시켜주도록 하고 다시 **xeyes**가 작동하는지 확인합니다.

```bash
source ~/.bashrc
```

### 3. Linux 환경에 Anaconda 설치

우선 Anaconda 설치용 shell script를 설치합니다: [[링크]](https://www.anaconda.com/distribution/#download-section)

![img/Jupyter/Untitled%205.png](img/Jupyter/Untitled%205.png)

설치된 파일[e.g. 2021년 2월 17일 기준 최신버젼: `Anaconda3-2020.11-Linux-x86_64.sh`]을 `c:` 드라이브의 기본 위치에 옮기고 **Ubuntu**를 실행합니다(. 다음과 같은 명령어를 통해 설치한 파일을 **Linux** 환경에 복사하도록 합니다. 

```bash
cp /mnt/c/Anaconda3-2020.11-Linux-x86_64.sh ~/.
```

이제 아래와 같이 bash shell 에서 `ls` 를 치면 `Anaconda3-2020.11-Linux-x86_64.sh`  파일이 해당 경로로 이동했음을 확인할 수 있습니다. 다음 명령어를 통해 설치 파일을 실행시킵니다.

```bash
bash Anaconda3-2020.11-Linux-x86_64.sh
```

라이센스 서약을 `Enter` 를 통해 읽고 `yes` 를 입력하여 다음으로 진행:

![img/Jupyter/Untitled%206.png](img/Jupyter/Untitled%206.png)

`Enter` 를 입력하여 **Anaconda3** 설치 위치 설정:

![img/Jupyter/Untitled%207.png](img/Jupyter/Untitled%207.png)

설치 진행이 완료되면 다음과 같은 명령어를 통해 **Anaconda3**가 제대로 설치된 것을 확인 할 수 있습니다.

```bash
anaconda-navigator
```

![img/Jupyter/Untitled%208.png](img/Jupyter/Untitled%208.png)

### 4. Jupyter notebook 실행

Python 코드를 실행해볼 수 있는 **Jupyter notebook**를 원격 브라우저 환경에서 실행시기키 위해서는 다음과 같은 방법을 이용할 수 있습니다.

```bash
cd ~
jupyter-notebook --generate-config
cd .jupyter
vi jupyter_notebook_config.py
```

Vim 에디터가 열리면 `/open_brower` 을 입력하여 관련 키워드를 검색합니다.

![img/Jupyter/Untitled%209.png](img/Jupyter/Untitled%209.png)

발견된 위치에 해당하는 코드의 줄을 다음과 같이 수정합니다(주석 문자 `#` 지움).  이를 통해서  **Jupyter notebook**가 실행될 때 자동으로 브라우져를 실행하지 않도록 할 수 있습니다(참고로 기본 브라우져가 설치된 환경에서는 유용한 설정일 수 있습니다).

```python
c.NotebookApp.open_browser = False
```

코드를 수정하고 `Esc` 를 통해서 명령어 모드로 전환 후 `:wq` 을 입력하여 코드를 저장하고 Vim를 종료합니다. 이제 **Jupyter notebook**를 실행할 수 있습니다.

```python
jupyter notebook
```

![img/Jupyter/Untitled%2010.png](img/Jupyter/Untitled%2010.png)

위 명령어를 구동하면 아래와 같이 URLs 정보가 입력됩니다. 이를 복사하여 자신의 브라우져 환경에서 접속하면 **Jupyter notebook**이 실행된 것을 확인 할 수 있습니다.

![img/Jupyter/Untitled%2011.png](img/Jupyter/Untitled%2011.png)