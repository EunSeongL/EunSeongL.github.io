---
title: "Day4"
date: "2025-08-11"
thumbnail: "/assets/img/CPU/cpu_thumb.png"
---

# DAY4
- CPU(Central Processing Unit) 중앙처리장치
---

## ✅ CISC vs RISC

- **CISC vs RISC**
   - CISC(Complex Instruction Set Computer)
   - RISC(Reduced Instruction Set Computer)

| 특징 (Feature) | RISC (축소 명령어 집합 컴퓨터) | CISC (복잡 명령어 집합 컴퓨터) |
| :--- | :--- | :--- |
| **명령어** | 적고 단순, 길이가 **고정** | 많고 복잡, 길이가 **가변적** |
| **실행 속도 (CPI)** | 한 클럭에 한 명령어 처리를 지향 (CPI ≈ 1) | 한 명령어가 여러 클럭을 소모 (CPI > 1) |
| **설계 복잡성** | 하드웨어는 단순, 소프트웨어(컴파일러)가 복잡 | 하드웨어가 복잡, 소프트웨어(컴파일러)는 단순 |
| **메모리 접근** | `LOAD`, `STORE` 등 전용 명령어로만 접근 | 다양한 명령어가 직접 메모리에 접근 가능 |
| **레지스터** | 범용 레지스터가 **많고** 활용도 높음 | 범용 레지스터 수가 비교적 **적음** |
| **파이프라이닝** | 구조가 단순하여 파이프라이닝에 **매우 효율적** | 명령어가 복잡하고 길이가 달라 파이프라이닝이 **복잡함** |
| **전력 소비** | 일반적으로 전력 소비가 **적음** | 일반적으로 전력 소비가 **많음** |
| **가격** | CISC보다 가격이 **저렴** | 트랜지스터가 많이 들어가기 때문에 가격이 **비쌈** |

- **MIPS(Microprocessor without Interlocked Pipeline Stages)**
   - 파이프라인 기술을 효율적으로 사용해 프로세서의 성능을 높이는 데에 초점을 맞춘 설계 방식
   - 고정 길이의 단순한 명령어
   - 파이프라인(Pipelining) 최적화
      - 파이프라인은 명령어 처리 과정을 '명령어 인출(IF) → 해석(ID) → 실행(EX) → 메모리 접근(MEM) → 결과 저장(WB)'의 5단계   
   - 로드-스토어 (Load-Store) 구조
   - 많은 수의 범용 레지스터

---

## ✅ 폰노이만 구조 vs 하버드 구조

![alt text](../../../../assets/img/CPU/von.png)

- 폰노이만 구조


![alt text](../../../../assets/img/CPU/har.png)

- 하버드 구조 

---

## ✅ RISC-V

- UC 버클리에서 개발중인 무료 오픈 소스 RISC 명령어셋 아키텍처
- MIPS 구조와 거의 비슷하다.

## ✅ 목표

- 1. Single Cycle Processor
  - 모든 명령어가 1clock 내에 실행 
- 2. Multi Cycle Processor
  - 명령어 종류에 따라 실행 clock수가 다르다.
- 3. pipe-line 구조 CPU

---

## ✅ Dedicated Processor Counter

- 0~9까지 카운트하는 Processor를 설계하시오. 

```c
// C언어 관점
A = 0;
while (A < 10){
   output = A;
   A = A + 1;
}
halt;
// A를 중심에 놓고 생각
// A를 하드웨어적으로 구현한다 생각하면 -> A 레지스터
```

| Block Diagram | State Machine |
| :---: | :---: |
| <img src="/assets/img/CPU/dedicnt2.png" alt="Block Diagram" style="width:50%; object-fit:contain;"> | <img src="/assets/img/CPU/dedicnt.png" alt="State Machine" style="width:50%; object-fit:contain;"> |

<img src="/assets/img/CPU/deditop.png" style="width:50%; object-fit:contain;">

---

## ✅ Dedicated Processor Adder

- 0~10까지 누적으로 더하는 Dedicated Processor를 설계하시오.

#### **C 구현**

```c
// C언어 관점
A = 0;
SUM = 0;
while (A < 11){
   SUM = SUM + A;
   A = A + 1;
   output = SUM;
}
halt;
```

#### **DataPath 구조 설계**

<img src="/assets/img/CPU/dpadder.png" style="width:75%; object-fit:contain;">

---

#### **ASM chart -> Control Unit 설계**

![alt text](../../../../assets/img/CPU/addsig.png)

<img src="/assets/img/CPU/addasm.png" style="width:75%; object-fit:contain;">

### **코드**
---
#### DedicatedProcessor_Adder.sv

```verilog
`timescale 1ns / 1ps

module DedicatedProcessor_Adder(
    input  logic        clk,
    input  logic        reset,
    output logic [ 7:0] OutPort
    //output logic [ 3:0] fndCom,
    //output logic [ 7:0] fndFont
    );

    logic       SumSrcMuxSel;
    logic       ISrcMuxSel;
    logic       AdderSrcMuxSel;  
    logic       SumEn;
    logic       IEn;
    logic       ILe10;
    logic       OutPortEn;

    DataPath U_DataPath (
        .clk            (clk),
        .reset          (reset),
        .SumSrcMuxSel   (SumSrcMuxSel),
        .ISrcMuxSel     (ISrcMuxSel),
        .AdderSrcMuxSel (AdderSrcMuxSel),     
        .SumEn          (SumEn),
        .IEn            (IEn),
        .ILe10          (ILe10),
        .OutPortEn      (OutPortEn),
        .OutPort        (OutPort)
    );

    ControlUnit U_ControlUnit (
        .clk            (clk),
        .reset          (reset),
        .ILe10          (ILe10),
        .SumSrcMuxSel   (SumSrcMuxSel),
        .ISrcMuxSel     (ISrcMuxSel),
        .AdderSrcMuxSel (AdderSrcMuxSel),     
        .SumEn          (SumEn),
        .IEn            (IEn),
        .OutPortEn      (OutPortEn)
    );

    endmodule
