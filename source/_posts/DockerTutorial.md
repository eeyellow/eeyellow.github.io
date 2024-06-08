---
title: Docker技術分享
date: 2024-06-08 22:13:42
tags:
---
# Docker技術分享

### Docker簡介

- Docker是一個開源的平台，用於開發、交付和執行應用程序。它透過容器（Container）技術實現應用程序的封裝，確保應用程序可以在任何環境中一致地運行。
- 特點
    1. **輕量級**：容器共享主機的操作系統內核，避免了虛擬機的額外開銷，使得容器更輕量，啟動速度更快。
    2. **一致性**：開發、測試、部署環境一致，減少了「在我的電腦上可以運行」這類問題。
    3. **便捷性**：容易打包、分享和部署應用，通過Dockerfile可以簡單自動化構建過程。
    4. **隔離性**：每個容器都是獨立的運行環境，應用之間互不干擾，提高安全性和穩定性。
    5. **擴展性**：容易擴展，可以輕鬆增加或減少容器數量以應對負載變化。
- 與傳統虛擬機的比較
    
    {% asset_img Untitled.png %}

    {% asset_img Untitled_1.png %}
    
### 環境建置：Windows 11 + WSL2 （Ubuntu）

1. 啟動WSL2
    
    {% asset_img Untitled_2.png %}
    
2. 從 Microsoft Store 中安裝 Ubuntu （22.04）
    
    {% asset_img Untitled_3.png %}
    
3. Windows Update
    
    {% asset_img Untitled_4.png %}
    
4. （如果原本有安裝）移除Docker Desktop
5. 啟動WSL2（Ubuntu）
    
    {% asset_img Untitled_5.png %}
    
6. 在WSL2中安裝Docker
    
    ```bash
    curl -fsSL https://get.docker.com -o get-docker.sh	
    sudo sh get-docker.sh	
    sudo usermod -aG docker $USER	
    sudo apt-get update && sudo apt-get install docker-compose docker-compose-plugin
    	
    # Using Ubuntu 22.04 or Debian 10 / 11? You need to do 1 extra step for iptables
    # compatibility, you'll want to choose option (1) from the prompt to use iptables-legacy.
    
    sudo update-alternatives --config iptables	# 選nftables-auto
    sudo service docker start
    ```
    
