---
title: "Python Building exe"
date: 2018-09-03 10:25:00 -0400
categories: Python-Modules
---


## 참고
> [https://pyinstaller.readthedocs.io][1]
> 
> Python 기준 버전 3.6.6

## Python Building exe
python을 사용할때 개발환경에서는 python을 설치하여 개발하나, 실제 운영환경에서는 python을 설치하여 사용하기 힘든 경우가 있습니다.
실제로 기존 시스템이 운영중이기 때문에 설치가 불가능한 경우, 혹은 간단한 스크립트때문에 python 및 다양한 패키지를 설치해야 할 경우가 있습니다

이와같은 경우에는 실제로 Python 및 모듈들을 설치하지 않고도 exe파일로 관련 파일들을 묶어 사용하는 방법이 있습니다.

Python에서는 크게 다음 모듈들이 지원하고 있습니다.
- pyinstaller
- cx_freeze
- py2exe

다음과 같은 모듈들이 있으나, 각각 설명을 해보자면
- cx_freeze
  - 모듈 포함하기가 생각보다 어렵고, setup.py 파일을 작성하여야 함
- py2exe
  - cx_freeze 와 동일한 문제
- pyinstaller
  - 가장 사용하기 쉬우며, import 파일들을 알아서 묶어줌

이며, 기존 솔루션을 python으로 컨버팅 하는 과정에서는 위의 장점때문에 pyinstaller 를 많이 사용하게 되었으며,

따라서 pyinstaller 를 기준으로 설명하고자 합니다.

## pyinstaller
pyinstaller 를 간단하게 소개해보자면 Python2.7 and Python3.3+ 이상을 exe bundle로 묶어주는 Python 모듈입니다. 

실행 방법도 간단하게 pyinstaller module.py 면 충분하며,
한개의 exe 파일로 묶기 위해서는 -F 옵션 혹은 \-\-onefile 옵션을 사용하면 됩니다.

만약 사용할때 pyinstaller 명령어를 찾을수 없다고 나오는 경우에는 window 의 경우에는 path 환경변수에 python 설치경로의 Scripts 폴더를 추가해주면 해결됩니다.

### Options
pyinstaller 의 옵션은 다음과 같습니다. [전체 옵션][2]은 다음 문서를 참조하면 됩니다.
```
usage: pyinstaller [-h] [-v] [-D] [-F] [--specpath DIR] [-n NAME]
                   [--add-data <SRC;DEST or SRC:DEST>]
                   [--add-binary <SRC;DEST or SRC:DEST>] [-p DIR]
                   [--hidden-import MODULENAME]
                   [--additional-hooks-dir HOOKSPATH]
                   [--runtime-hook RUNTIME_HOOKS] [--exclude-module EXCLUDES]
                   [--key KEY] [-d] [-s] [--noupx] [-c] [-w]
                   [-i <FILE.ico or FILE.exe,ID or FILE.icns>]
                   [--version-file FILE] [-m <FILE or XML>] [-r RESOURCE]
                   [--uac-admin] [--uac-uiaccess] [--win-private-assemblies]
                   [--win-no-prefer-redirects]
                   [--osx-bundle-identifier BUNDLE_IDENTIFIER]
                   [--runtime-tmpdir PATH] [--distpath DIR]
                   [--workpath WORKPATH] [-y] [--upx-dir UPX_DIR] [-a]
                   [--clean] [--log-level LEVEL]
                   scriptname [scriptname ...]
```
몇몇 주요 옵션들을 살펴보자면,

\-\-clean	

    : build 전에 임시 파일들을 모두 삭제한다.
      build 시 import module 을 변경했을 경우 새로 import된 모듈을 불러오지 못할 경우 사용하면 된다.

\-\-onfile

    : build 시 폴더가 아닌 한개의 exe파일로 작성되어 나오게 된다.
      (기본 세팅을 dll등이 압축되지 않고 폴더로 나오게 된다)

\-\-icon

    : 아이콘을 지정할수 있다. (버그인지 경로에 띄어쓰기 불가능)

\-\-name

    : output 파일의 이름을 지정한다.

\-\-windowed

    : window 모드, 실행시 console 창을 띄우지 않는다.

\-\-version-file

    : version file을 읽어 해당 프로그램에 적용한다.
      version file은 다음과 같이 설정한다.
```python
CMD 의 파일 버전 설명이다.
VSVersionInfo(
ffi=FixedFileInfo(
    filevers=(6, 1, 7601, 17514),
    prodvers=(6, 1, 7601, 17514),
    mask=0x3f,
    flags=0x0,
    OS=0x40004,
    fileType=0x1,
    subtype=0x0,
    date=(0, 0)
    ),
kids=[
    StringFileInfo(
    [
    StringTable(
        u'040904B0',
        [StringStruct(u'CompanyName', u'Microsoft Corporation'),
        StringStruct(u'FileDescription', u'Windows Command Processor'),
        StringStruct(u'FileVersion', u'6.1.7601.17514 (win7sp1_rtm.101119-1850)'),
        StringStruct(u'InternalName', u'cmd'),
        StringStruct(u'LegalCopyright', u'\xa9 Microsoft Corporation. All rights reserved.'),
        StringStruct(u'OriginalFilename', u'Cmd.Exe'),
        StringStruct(u'ProductName', u'Microsoft\xae Windows\xae Operating System'),
        StringStruct(u'ProductVersion', u'6.1.7601.17514')])
    ]), 
    VarFileInfo([VarStruct(u'Translation', [1033, 1200])])
]
)
```
> ![no_support_completion](/assets/img/property.jpg)
>
> 다음과 같이 나오게 됩니다.

다음 옵션들을 사용하여 빌드할 경우 별도의 path를 지정하지 않을 경우에는 \\dist 폴더 안에 exe파일 혹은 폴더가 생기게 되며, python이 설치되지 않은 경우에도 사용이 가능하게 됩니다.

## 작동 방식
[원본 문서][3]를 참조하여 작성하였습니다.
### import analysis
import 에서 쓰인 모듈들을 사용하며, import 된 모듈에서 import 된 모듈들도 분석합니다.

재귀 형태로 분석하게 되며, 스크립트및 모듈이 사용하는 모든 모듈을 분석하여 사용하게 됩니다.

major 모듈들 ([목록][4]) 들의 경우에는 자동으로 리스트를 넣어줍니다. ex) PyQT, Django

