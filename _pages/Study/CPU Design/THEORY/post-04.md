---
title: "Day4"
date: "2025-08-11"
thumbnail: "/assets/img/CPU/cpu_thumb.png"
---

# DAY4

---

## ✅ CPU(Central Processing Unit) 중앙처리장치

- **CISC vs RISC**
   - CISC(Complex Instruction Set Computer)
   - RISC(Reduced Instruction Set Computer)

| 특징 (Feature) | RISC (축소 명령어 집합 컴퓨터) | CISC (복잡 명령어 집합 컴퓨터) |
| :--- | :--- | :--- |
| **명령어** | 적고 단순하며, 길이가 **고정됨** | 많고 복잡하며, 길이가 **가변적임** |
| **실행 속도 (CPI)** | 한 클럭에 한 명령어 처리를 지향 (CPI ≈ 1) | 한 명령어가 여러 클럭을 소모 (CPI > 1) |
| **설계 복잡성** | 하드웨어는 단순, 소프트웨어(컴파일러)가 복잡 | 하드웨어가 복잡, 소프트웨어(컴파일러)는 단순 |
| **메모리 접근** | `LOAD`, `STORE` 등 전용 명령어로만 접근 | 다양한 명령어가 직접 메모리에 접근 가능 |
| **레지스터** | 범용 레지스터가 **많고** 활용도가 높음 | 범용 레지스터 수가 비교적 **적음** |
| **파이프라이닝** | 구조가 단순하여 파이프라이닝에 **매우 효율적** | 명령어가 복잡하고 길이가 달라 파이프라이닝이 **복잡함** |
| **전력 소비** | 일반적으로 전력 소비가 **적음** | 일반적으로 전력 소비가 **많음** |
| **대표 CPU** | Apple Silicon (M-시리즈), Qualcomm Snapdragon | Intel Core (i-시리즈), AMD Ryzen |

---
