---
title: 理解 LLVM 架构
date: 2023-07-18 11:14:51
categories: Intern
tags:
  - intern
description: 
---
<p style="opacity: 0.7;"> Start Notes: 

<small style="opacity: 0.7;"> </small>

---

### llvm 安装调试

```
apt install llvm
apt install clang
apt install build-essential
apt install zip
apt install cmake
apt install python3
unzip llvm-project-main.zip
mkdir llvm
cd llvm-project-main
mkdir build
cd build
cmake -DLLVM_ENABLE_PROJECTS="clang;lldb" -DCMAKE_BUILD_TYPE="Debug" -DCMAKE_INSTALL_PREFIX="/usr/local/llvm" ../llvm
make
```

```
root@LAPTOP-BFFKBFTN:~/llvm-project-llvmorg-16.0.6# cmake -S llvm -B build -G Ninja -DCMAKE_BUILD_TYPE=Debug
root@LAPTOP-BFFKBFTN:~/llvm-project-llvmorg-16.0.6# cmake -DLLVM_ENABLE_PROJECTS="clang;lldb" -DCMAKE_BUILD_TYPE="Debug" -DCMAKE_INSTALL_PREFIX="/usr/local/llvm" llvm
root@LAPTOP-BFFKBFTN:~/llvm-project-llvmorg-16.0.6# make
```

---

### Kaleidoscope + LLVM

[教程](https://www.llvm.org/docs/tutorial/MyFirstLanguageFrontend/index.html)

运行：

```
clang++ -g -O3 toy.cpp `llvm-config --cxxflags --ldflags --system-libs --libs core` -o toy
./toy
```

---

### CTPG (Compile Time Parser Generator) 

[GitHub Repo](https://github.com/peter-winter/ctpg)

运行：

```
g++ test_ctpg.cpp -std=c++17 -o test_stpg
./test_ctpg "10, 20, 30"
```

---

### License 许可证


---

### 
