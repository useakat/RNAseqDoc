---
title: "RNA-seq 解析環境の構築"
linkTitle: "解析環境の構築"
weight: 1
description: >-
     RNA-seq 解析に必要なツールを AWS EC2 にインストールする方法について説明
---

## launch an instance in EC2
ubuntu 22

## configure the instance
root で作業する: sudo su -

1. configure Ubuntu
    1. sudo apt update

2. install latest R
    1. export R_VERSION=4.3.3
    2. curl -O https://cdn.rstudio.com/r/ubuntu-2204/pkgs/r-${R_VERSION}_1_amd64.deb
    3. sudo gdebi r-${R_VERSION}_1_amd64.deb
    4. sudo ln -s /opt/R/${R_VERSION}/bin/R /usr/local/bin/R
    5. sudo ln -s /opt/R/${R_VERSION}/bin/Rscript /usr/local/bin/Rscript

(
    1. sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
    2. sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    3. sudo apt update
    2. sudo apt install r-base
    3. sudo R
    4. update.packages()
)

3. install libssl
    1. wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.0g-2ubuntu4_amd64.deb
    2. sudo dpkg -i libssl1.1_1.1.0g-2ubuntu4_amd64.deb

3. install Rstudio
    1. sudo apt-get install gdebi-core
    2. wget https://download2.rstudio.org/server/jammy/amd64/rstudio-server-2022.07.1-554-amd64.deb
    3. sudo gdebi rstudio-server-2022.07.1-554-amd64.deb
(Rstudio server のアンインストール: sudo apt-get remove --purge rstudio-server)

4. set a port for Rstudio
    1. /etc/rstudio/rserver.conf の１行目に www-port=1234 (1234は好きなポート番号）と追加する
    2. sudo systemctl restart rstudio-server.service

5. ユーザーパスワードの設定
sudo passwd ユーザー名

## インストール libcurl
sudo apt install libcurl4-openssl-dev

## TCC-GUI のインストール
1. git clone https://github.com/swsoyee/TCC-GUI.git ~/TCC-GUI
2. login to Rstudio-server with IPaddress:ポート番号
3.  double click TCC-GUI.Rproj in TCC-GUI folder
4. install BiocManager (v3.19 for R 4.4.0)
    if (!require("BiocManager", quietly = TRUE))
  install.packages("BiocManager")
BiocManager::install(version = "3.19") 
4. renv::restore()