다만, \_\_import__() function 혹은 sys.path에 import path 를 append 해준 경우에는 정상적으로 작동하지 않을수 있습니다.

그런 경우에는 옵션 및 spec 파일 수정을 통하여 module 을 직접 지정해주거나 path 를 추가해주는 방법을 사용할수 있습니다.

참고로 pyinstaller 는 기본적인 os 라이브러리등을 포함하지 않습니다. 
ex) /lib, /usr/lib 등의 system lib 이 있는 폴더

### one folder bundle
-F, \-\-one-file 옵션을 붙히지 않고 빌드할 경우에는 dist 폴더 내부에 해당 파일명의 폴더가 생기게 되며, 다른 시스템에서 사용할때는 해당 폴더 통째로, 혹은 압축하여 전달하면 사용이 가능합니다.

장점으로써는 import 가 바뀌지 않았다는 가정 하에 .exe 파일만 다시 보내주면 수정된 코드로 실행이 가능하다는 점입니다.

이는 -F, \-\-one-file 옵션으로 묶었을때 실제 파일이 6mb에서 module 이 많아질수록 파일 크기가 늘어나기 때문에 꽤나 큰 장점입니다.

다만, 파일 관리(상당히 많은 양의 dll, lib 등이 포함되어 있다)가 어렵기 때문에 이점은 단점으로 적용될수 있습니다.

### one file bundle
-F, \-\-one-file 옵션을 사용하였을 경우에는 dist 폴더에 스크립트명.exe 파일이 생기게 됩니다.

이는 파일 하나만 관리하면 되나, 다음과 같은 특징이 있습니다.

- OS 마다 다른 temp 폴더에 _MEIxxxx 파일이 생성됩니다.
  - 이는 정상적으로 프로그램이 종료되게 된다면 삭제되는 파일입니다.
  - \-\-runtime-tmpdir 옵션을 통하여 디렉토리 지정이 가능합니다.

이 특징의 경우에는 프로그램이 정상적으로 종료되지 않을 경우 ex) kill -9, taskkill /F _MEI 파일을 남기게 되며, 이는 무시할수 없을 정도의 디스크를 잡아먹게 됩니다.

실제로 서비스중인 경우 _MEI 파일이 쌓여 정상적이지 않게 동작할수 있습니다. (프로그램 뿐만이 아니라 시스템 전체적으로)


## 기타 중요사항
>버전업된 Python3.7에서는 정상 동작하는것을 확인하지 못하였습니다.
>
> 확인 일자 2018-09-03
> 확인 버전
> ```
> Python3.7.0
> Package     Version
> ----------- --------
> altgraph    0.16.1
> future      0.16.0
> macholib    1.11
> pefile      2018.8.8
> pip         10.0.1
> PyInstaller 3.3.1
> pypiwin32   223
> pywin32     223
> setuptools  39.0.1
> ```
> 기본설치 후 pip 를 통하여 pyinstaller 만 최신버전으로 설치하였습니다.
>
> ```
> Fatal Python error: initfsencoding: unable to load the file system codec
> zipimport.ZipImportError: can't find module 'encodings'
> ```
> 다음과 같은 오류가 나며 비정상적으로 안되는것을 확인하였습니다.


[1]: https://pyinstaller.readthedocs.io
[2]: https://pyinstaller.readthedocs.io/en/v3.3.1/usage.html
[3]: https://pyinstaller.readthedocs.io/en/v3.3.1/operating-mode.html
[4]: https://github.com/pyinstaller/pyinstaller/wiki/Supported-Packages