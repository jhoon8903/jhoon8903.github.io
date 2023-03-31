---
title: rpm Package 생성
category: test
author: "이정훈"
tags: [rpm, test, 자동화]
img : ":RPM_Logo.svg.png"
comments_disable: true
meta_description: "rpm 패키지 생성"
---

CentOS 기준
```linux
sudo yum install -y rpm-build rpmdevtools
rpmdev-setuptree
cd ~/rpmbuild
nano SPECS/my-packages.spec
```

```
Name: db-packages

Version: 1.1

Release: 1%{?dist}

Summary: Custom package for installing multiple packages

License: GPL

%description

This package install multiple packages with a single command. database set postgresql14 and monitoring

%prep

# Naver CentOS7

export LC_ALL=en_US.UTF-8

  

%build

  

%install

wget https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

sudo yum install -y centos-release-scl-rh

sudo yum install -y epel-release

sudo yum install -y dnf

sudo dnf install -y llvm-toolset-7-clang

sudo dnf install -y llvm9.0-devel

sudo dnf install -y postgresql14

sudo dnf install -y postgresql14-libs

sudo dnf install -y postgresql14-devel

sudo dnf install -y postgresql14-llvmjit

sudo dnf install -y postgresql14-contrib

sudo dnf install -y postgresql14-tcl

sudo dnf install -y postgresql14-server

wget -O /tmp/netdata-kickstart.sh https://my-netdata.io/kickstart.sh && sh /tmp/netdata-kickstart.sh

sudo yum install -y htop

sudo yum install -y repmgr_14.x86_64 repmgr_14-devel.x86_64

sudo dnf install -y pgpool-II

%files

%changelog
```

# 설치
제작한 Custom Package
```
sudo wget https://github.com/jhoon8903/packageRepo/raw/main/db-packages-1.1-1.el7.x86_64.rpm
sudo yum install -y db-packages-1.1-1.el7.x86_64.rpm
```