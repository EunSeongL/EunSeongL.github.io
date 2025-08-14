---
title: "Day6"
tags:
    - Theory
    - Study
    - CPU Design
date: "2025-08-14"
thumbnail: "/assets/img/CPU/cpu_thumb.png"
bookmark: true
---

# DAY6

---

## ✅ RISC-V
> UC버클리에서 개발중인 무료 오픈소스 RISC 명령어셋 아키텍처

<img src="/assets/img/CPU/regstate.png" style="width:50%; object-fit:contain;">

<img src="/assets/img/CPU/instr.png" style="width:50%; object-fit:contain;">

```
명령어 길이가 모두 32bit로 동일
```

## ✅ Single-Cycle Architecture

- 특징 : 모든 명령어가 (1CLK) 내에 동작.
- 장점 : 구조가 매우 simple하다.
- 단점 : 한 클럭 내에 동작해야되기 때문에 느리다.

## ✅ Multi-Cycle Architecture

- 특징 : 명령어 Type별 동작 CLK 수가 다르다. 
- 장점 : Single-Cycle보다는 **조금** 빠르다.
- 단점 : Single-Cycle보다 구조가 **조금** 복잡하다.

## ✅ Pipe-Line Architecture

- 특징 : 모든 명령어가 (1CLK) 내에 동작. 
- 장점 : Single-Cycle보다 **많이** 빠르다.
- 단점 : Single-Cycle보다 구조가 **많이** 복잡하다.

## ✅ CPU 기본 모듈 (하버드 구조)
- RegisterFile
- ALU
- ROM/Flash(Instruction Memory)
- RAM(DataMemory)
- PC(Program Counter)

<img src="/assets/img/CPU/mcu.png" style="width:50%; object-fit:contain;">


## ⚒️ 코드 ⚒️

