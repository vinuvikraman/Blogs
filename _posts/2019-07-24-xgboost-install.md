---
layout: post
title: "XGBoost multicore installation" 
date: 2019-07-24 13:25:00
categories: jekyll update
---

Probably you may have already known how to install multicore xgboost using clang instead of gcc. If someone don't know that please read on. I have a quad core laptop with High Sierra. When I install xgboost with conda or pip it runs only in one core. I read somewhere that conda installation will not be doing multicore processing. So I tried the source installation as described in the xgboost document

1. brew install gcc@8
1. git clone --recursive https://github.com/dmlc/xgboost
1. mkdir build
1. cd build
1. CC=gcc-8 CXX=g++-8 cmake ..
1. make -j4

However, gcc doesn't support the option  '-stdlib=libc++'. There is an issue with gcc which can be seen in this [post](https://tinyurl.com/yyrnluqp) (Note that if some build gcc as described in that post will be able to install xgboost without any problem). So I did the following with clang

1. brew install llvm

    This install clang\* at /usr/local/Cellar/llvm/VERSION

2. export PATH=/usr/local/Cellar/llvm/VERSION/bin:$PATH
3. export LDFLAGS="-L/usr/local/Cellar/llvm/VERSION/lib"
4. export CPPFLAGS="-I/usr/local/Cellar/llvm/VERSION/include"
5. git clone --recursive https://github.com/dmlc/xgboost
6. cd xgboost
7. mkdir build
8. cd build
9. CC=clang-mp-8.0 CXX=clang++-mp-8.0 cmake ..

    Note that I have replaced gcc-8 and g++-8 to clang-mp-8.0 and clang++-mp-8.0. It doesn't work with clang-8.0 and clang++-8.0

10. make -j4
11. cd ../python\_package
12. python setup.py install

These steps run xgboost in multicore.

