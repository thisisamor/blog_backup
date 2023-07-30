---
title: 理解 LLVM 架构
date: 2023-07-18 11:14:51
categories: Intern
tags:
  - intern
description: Clang++ LLVM 踩坑撞墙指南。
---
<p style="opacity: 0.7;"> Start Notes: 

<small style="opacity: 0.7;"> 目的是给ST语言写个LLVM编译器前端。</small>

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

### 笔记

一些零散的llvm学习笔记，和备忘。

opaque object that owns  **core LLVM data structures**
such as type and constant value tables.
`static std::unique_ptr<LLVMContext> TheContext;`

a helper object that allows us to generate LLVM instructions easily
We assume it has been set up to **generate code into something**
`static std::unique_ptr<IRBuilder<>> Builder;`

an LLVM construct containing **functions and global variables**
It is the **top-level structure** LLVM IR uses to contain the code. 
It owns memory for all generated IR, 
this is why the codegen() method returns **a raw Value** 
rather than a smart pointer `unique_ptr<Value>`
`static std::unique_ptr<Module> TheModule;`

tracks the values defined in the **current scope** and their LLVM representations. 
We can view it as **a symbol table** for the code.
`static std::map<std::string, Value *> NamedValues;`


```
VAR
    temperature : REAL := 0.0;         (* Input: Temperature sensor value *)
    pressure    : REAL := 0.0;         (* Input: Pressure sensor value *)
    motorStatus : BOOL := FALSE;       (* Output: Motor status *)

    (* Constants *)
    TEMP_THRESHOLD : REAL := 100.0;    (* Temperature threshold for activation *)
    PRESSURE_THRESHOLD : REAL := 50.0; (* Pressure threshold for activation *)

    (* Local variables *)
    tempAlarm : BOOL := FALSE;         (* Temperature alarm flag *)
    pressureAlarm : BOOL := FALSE;     (* Pressure alarm flag *)
END_VAR

BEGIN
    (* Check if temperature or pressure is above threshold *)
    IF temperature > TEMP_THRESHOLD THEN
        tempAlarm := TRUE;
    ELSE
        tempAlarm := FALSE;
    END_IF;

    IF pressure > PRESSURE_THRESHOLD THEN
        pressureAlarm := TRUE;
    ELSE
        pressureAlarm := FALSE;
    END_IF;

    (* Activate the motor only if both temperature and pressure alarms are true *)
    IF tempAlarm AND pressureAlarm THEN
        motorStatus := TRUE;
    ELSE
        motorStatus := FALSE;
    END_IF;
END
```


`;` in IR means comment

```
; ModuleID = 'FactorialCalculator'

define i32 @CalculateFactorial(i32 %num) {
entry:
  %cmp = icmp sle i32 %num, 1
  br i1 %cmp, label %if.end, label %if.then

if.then:                                          ; <label>:4:10
  ret i32 1

if.end:                                          ; <label>:6:3
  %sub = sub i32 %num, 1
  %call = call i32 @CalculateFactorial(i32 %sub)
  %mult = mul i32 %num, %call
  ret i32 %mult
}

define i32 @main() {
entry:
  %call = call i32 @CalculateFactorial(i32 5)   ; CalculateFactorial(5)
  ret i32 %call
}
```


By including the implementation of the codegen function inside the class definition, you no longer need to use the override keyword. The override keyword is used to explicitly indicate that a member function is intended to override a virtual function from a base class, but since you have provided the function definition directly in the class, it is implicitly understood as overriding the virtual function.

```
llvm-config --version --cxxflags -ldflags --system-libs --libs core
```


// ------ ORIGINAL PARSER ------
// constexpr parser p(
//     expr, 
//     terms(t_var, t_real, t_end_var, variable, ':', ';', number, o_plus, o_minus, o_mul, o_div, '(', ')'),
//     nterms(expr, decl),
//     rules(
//         decl(variable, ':', t_real, ';') >= [](const auto& w1, const auto& w2, const auto& w3, const auto& w4){return declarator(w1.get_value()); }, 
//         expr(expr, t_end_var) >= [](const auto& w1, const auto& w2){return w1; }, 
//         expr(t_var, decl) >= [](const auto& w1, const auto& w2){return w2; }, 
//         expr(expr, decl) >= [](const auto& w1, const auto& w2){return w2; }, 
//         expr(expr, o_plus, expr) >= c,
//         expr(expr, o_minus, expr) >= c,
//         expr(expr, o_mul, expr) >= c,
//         expr(expr, o_div, expr) >= c,
//         expr(o_minus, expr)[3] >= c,
//         expr('(', expr, ')') >= _e2, 
//         expr(number)
//     )
// );

// ------- PRIMARY NUMBER ---------
// constexpr parser p(
//     primary, 
//     terms(number),
//     nterms(primary),
//     rules(
//         primary(number) >= [](const auto& w1){ return std::make_shared<Number>(w1.get_value()); }
//     )
// );

---

### compiling

IR -> assembly code
assembly -> executable

```
llc ir1.ll -o ir1.s
clang++ ir1.s -o executable
```