```

---

#### DataPath.sv

```verilog
`timescale 1ns / 1ps

module DataPath(
        input  logic       clk,
        input  logic       reset,
        input  logic       SumSrcMuxSel,
        input  logic       ISrcMuxSel,
        input  logic       AdderSrcMuxSel,     
        input  logic       SumEn,
        input  logic       IEn,
        output logic       ILe10,
        input  logic       OutPortEn,
        output logic [7:0] OutPort 
    );

    logic [7:0] SumSrcMuxOut, ISrcMuxOut;
    logic [7:0] SumRegOut, IRegOut;
    logic [7:0] AdderResult, AdderSrcMuxOut;

    mux_2X1 U_SumSrcMux (
        .sel  (SumSrcMuxSel),
        .x0   (0),
        .x1   (AdderResult),
        .y    (SumSrcMuxOut)
    );

    mux_2X1 U_ISrcMux (
        .sel  (ISrcMuxSel),
        .x0   (0),
        .x1   (AdderResult),
        .y    (ISrcMuxOut)
    );

    register U_SUM_REG (
        .clk    (clk),
        .reset  (reset),
        .en     (SumEn),
        .d      (SumSrcMuxOut),
        .q      (SumRegOut)
    );

    register U_I_Reg (
        .clk    (clk),
        .reset  (reset),
        .en     (IEn),
        .d      (ISrcMuxOut),
        .q      (IRegOut)
    );

    comparator U_ILe10 (
        .a      (IRegOut),
        .b      (8'd10),
        .lt     (ILe10)
    );

    mux_2X1 U_AdderSrcMux (
        .sel  (AdderSrcMuxSel),
        .x0   (SumRegOut),
        .x1   (1),
        .y    (AdderSrcMuxOut)
    );

    adder U_Adder (
        .a      (AdderSrcMuxOut),
        .b      (IRegOut),
        .sum    (AdderResult)    
    );

    register U_OutPort (
        .clk    (clk),
        .reset  (reset),
        .en     (OutPortEn),
        .d      (SumRegOut),
        .q      (OutPort)
    );
    
    endmodule
```

---

#### ControlUnit.sv

```verilog
`timescale 1ns / 1ps

module ControlUnit(
    input  logic       clk,
    input  logic       reset,
    input  logic       ILe10,
    output logic       SumSrcMuxSel,
    output logic       ISrcMuxSel,
    output logic       AdderSrcMuxSel,     
    output logic       SumEn,
    output logic       IEn,
    output logic       OutPortEn
    );

    typedef enum {
        S0,
        S1, 
        S2, 
        S3, 
        S4,
        S5  
    } state_e;

    state_e state, next_state;

    always_ff @(posedge clk or posedge reset) begin
        if(reset) begin
            state <= S0;
        end
        else begin
            state <= next_state;
        end
    end

    always_comb begin
        next_state = state;
        SumSrcMuxSel   = 0;
        ISrcMuxSel     = 0;
        SumEn          = 0;
        IEn            = 0;
        AdderSrcMuxSel = 0;
        OutPortEn      = 0;
        case (state)
            S0:begin
                SumSrcMuxSel   = 0;
                ISrcMuxSel     = 0;
                SumEn          = 1;
                IEn            = 1;
                AdderSrcMuxSel = 0;
                OutPortEn      = 0;
                next_state     = S1;
            end 
            S1:begin
                SumSrcMuxSel   = 0;
                ISrcMuxSel     = 0;
                SumEn          = 0;
                IEn            = 0;
                AdderSrcMuxSel = 0;
                OutPortEn      = 0;
                if(ILe10)  next_state = S2;
                else       next_state = S5;
            end  
            S2:begin
                SumSrcMuxSel   = 1;
                ISrcMuxSel     = 1;
                SumEn          = 1;
                IEn            = 0;
                AdderSrcMuxSel = 0;
                OutPortEn      = 0;
                next_state     = S3;
            end  
            S3:begin
                SumSrcMuxSel   = 1;
                ISrcMuxSel     = 1;
                SumEn          = 0;
                IEn            = 1;
                AdderSrcMuxSel = 1;
                OutPortEn      = 0;
                next_state     = S4;
            end  
            S4:begin
                SumSrcMuxSel   = 1;
                ISrcMuxSel     = 1;
                SumEn          = 0;
                IEn            = 0;
                AdderSrcMuxSel = 0;
                OutPortEn      = 1;
                next_state     = S1;
            end
            S5:begin
                SumSrcMuxSel   = 1;
                ISrcMuxSel     = 1;
                SumEn          = 0;
                IEn            = 0;
                AdderSrcMuxSel = 0;
                OutPortEn      = 0;
                next_state     = S5;
            end    
        endcase
    end

    endmodule

```

---

#### TestBench

```verilog
`timescale 1ns / 1ps

module tb_DedicatedProcessor_Adder ();

    logic       clk;
    logic       reset;
    logic [3:0] fndCom;
    logic [7:0] fndFont;
    
    DedicatedProcessor_Adder U_DedicatedProcessor_Adder (.*);

    always #5 clk = ~clk;

    initial begin
        clk = 0;
        reset = 1;
        #10;
        reset = 0;
    end
    
endmodule
```

---
### **시뮬레이션**
<img src="/assets/img/CPU/dediaddersim.png" style="width:100%; object-fit:contain;">

---
### **동작 영상**
