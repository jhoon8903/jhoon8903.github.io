---
layout: post
title: Naver Cloud postgresql-14 Install
subtitle: 네이버 클라우드 인스턴스를 이용한 postgresql 설치
categories: Project
author: Daniel
tags:
 - Database
 - Cloud
 - Project
 - OpenSource
 - Tooltip
banner:
 - image : assets/images/posts/postglogo.png
---

CentOS 7.8 기준
--

## postgresql 14 Install

제작한 Custom Package
```
sudo wget https://github.com/jhoon8903/packageRepo/raw/main/db-packages-1.1-1.el7.x86_64.rpm
sudo yum install -y db-packages-1.1-1.el7.x86_64.rpm
```

```
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```
```
sudo yum update -y
```
```
sudo yum install dnf centos-release-scl-rh pel-release
```
```
sudo dnf isntall 
```
```
sudo dnf update
```
```
yum install -y  llvm-toolset-7-clang llvm9.0-devel
```
```
dnf install -y postgresql14 postgresql14-libs postgresql14-devel postgresql14-llvmjit postgresql14-contrib postgresql14-tcl postgresql14-server
```

## pg pool-II Install
```
sudo yum update -y
```
```
sudo yum isntall -y dnf
```
```
sudo dnf update
```
```
sudo dnf install -y yum 
```