7. 終端機美化（Optional）
    
    ```bash
    # 安裝 zsh    
    sudo apt install zsh
    # 更改預設shell
    chsh -s $(which zsh)
    # 安裝 oh-my-zsh
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    # 安裝 powerlevel10k
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
    # 設定zsh的主題使用powerlevel10k
    sed -i 's/ZSH_THEME="[^"]*"/ZSH_THEME="powerlevel10k\/powerlevel10k"/' ~/.zshrc     
    
    git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
    
    sed -i 's/# ENABLE_CORRECTION="true"/ENABLE_CORRECTION="true"/' ~/.zshrc
    sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc
    
    # LS_COLORS
    # https://medium.com/@asvinjangid.kumar/changing-colors-of-files-and-folders-in-linux-5d29967c3012
    echo 'export LS_COLORS="bd=0;38;2;102;217;239;48;2;51;51;51:rs=0:mi=0;38;2;0;0;0;48;2;255;74;68:do=0;38;2;0;0;0;48;2;249;38;114:di=0;38;2;102;217;239:ln=0;38;2;249;38;114:ow=0:fi=0:ca=0:no=0:pi=0;38;2;0;0;0;48;2;102;217;239:st=0:so=0;38;2;0;0;0;48;2;249;38;114:sg=0:su=0:ex=1;38;2;249;38;114:cd=0;38;2;249;38;114;48;2;51;51;51:or=0;38;2;0;0;0;48;2;255;74;68:tw=0:mh=0:*~=0;38;2;122;112;112:*.m=0;38;2;0;255;135:*.z=4;38;2;249;38;114:*.d=0;38;2;0;255;135:*.h=0;38;2;0;255;135:*.o=0;38;2;122;112;112:*.p=0;38;2;0;255;135:*.t=0;38;2;0;255;135:*.a=1;38;2;249;38;114:*.r=0;38;2;0;255;135:*.c=0;38;2;0;255;135:*.pl=0;38;2;0;255;135:*.la=0;38;2;122;112;112:*.di=0;38;2;0;255;135:*.ml=0;38;2;0;255;135:*.md=0;38;2;226;209;57:*.hi=0;38;2;122;112;112:*.jl=0;38;2;0;255;135:*.wv=0;38;2;253;151;31:*.bz=4;38;2;249;38;114:*.cc=0;38;2;0;255;135:*.mn=0;38;2;0;255;135:*.bc=0;38;2;122;112;112:*.hh=0;38;2;0;255;135:*.ex=0;38;2;0;255;135:*.rb=0;38;2;0;255;135:*.rs=0;38;2;0;255;135:*.7z=4;38;2;249;38;114:*.td=0;38;2;0;255;135:*.pp=0;38;2;0;255;135:*.ui=0;38;2;166;226;46:*.sh=0;38;2;0;255;135:*.kt=0;38;2;0;255;135:*.lo=0;38;2;122;112;112:*.gv=0;38;2;0;255;135:*.cr=0;38;2;0;255;135:*.py=0;38;2;0;255;135:*.fs=0;38;2;0;255;135:*.so=1;38;2;249;38;114:*.ll=0;38;2;0;255;135:*css=0;38;2;0;255;135:*.ps=0;38;2;230;219;116:*.cp=0;38;2;0;255;135:*.gz=4;38;2;249;38;114:*.el=0;38;2;0;255;135:*.vb=0;38;2;0;255;135:*.pm=0;38;2;0;255;135:*.js=0;38;2;0;255;135:*.go=0;38;2;0;255;135:*.hs=0;38;2;0;255;135:*.ts=0;38;2;0;255;135:*.as=0;38;2;0;255;135:*.ko=1;38;2;249;38;114:*.xz=4;38;2;249;38;114:*.rm=0;38;2;253;151;31:*.cs=0;38;2;0;255;135:*.nb=0;38;2;0;255;135:*.rtf=0;38;2;230;219;116:*.com=1;38;2;249;38;114:*.tmp=0;38;2;122;112;112:*.bak=0;38;2;122;112;112:*.img=4;38;2;249;38;114:*.dll=1;38;2;249;38;114:*.git=0;38;2;122;112;112:*.exe=1;38;2;249;38;114:*.xls=0;38;2;230;219;116:*.bcf=0;38;2;122;112;112:*.xml=0;38;2;226;209;57:*.lua=0;38;2;0;255;135:*.sxi=0;38;2;230;219;116:*.ics=0;38;2;230;219;116:*.psd=0;38;2;253;151;31:*.php=0;38;2;0;255;135:*.m4v=0;38;2;253;151;31:*.nix=0;38;2;166;226;46:*TODO=1:*.tcl=0;38;2;0;255;135:*.mp4=0;38;2;253;151;31:*.tex=0;38;2;0;255;135:*.ttf=0;38;2;253;151;31:*.ppt=0;38;2;230;219;116:*.xlr=0;38;2;230;219;116:*.tif=0;38;2;253;151;31:*.hxx=0;38;2;0;255;135:*.gvy=0;38;2;0;255;135:*.pod=0;38;2;0;255;135:*.deb=4;38;2;249;38;114:*.mid=0;38;2;253;151;31:*.clj=0;38;2;0;255;135:*.gif=0;38;2;253;151;31:*.fls=0;38;2;122;112;112:*.ind=0;38;2;122;112;112:*.dpr=0;38;2;0;255;135:*.kts=0;38;2;0;255;135:*.eps=0;38;2;253;151;31:*.png=0;38;2;253;151;31:*.blg=0;38;2;122;112;112:*.vob=0;38;2;253;151;31:*.bin=4;38;2;249;38;114:*.bag=4;38;2;249;38;114:*.pas=0;38;2;0;255;135:*.pro=0;38;2;166;226;46:*.txt=0;38;2;226;209;57:*.vim=0;38;2;0;255;135:*.svg=0;38;2;253;151;31:*.ods=0;38;2;230;219;116:*.ipp=0;38;2;0;255;135:*.epp=0;38;2;0;255;135:*.def=0;38;2;0;255;135:*.awk=0;38;2;0;255;135:*.pgm=0;38;2;253;151;31:*.cxx=0;38;2;0;255;135:*.ini=0;38;2;166;226;46:*.wav=0;38;2;253;151;31:*.kex=0;38;2;230;219;116:*.rpm=4;38;2;249;38;114:*.mpg=0;38;2;253;151;31:*.tar=4;38;2;249;38;114:*.rar=4;38;2;249;38;114:*.jar=4;38;2;249;38;114:*.pyd=0;38;2;122;112;112:*.xcf=0;38;2;253;151;31:*.tbz=4;38;2;249;38;114:*.bmp=0;38;2;253;151;31:*.zst=4;38;2;249;38;114:*.dot=0;38;2;0;255;135:*.sxw=0;38;2;230;219;116:*.apk=4;38;2;249;38;114:*.ilg=0;38;2;122;112;112:*.wmv=0;38;2;253;151;31:*.odp=0;38;2;230;219;116:*.xmp=0;38;2;166;226;46:*.pid=0;38;2;122;112;112:*.bz2=4;38;2;249;38;114:*.cpp=0;38;2;0;255;135:*hgrc=0;38;2;166;226;46:*.pbm=0;38;2;253;151;31:*.swp=0;38;2;122;112;112:*.fsi=0;38;2;0;255;135:*.htc=0;38;2;0;255;135:*.cgi=0;38;2;0;255;135:*.toc=0;38;2;122;112;112:*.rst=0;38;2;226;209;57:*.ltx=0;38;2;0;255;135:*.csx=0;38;2;0;255;135:*.pkg=4;38;2;249;38;114:*.fnt=0;38;2;253;151;31:*.tml=0;38;2;166;226;46:*.htm=0;38;2;226;209;57:*.aux=0;38;2;122;112;112:*.mov=0;38;2;253;151;31:*.bib=0;38;2;166;226;46:*.ogg=0;38;2;253;151;31:*.mkv=0;38;2;253;151;31:*.sbt=0;38;2;0;255;135:*.bbl=0;38;2;122;112;112:*.tgz=4;38;2;249;38;114:*.asa=0;38;2;0;255;135:*.cfg=0;38;2;166;226;46:*.bsh=0;38;2;0;255;135:*.mir=0;38;2;0;255;135:*.bst=0;38;2;166;226;46:*.mli=0;38;2;0;255;135:*.odt=0;38;2;230;219;116:*.zip=4;38;2;249;38;114:*.erl=0;38;2;0;255;135:*.vcd=4;38;2;249;38;114:*.pdf=0;38;2;230;219;116:*.yml=0;38;2;166;226;46:*.wma=0;38;2;253;151;31:*.avi=0;38;2;253;151;31:*.inl=0;38;2;0;255;135:*.log=0;38;2;122;112;112:*.bat=1;38;2;249;38;114:*.hpp=0;38;2;0;255;135:*.otf=0;38;2;253;151;31:*.pyc=0;38;2;122;112;112:*.zsh=0;38;2;0;255;135:*.exs=0;38;2;0;255;135:*.jpg=0;38;2;253;151;31:*.dox=0;38;2;166;226;46:*.tsx=0;38;2;0;255;135:*.c++=0;38;2;0;255;135:*.fon=0;38;2;253;151;31:*.pyo=0;38;2;122;112;112:*.csv=0;38;2;226;209;57:*.mp3=0;38;2;253;151;31:*.ppm=0;38;2;253;151;31:*.doc=0;38;2;230;219;116:*.pps=0;38;2;230;219;116:*.arj=4;38;2;249;38;114:*.iso=4;38;2;249;38;114:*.m4a=0;38;2;253;151;31:*.idx=0;38;2;122;112;112:*.ps1=0;38;2;0;255;135:*.out=0;38;2;122;112;112:*.sql=0;38;2;0;255;135:*.inc=0;38;2;0;255;135:*.sty=0;38;2;122;112;112:*.elm=0;38;2;0;255;135:*.dmg=4;38;2;249;38;114:*.aif=0;38;2;253;151;31:*.h++=0;38;2;0;255;135:*.fsx=0;38;2;0;255;135:*.ico=0;38;2;253;151;31:*.flv=0;38;2;253;151;31:*.swf=0;38;2;253;151;31:*.docx=0;38;2;230;219;116:*.yaml=0;38;2;166;226;46:*.pptx=0;38;2;230;219;116:*.flac=0;38;2;253;151;31:*.rlib=0;38;2;122;112;112:*.purs=0;38;2;0;255;135:*.opus=0;38;2;253;151;31:*.epub=0;38;2;230;219;116:*.json=0;38;2;166;226;46:*.bash=0;38;2;0;255;135:*.hgrc=0;38;2;166;226;46:*.tiff=0;38;2;253;151;31:*.java=0;38;2;0;255;135:*.xlsx=0;38;2;230;219;116:*.less=0;38;2;0;255;135:*.dart=0;38;2;0;255;135:*.webm=0;38;2;253;151;31:*.lock=0;38;2;122;112;112:*.html=0;38;2;226;209;57:*.mpeg=0;38;2;253;151;31:*.psm1=0;38;2;0;255;135:*.conf=0;38;2;166;226;46:*.psd1=0;38;2;0;255;135:*.diff=0;38;2;0;255;135:*.h264=0;38;2;253;151;31:*.make=0;38;2;166;226;46:*.toml=0;38;2;166;226;46:*.lisp=0;38;2;0;255;135:*.fish=0;38;2;0;255;135:*.tbz2=4;38;2;249;38;114:*.orig=0;38;2;122;112;112:*.jpeg=0;38;2;253;151;31:*.mdown=0;38;2;226;209;57:*.cmake=0;38;2;166;226;46:*.swift=0;38;2;0;255;135:*.xhtml=0;38;2;226;209;57:*.dyn_o=0;38;2;122;112;112:*.class=0;38;2;122;112;112:*.cabal=0;38;2;0;255;135:*passwd=0;38;2;166;226;46:*README=0;38;2;0;0;0;48;2;230;219;116:*.toast=4;38;2;249;38;114:*shadow=0;38;2;166;226;46:*.shtml=0;38;2;226;209;57:*.patch=0;38;2;0;255;135:*.scala=0;38;2;0;255;135:*.cache=0;38;2;122;112;112:*.ipynb=0;38;2;0;255;135:*TODO.md=1:*.groovy=0;38;2;0;255;135:*INSTALL=0;38;2;0;0;0;48;2;230;219;116:*.gradle=0;38;2;0;255;135:*.matlab=0;38;2;0;255;135:*.config=0;38;2;166;226;46:*.dyn_hi=0;38;2;122;112;112:*LICENSE=0;38;2;182;182;182:*.flake8=0;38;2;166;226;46:*.ignore=0;38;2;166;226;46:*COPYING=0;38;2;182;182;182:*setup.py=0;38;2;166;226;46:*TODO.txt=1:*Makefile=0;38;2;166;226;46:*.desktop=0;38;2;166;226;46:*Doxyfile=0;38;2;166;226;46:*.gemspec=0;38;2;166;226;46:*.rgignore=0;38;2;166;226;46:*README.md=0;38;2;0;0;0;48;2;230;219;116:*.fdignore=0;38;2;166;226;46:*.markdown=0;38;2;226;209;57:*.cmake.in=0;38;2;166;226;46:*configure=0;38;2;166;226;46:*.kdevelop=0;38;2;166;226;46:*.DS_Store=0;38;2;122;112;112:*COPYRIGHT=0;38;2;182;182;182:*.gitignore=0;38;2;166;226;46:*.scons_opt=0;38;2;122;112;112:*CODEOWNERS=0;38;2;166;226;46:*SConscript=0;38;2;166;226;46:*Dockerfile=0;38;2;166;226;46:*INSTALL.md=0;38;2;0;0;0;48;2;230;219;116:*SConstruct=0;38;2;166;226;46:*README.txt=0;38;2;0;0;0;48;2;230;219;116:*.gitconfig=0;38;2;166;226;46:*.localized=0;38;2;122;112;112:*.gitmodules=0;38;2;166;226;46:*Makefile.am=0;38;2;166;226;46:*.synctex.gz=0;38;2;122;112;112:*INSTALL.txt=0;38;2;0;0;0;48;2;230;219;116:*Makefile.in=0;38;2;122;112;112:*.travis.yml=0;38;2;230;219;116:*MANIFEST.in=0;38;2;166;226;46:*LICENSE-MIT=0;38;2;182;182;182:*appveyor.yml=0;38;2;230;219;116:*configure.ac=0;38;2;166;226;46:*.applescript=0;38;2;0;255;135:*.fdb_latexmk=0;38;2;122;112;112:*CONTRIBUTORS=0;38;2;0;0;0;48;2;230;219;116:*.clang-format=0;38;2;166;226;46:*.gitattributes=0;38;2;166;226;46:*CMakeCache.txt=0;38;2;122;112;112:*CMakeLists.txt=0;38;2;166;226;46:*LICENSE-APACHE=0;38;2;182;182;182:*CONTRIBUTORS.md=0;38;2;0;0;0;48;2;230;219;116:*requirements.txt=0;38;2;166;226;46:*CONTRIBUTORS.txt=0;38;2;0;0;0;48;2;230;219;116:*.sconsign.dblite=0;38;2;122;112;112:*package-lock.json=0;38;2;122;112;112:*.CFUserTextEncoding=0;38;2;122;112;112"' >> ~/.zshrc
    echo "alias ll='ls -al'" >> ~/.zshrc
    
    # .vimrc
    # https://unix.stackexchange.com/questions/88879/better-colors-so-comments-arent-dark-blue-in-vim
    echo 'set number
    set encoding=utf-8
    set t_Co=256
    set background=dark
    let g:solarized_termcolors=256
    let g:solarized_termtrans=1' >> ~/.vimrc
    
    # 重新載入zsh設定檔
    source ~/.zshrc
    
    # 進行樣式選擇   
    ```
    

