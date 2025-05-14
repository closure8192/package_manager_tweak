Below is a concise, step-by-step guideline for configuring each package manager tool (including the original ones—pip, npm, Docker, RubyGems, Maven Central, Go Modules, Crates.io, Homebrew, Android SDK, Gradle—and the newly added ones—apt, yum/dnf, Composer, NuGet, pacman, Helm, Conan) to work behind the GFW in mainland China without a VPN. Each section uses a primary mirror, with a secondary mirror noted at the end, formatted in Markdown.
pip
Primary Mirror: Tsinghua University  

    Configure pip to use Tsinghua’s mirror:  
    bash

    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

    Set trusted host to avoid SSL issues:  
    bash

    pip config set global.trusted-host pypi.tuna.tsinghua.edu.cn

    Install packages:  
    bash

    pip install package-name

    Secondary Mirror: Aliyun (https://mirrors.aliyun.com/pypi/simple/)

npm
Primary Mirror: npmmirror  

    Set npm to use npmmirror.com registry (formerly known as the Taobao npm mirror):  
    bash

    npm config set registry https://registry.npmmirror.com

    Install packages:  
    bash

    npm install package-name

    Alternatively, use cnpm wherever npm is needed. To install cnpm globally using the new mirror:

    npm install -g cnpm --registry=https://registry.npmmirror.com

    Then you can install any package like:

    cnpm install express

Docker
Primary Mirror: DaoCloud  

    Edit /etc/docker/daemon.json:  
    json

    {
      "registry-mirrors": ["https://docker.m.daocloud.io"]
    }

    Reload and restart Docker:  
    bash

    sudo systemctl daemon-reload
    sudo systemctl restart docker

    Run Docker commands:  
    bash

    docker run -it ubuntu:latest

    Secondary Mirror: USTC (https://docker.mirrors.ustc.edu.cn)

RubyGems
Primary Mirror: Ruby China  

    Replace the default source:  
    bash

    gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

    Install gems:  
    bash

    gem install package-name

    Secondary Mirror: Tsinghua (https://mirrors.tuna.tsinghua.edu.cn/rubygems/)

Maven Central
Primary Mirror: Aliyun  

    Edit ~/.m2/settings.xml:  
    xml

    <mirror>
      <id>aliyunmaven</id>
      <mirrorOf>*</mirrorOf>
      <name>Aliyun Maven</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>

    Build project:  
    bash

    mvn clean install

    Secondary Mirror: USTC (check https://mirrors.ustc.edu.cn/)

Go Modules
Primary Mirror: Aliyun  

    Set GOPROXY:  
    bash

    export GOPROXY=https://mirrors.aliyun.com/goproxy/

    Install dependencies:  
    bash

    go get package-name

    Secondary Mirror: goproxy.cn (https://goproxy.cn)

Crates.io
Primary Mirror: USTC  

    Edit ~/.cargo/config.toml:  

    [source.crates-io]
    replace-with = 'ustc'
    [source.ustc]
    registry = "sparse+https://mirrors.ustc.edu.cn/crates.io-index/"

    Install crates:  
    bash

    cargo build

    Secondary Mirror: Tsinghua (sparse+https://mirrors.tuna.tsinghua.edu.cn/crates.io-index/)

Homebrew
Primary Mirror: Tsinghua University  

    Install Homebrew:  
    bash

    git clone https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/install.git
    cd install
    /bin/bash install.sh

    Replace homebrew-core:  
    bash

    git -C "$(brew --repo)/Library/Taps/homebrew/homebrew-core" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

    Replace homebrew-cask:  
    bash

    git -C "$(brew --repo)/Library/Taps/homebrew/homebrew-cask" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git

    Set bottle domain (add to ~/.zshrc):  
    bash

    export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles
    source ~/.zshrc

    Update and install:  
    bash

    brew update
    brew install package-name

    Secondary Mirror: USTC (Core: https://mirrors.ustc.edu.cn/homebrew-core.git, Cask: https://mirrors.ustc.edu.cn/homebrew-cask.git, Bottles: https://mirrors.ustc.edu.cn/homebrew-bottles)

Android SDK
Primary Mirror: Tsinghua University  

    Edit SDK Manager settings in Android Studio (Settings > Android SDK > Edit):  
        Set custom mirror:  

        https://mirrors.tuna.tsinghua.edu.cn/android/

    Update SDK:  
        Select components in SDK Manager and click Apply.

    Secondary Mirror: USTC (https://mirrors.ustc.edu.cn/android/)

Gradle
Primary Mirror: Aliyun  

    Edit project-level build.gradle:  
    gradle

    repositories {
        maven { url 'https://maven.aliyun.com/repository/public' }
        mavenCentral()
    }

    Sync and build:  
    bash

    gradle build

    Secondary Mirror: Tencent (https://mirrors.tencent.com/repository/maven-public/)

APT
Primary Mirror: Tsinghua University  

    Backup default source:  
    bash

    sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Edit /etc/apt/sources.list (e.g., Ubuntu 20.04):  
    bash

    deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
    deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
    deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

    Update and install:  
    bash

    sudo apt update
    sudo apt install package-name

    Secondary Mirror: USTC (https://mirrors.ustc.edu.cn/ubuntu/)

Yum/DNF
Primary Mirror: Tsinghua University  

    Backup default source:  
    bash

    sudo cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak

    Edit /etc/yum.repos.d/CentOS-Base.repo (e.g., CentOS 8):  
    bash

    [base]
    name=CentOS-$releasever - Base
    baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos/$releasever/os/$basearch/
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-8

    Update and install:  
    bash

    sudo dnf update
    sudo dnf install package-name

    Secondary Mirror: Aliyun (https://mirrors.aliyun.com/centos/)

Composer
Primary Mirror: Aliyun  

    Configure Composer:  
    bash

    composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

    Install packages:  
    bash

    composer install

    Secondary Mirror: Tencent (https://mirrors.tencent.com/composer/)

NuGet
Primary Mirror: Tsinghua University  

    Edit NuGet.Config (e.g., ~/.nuget/NuGet/):  
    xml

    <packageSources>
      <add key="tsinghua" value="https://mirrors.tuna.tsinghua.edu.cn/nuget/" />
    </packageSources>

    Restore packages:  
    bash

    dotnet restore

    Secondary Mirror: USTC (https://mirrors.ustc.edu.cn/nuget/)

Pacman
Primary Mirror: Tsinghua University  

    Edit /etc/pacman.d/mirrorlist:  
    bash

    Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch

    Update and install:  
    bash

    sudo pacman -Syu
    sudo pacman -S package-name

    Secondary Mirror: USTC (https://mirrors.ustc.edu.cn/archlinux/)

Helm
Primary Mirror: Aliyun  

    Add Aliyun repository:  
    bash

    helm repo add aliyun https://kubernetes.oss-cn-hangzhou.aliyuncs.com/charts

    Update and install:  
    bash

    helm repo update
    helm install my-release aliyun/chart-name

    Secondary Mirror: None widely recommended; use internal mirrors if needed.

Conan
Primary Mirror: USTC  

    Edit ~/.conan/conan.conf:  
    ini

    [remotes]
    ustc = https://mirrors.ustc.edu.cn/conan/

    Install packages:  
    bash

    conan install package-name

    Secondary Mirror: Aliyun (self-hosted, check https://mirrors.aliyun.com/)

This summary covers all the package manager tools discussed, focusing on practical steps with primary mirrors for reliability in China’s GFW environment. Let me know if you need adjustments or additional tools!