### 基本概念

- Dockerfile
    
    用來定義如何構建 Image 的文件。包含一系列指令，指定基礎 Image、需要安裝的套件、複製文件、設置環境變量等。
    
- Image（映像檔）
    
    一個唯讀的模板，可以用來建立 Docker Container。
    例如：一個 Image 可以包含一個完整的 ubuntu 作業系統環境，裡面僅安裝了 Apache 或使用者需要的其它應用程式。
    
- Container（容器）
    
    Container 是從 Image 建立的執行實例。它可以被啟動、開始、停止、刪除。每個 Container 都是相互隔離的、保證安全的平台。
    可以把 Container 看做是一個簡易版的 Linux 環境（包括 root 使用者權限、程式空間、使用者空間和網路空間等）和在其中執行的應用程式。
    
- [Docker Hub](https://hub.docker.com/)
    
    Docker 的公開映像倉庫，可直接下載已建置好的 Image。
    
- Docker Compose
    
    Docker Compose 是一個工具，用於定義和運行多容器的 Docker 應用程序，可通過 YAML 文件來配置應用服務，然後使用單個命令來創建和啟動所有服務。
    

### 常用指令

```bash
# 停止所有Container
docker stop $(docker ps -aq)
# 刪除所有Container
docker rm $(docker ps -aq)
# 刪除所有image
docker rmi $(docker images -q)
# 刪除所有的volume
docker volume rm $(docker volume ls -q)

# 啟動docker-compose
docker-compose up -d
# 終止docker-compose
docker-compose down
```

### 動手製作 Dockerfile

- Step1
    
    第一步就是先跑一台 Linux 出來
    
    ```bash
    cd ~ && mkdir DockerTutorial && cd DockerTutorial # 建立資料夾
    touch Dockerfile # 建立檔案
    code . # 開啟VSCode
    ```
    
    ```docker
    # 第一版
    
    # 基底 Image
    FROM ubuntu
    
    # 容器啟動後執行的命令
    CMD ["tail", "-f", "/dev/null"]
    ```
    
    建置 Image，並建立Container
    
    ```bash
    # 建立Image
    docker build -t my-image-1 .
    # 查看Image
    docker image list
    
    #建立 container
    docker run -d --name my-container-1 my-image-1
    #查看 container
    docker container list
    ```
    
    此時已經成功把 Linux 跑起來了，連進去這台 Linux 看看
    
    ```bash
     docker exec -it my-container /bin/bash
    ```
    
- Step2
    
    目標是用Docker跑一個Node Express起來
    
    網路上找如何在 Linux 上安裝 Express 的[教學](https://docs.vultr.com/installing-node-js-and-express-on-ubuntu-20-04)，實際在這台 Linux 裡面試試看
    
    ```bash
    DEBIAN_FRONTEND=noninteractive
    apt-get update -y
    #需要比較久，請稍等
    apt-get install nodejs npm vim -y
    mkdir app
    cd app
    npm init -y
    npm install express
    vim index.js
    ```
    
    把以下內容貼上 index.js(儲存離開 :wq)
    
    ```jsx
    const express = require('express')
    const app = express()
    const port = 3000
    
    app.get('/', (req, res) => {
       res.send('Hello World!')
    })
    
    app.listen(port, () => {
       console.log(`Example app listening at http://localhost:${port}`)
    })
    ```
    
    執行
    
    ```bash
    node index.js
    ```
    
    以上看起來是成功了，接著把剛剛的步驟寫成 Dockerfile
    
    ```docker
    # 第二版
    
    # 基底 Image
    FROM ubuntu
    
    ENV DEBIAN_FRONTEND=noninteractive
    
    # 安裝套件
    RUN apt-get update -y \
        && apt-get install nodejs npm -y
    
    WORKDIR app
    
    RUN npm init -y \
    		&& npm install express
    
    # 把編輯好的index.js放在跟Dockerfile同資料夾下
    ADD app/index.js .
    
    # 開放對外的 Port
    EXPOSE 3000
    
    # 容器啟動後執行的命令
    CMD ["node", "index.js"]
    ```
    
    建置 Image，並建立Container
    
    ```bash
    docker build -t my-image-2 .
    docker run -d -p 3000:3000 --name my-container-2  my-image-2
    ```
    
    瀏覽器查看 http://localhost:3000，成功得到回應
    
    - 如何瀏覽Http網址
        
        chrome://net-internals/#hsts
        
        {% asset_img Untitled_6.png %}
        
- 練習：
    
    實際操作，並嘗試縮減建置出來的 Image 大小
    
    {% asset_img Untitled_7.png %}
    
    ```docker
    # 第三版
    
    # 基底 Image
    FROM node:alpine
    
    WORKDIR app
    
    RUN npm init -y \
        && npm install express
    
    ADD app/index.js .
    
    # 開放對外的 Port
    EXPOSE 3000
    
    # 容器啟動後執行的命令
    CMD ["node", "index.js"]
    ```
    
    {% asset_img Untitled_8.png %}
    

### 動手製作 docker-compose.yml (帶實作)

以上是只有一個 Container 的狀況，用指令操作還可以接受。但通常一個應用系統會需要多個 Container，每個 Container 要跑起來都要打一串指令，加一堆參數，誰受的了。

所以我們需要 docker-compose，可以用單一文件設定管理多個 Container。甚至我建議不管幾個 Container，都直接寫成 docker-compose！

- Step4
    
    將資料夾結構調整成
    
    {% asset_img Untitled_9.png %}
    
    ```docker
    # 第四版
    
    # 基底 Image
    FROM node:alpine
    
    WORKDIR app
    
    # 開放對外的 Port
    EXPOSE 3000
    
    # 容器啟動後執行的命令
    CMD ["tail", "-f", "/dev/null"]
    ```
    
    ```yaml
    version: '3'
    
    services:
      express:
        build: ./express
        container_name: ${APP_NAME}_express
        ports: 
          - "3000:3000"
        entrypoint: npm run start
        volumes:
          - ./express/app:/app
    ```
    
    ```
    APP_NAME=MyAPP
    ```
    
    ```json
    {
        "name": "test",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "scripts": {
          "start": "npm install && nodemon index.js"
        },
        "keywords": [],
        "author": "",
        "license": "ISC",
        "dependencies": {
          "express": "^4.19.2",
          "nodemon": "^3.1.2"
        }
    }
    ```
    
    然後一個指令就能建置 Image，並把 Container 跑起來
    
    ```yaml
    docker-compose up
    ```
    
    同樣一個指令就能把所有的 Container 停掉
    
    ```yaml
    docker-compose down
    ```
    
    以下說明所有的更動
    
    1. Dockerfile 一般來說不需自己從頭開始撰寫所有步驟，可以到 DockerHub 找別人已經包好的 Image，尤其是官方的 Image 優先使用。
    
    本 Container 只需要 nodejs 執行環境，所以從 DockerHub 找 node 的官方 Image，使用 alpine 版本是因為它是輕量的 Linux 發行版。
    
    另外把 npm install 跟 複製 index.js 拿掉，讓這個 Image 只是單純的 node 執行環境。
    2. docker-compose.yml 檔案用來描述所有的服務與設定。
        - 指定服務名稱，container名稱
        - 可讀取.env的變數內容
        - entrypoint是容器跑起來後要執行的指令，也就是程式進入點，這邊的設定會覆寫Dockerfile中的CMD設定
        - volume是把本機的路徑映射到容器中，這樣程式碼的部分可以不用打包到Image裡面，而且容器被刪掉的話，資料也可以持續保存在本機
- Step5
    
    加入資料庫，這邊使用Postgres
    
    ```bash
    # 先建立postgres的資料夾
    mkdir -p postgres/data
    ```
    
    ```yaml
    services:
      express:
        ...
      postgres:
        image: postgres
        container_name: ${APP_NAME}_postgres
        restart: always
        user: "1000"  # WSL Only
        ports: 
          - "5442:5432"
        volumes:
          - ./postgres/data:/var/lib/postgresql/data
        environment:
          POSTGRES_USER: ${DB_USERNAME}
          POSTGRES_PASSWORD: ${DB_PASSWORD}
          POSTGRES_DB: ${DB_DATABASE}
    ```
    
- Step6
    
    加入資料庫管理介面工具，pgAdmin，這樣就能連線進去新增資料表與資料。
    
    ```bash
    # 先建立pgadmin的資料夾
    mkdir pgadmin
    ```
    
    ```yaml
    services:
      express:
        ...
      postgres:
        ...
      pgadmin:
        image: dpage/pgadmin4
        container_name: ${APP_NAME}_pgadmin
        restart: "always"
        user: root
        environment:
          PGADMIN_DEFAULT_EMAIL: ${PGADMIN_USERNAME}
          PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_PASSWORD}
        volumes:
          - ./pgadmin:/var/lib/pgadmin
        ports:
          - "5050:80"
        depends_on:
          - postgres
    ```
    
    - pgadmin相依（depends_on）於postgres，這樣啟動container時，會在postgres成功啟動後，才會接續啟動pgadmin
    - 當沒有指定network時，container會預設使用default的網路，drive是bridge，而postgres預設expose是5432port，所以pgadmin可以直接連得到
    
    ```bash
     docker network ls
     docker network inspect [NETWORK_NAME]
    ```
    
    - ports跟expose的區別
- Step7
    
    設定express連線postgres，讀取資料顯示出來。
    
    ```yaml
    services:
      express:
        ...
        depends_on:
          - postgres
        environment:
          - NODE_ENV=${NODE_ENV}
          - DB_USERNAME=${DB_USERNAME}
          - DB_PASSWORD=${DB_PASSWORD}
          - DB_DATABASE=${DB_DATABASE}
    ```
    
    ```jsx
    require('dotenv').config();
    const { Pool } = require('pg');
    
    const pool = new Pool({
        user: process.env.DB_USERNAME,
        password: process.env.DB_PASSWORD,
        host: 'postgres',
        port: 5432, // default Postgres port
        database: process.env.DB_DATABASE
    });
    
    module.exports = {
        query: (text, params) => pool.query(text, params)
    };
    ```
    
    - db.js 是 node連結資料庫的方式，注意 host: postgres，這邊無法直接填ip，因為服務不會知道其他服務的ip，而是要直接填服務名稱！
    - 資料庫連線字串參數可用environment帶入container中，這樣就能在最上層的.env統一管理環境變數
- Step9
    
    雖然可以用預設的default network很方便，但有時會想隔離container或做更細節的設定。
    
    Docker可以用network來描述網路模型，network有許多驅動模式，目前先使用bridge模式。
    
    ```yaml
    services:
      express:
    		...
        networks:
          - app-network
    	postgres:
        ...
        networks:
          - app-network
          
    networks:
      app-network:
        driver: bridge
    ```
    
    將container指定要使用的network，同樣network之間就可以互相連結得到。
    
    ```bash
    docker network ls
    
    docker network inspect [NetworkName]
    ```
    
- 練習
    
    延伸Step9，新開一台資料庫服務，使網頁同時讀取兩個ㄌ藥庫的資料，但兩個資料庫服務彼此是隔離的。
    
- Step10
    
    ```bash
    docker-compose up
    docker-compose up -d
    
    sudo ls -al /var/lib/docker/containers
    sudo cat /var/lib/docker/containers/[CONTAINER_ID]/[CONTAINER_ID]-json.log
    
    docker-compose logs express
    ```
    
- Step13
    
    docker-compose.yml 檔案可以切分成
    
    {% asset_img Untitled_10.png %}
    
    ```bash
    services:
    
      express:
        build: ./express
        container_name: ${APP_NAME}_express
        # ports: defined in [environment].yml
        # entrypoint: defined in [environment].yml
        volumes:
          - ./express/app:/app
        depends_on:
          - postgres
        networks:
          - app-network
    ```
    
    ```bash
    services:
    
      express:
        ports:
          - "3000:3000"
          - "9229:9229"
        entrypoint: sh -c "npm install && npm run debug" 
    ```
    
    ```bash
    services:
    
      express:
        ports:
          - "80:3000"
        entrypoint: sh -c "npm install && npm run start" 
    ```
    
    這樣就能依環境切換不同的設定檔
    
    ```bash
    # 開發環境
    # 預設都不指定-f的話，就會抓docker-compose.yml跟docker-compose.override.yml
    docker-compose up
    
    # 正式環境
    docker-compose -f docker-compose.yml -f docker-compose.production.yml up
    ```
    

### 常用連結

- [Dockerfile reference | Docker Docs](https://docs.docker.com/reference/dockerfile/)
- [docker/awesome-compose: Awesome Docker Compose samples (github.com)](https://github.com/docker/awesome-compose)

### 疑難排解

```bash
# Installing, this may take a few minutes...
# WslRegisterDistribution failed with error: 0x800701bc
# Error: 0x800701bc WSL 2 ???????????????????? visit https://aka.ms/wsl2kernel
請參考這篇的解法
https://blog.darkthread.net/blog/wsl2-gui/

# docker: Error response from daemon: driver failed programming external connectivity on endpoint test-nginx (8c9ad828c53e7b9be16d44bb032b7dab46c9fea745b1549c64e03374e967cfaa):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 8099 -j DNAT --to-destination 172.17.0.2:80 ! -i docker0: iptables: No chain/target/match by that name. (exit status 1)).
sudo iptables -t filter -F
sudo iptables -t filter -X
sudo systemctl restart docker
```