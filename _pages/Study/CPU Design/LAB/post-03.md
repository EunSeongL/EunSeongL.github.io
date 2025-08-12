---
title: "HomeWork"
date: "2025-08-12"
thumbnail: "/assets/img/CPU/cpu_thumb.png"
---

# DAY3

---

## ✅ 과제

<img src="/assets/img/CPU/hw.png" style="width:50%; object-fit:contain;">

<img src="/assets/img/CPU/hw2.png" style="width:50%; object-fit:contain;">

---

## ✅ 코드

### DedicatedProcessor_ALUOP.sv

```verilog
`timescale 1ns / 1ps

module DedicatedProcessor_ALUOP(
    input  logic        clk,
    input  logic        reset,
    output logic [7:0]  OutPort
    );

    logic       RFSrcMuxSel;
    logic [2:0] RAddr1;
    logic [2:0] RAddr2;
    logic [2:0] WAddr;
    logic [1:0] AluOpMuxSel;
    logic       we;
    logic       Lt;
    logic       OutPortEn;

    DataPath U_DataPath (
        .clk        (clk),
        .reset      (reset),
        .RFSrcMuxSel(RFSrcMuxSel),
        .RAddr1     (RAddr1),
        .RAddr2     (RAddr2),
        .WAddr      (WAddr),
        .AluOpMuxSel(AluOpMuxSel),
        .we         (we),
        .Lt         (Lt),
        .OutPortEn  (OutPortEn),
        .OutPort    (OutPort)
    );

    ControlUnit U_ControlUnit (
        .clk        (clk),
        .reset      (reset),
        .RFSrcMuxSel(RFSrcMuxSel),
        .RAddr1     (RAddr1),
        .RAddr2     (RAddr2),
        .WAddr      (WAddr),
        .AluOpMuxSel(AluOpMuxSel),
        .we         (we),
        .Lt         (Lt),
        .OutPortEn  (OutPortEn)
    );

endmodule
```

### DataPath.sv

```verilog
`timescale 1ns / 1ps

module DataPath (
    input  logic       clk,
    input  logic       reset,
    input  logic       RFSrcMuxSel,
    input  logic [2:0] RAddr1,
    input  logic [2:0] RAddr2,
    input  logic [2:0] WAddr,
    input  logic [1:0] AluOpMuxSel,
    input  logic       we,
    output logic       Lt,
    input  logic       OutPortEn,
    output logic [7:0] OutPort
);

    logic [7:0] AdderResult, RFSrcMuxOut;
    logic [7:0] RData1, RData2;

    mux_2x1 U_RFSrcMux (
        .sel(RFSrcMuxSel),
        .x0 (AdderResult),
        .x1 (1),
        .y  (RFSrcMuxOut)
    );

    RegFile U_RegFile (
        .clk   (clk),
        .RAddr1(RAddr1),
        .RAddr2(RAddr2),
        .WAddr (WAddr),
        .we    (we),
        .WData (RFSrcMuxOut),
        .RData1(RData1),
        .RData2(RData2)
    );

    comparator U_comparator (
        .a      (RData1),
        .b      (RData2),
        .lt     (Lt)
    );

    alu_op U_ALU_OP (
        .x0         (RData1),
        .x1         (RData2),
        .AluOpMuxSel(AluOpMuxSel),
        .y          (AdderResult)
    );

    register U_OutPort (
        .clk  (clk),
        .reset(reset),
        .en   (OutPortEn),
        .d    (RData1),
        .q    (OutPort)
    );

endmodule
```

### alu_op 추가

```verilog
module alu_op (
    input  logic [7:0] x0,
    input  logic [7:0] x1,
    input  logic [1:0] AluOpMuxSel,
    output logic [7:0] y
);

    always_comb begin
        y = 8'b00;
        case (AluOpMuxSel)
            2'b00: begin
                y = x0 + x1;
            end
            2'b01: begin
                y = x0 - x1;
            end
            2'b10: begin
                y = x0 & x1;
            end
            2'b11: begin
                y = x0 | x1;
            end
        endcase
    end
    
endmodule
```

---

### ControlUnit

```verilog
`timescale 1ns / 1ps

module ControlUnit (
    input   logic       clk,
    input   logic       reset,
    output  logic       RFSrcMuxSel,
    output  logic [2:0] RAddr1,
    output  logic [2:0] RAddr2,
    output  logic [2:0] WAddr,
    output  logic [1:0] AluOpMuxSel,
    output  logic       we,
    input   logic       Lt,
    output  logic       OutPortEn
    );

    typedef enum {
        S0,
        S1, 
        S2, 
        S3, 
        S4,
        S5,
        S6,
        S7,
        S8,
        S9,
        S10,
        S11,
        S12,
        S13  
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
        RFSrcMuxSel    = 0;
        RAddr1         = 0;
        RAddr2         = 0;
        WAddr          = 0;
        AluOpMuxSel    = 0;
        we             = 0;
        OutPortEn      = 0;
        case (state)
            S0:begin   // R1 = 1
                RFSrcMuxSel    = 1;
                RAddr1         = 0;
                RAddr2         = 0;
                WAddr          = 1;
                AluOpMuxSel    = 0;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S1;
            end 
            S1:begin    // R2 = 0
                RFSrcMuxSel    = 0;
                RAddr1         = 0;
                RAddr2         = 0;
                WAddr          = 3'h2;
                AluOpMuxSel    = 0;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S2;
            end  
            S2:begin    // R3 = 0
                RFSrcMuxSel    = 0;
                RAddr1         = 0;
                RAddr2         = 0;
                WAddr          = 3'h3;
                AluOpMuxSel    = 0;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S3;
            end  
            S3:begin    // R4 = 0
                RFSrcMuxSel    = 0;
                RAddr1         = 0;
                RAddr2         = 0;
                WAddr          = 3'h4;
                AluOpMuxSel    = 0;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S4;
            end  
            S4:begin    // R2 = R1 + R1
                RFSrcMuxSel    = 0;
                RAddr1         = 1;
                RAddr2         = 1;
                WAddr          = 3'h2;
                AluOpMuxSel    = 0;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S5;
            end
            S5:begin    // R3 = R2 + R1
                RFSrcMuxSel    = 0;
                RAddr1         = 3'h2;
                RAddr2         = 1;
                WAddr          = 3'h3;
                AluOpMuxSel    = 0;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S6;
            end
            S6:begin    // R4 = R3 - R1 
                RFSrcMuxSel    = 0;
                RAddr1         = 3'h3;
                RAddr2         = 1;
                WAddr          = 3'h4;
                AluOpMuxSel    = 1;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S7;
            end
            S7:begin    // R1 = R1 | R2
                RFSrcMuxSel    = 0;
                RAddr1         = 3'h1;
                RAddr2         = 3'h2;
                WAddr          = 3'h1;
                AluOpMuxSel    = 2'h3;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S8;
            end
            S8:begin    // R4 < R2
                RFSrcMuxSel    = 0;
                RAddr1         = 3'h4;
                RAddr2         = 3'h2;
                WAddr          = 0;
                AluOpMuxSel    = 0;
                we             = 0;
                OutPortEn      = 0;
                if(Lt) next_state = S6;
                else next_state = S9; 
            end
            S9:begin    // R4 = R4 & R3
                RFSrcMuxSel    = 0;
                RAddr1         = 3'h4;
                RAddr2         = 3'h3;
                WAddr          = 3'h4;
                AluOpMuxSel    = 2'h2;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S10;
            end
            S10:begin    // R4 = R2 + R3
                RFSrcMuxSel    = 0;
                RAddr1         = 3'h2;
                RAddr2         = 3'h3;
                WAddr          = 3'h4;
                AluOpMuxSel    = 2'h0;
                we             = 1;
                OutPortEn      = 0;
                next_state     = S11;
            end
            S11:begin    // R4 > R2
                RFSrcMuxSel    = 0;
                RAddr1         = 3'h4;
                RAddr2         = 3'h2;
                WAddr          = 0;
                AluOpMuxSel    = 0;
                we             = 0;
                OutPortEn      = 0;
                if(Lt) next_state = S12;
                else   next_state = S4;
            end
            S12:begin   // OutPut    
                RFSrcMuxSel    = 0;
                RAddr1         = 3'h4;
                RAddr2         = 0;
                WAddr          = 0;
                AluOpMuxSel    = 0;
                we             = 0;
                OutPortEn      = 1;
                next_state     = S13;
            end
            S13:begin   // halt    
                RFSrcMuxSel    = 0;
                RAddr1         = 0;
                RAddr2         = 0;
                WAddr          = 0;
                AluOpMuxSel    = 0;
                we             = 0;
                OutPortEn      = 0;
                next_state     = S13;
            end 
        endcase
    end
    endmodule

```

---

## ✅ Schematic

<img src="/assets/img/CPU/alusche.png" style="width:100%; object-fit:contain;">

---

## ✅ 시뮬레이션

<img src="/assets/img/CPU/hwdp.png" style="width:100%; object-fit:contain;">

<img src="/assets/img/CPU/hwcu.png" style="width:100%; object-fit:contain;">

---

## ✅ 분석

### 반복별 레지스터 값
- 1회: R1=3,   R2=2,   R3=3,   R4=5   → Yes
- 2회: R1=7,   R2=6,   R3=9,   R4=15  → Yes
- 3회: R1=15,  R2=14,  R3=21,  R4=35  → Yes
- 4회: R1=31,  R2=30,  R3=45,  R4=75  → Yes
- 5회: R1=63,  R2=62,  R3=93,  R4=155 → Yes
- 6회: R1=127, R2=126, R3=189, R4=59  → **No ⇒ halt**

### ➡️ 첫 번째 반복
- 시작 값: (R1, R2, R3, R4) = (1, 0, 0, 0)
```bash
**계산 순서**
R2 = R1 + R1 (1 + 1 = 2) ➞ (1, **2**, 0, 0)
R3 = R2 + R1 (2 + 1 = 3) ➞ (1, 2, **3**, 0)
R4 = R3 - R1 (3 - 1 = 2) ➞ (1, 2, 3, **2**)
R1 = R1 | R2 (1 | 2 = 3) ➞ (**3**, 2, 3, 2)
R4 = R4 & R3 (2 & 3 = 2) ➞ (3, 2, 3, **2**)
R4 = R2 + R3 (2 + 3 = 5) ➞ (3, 2, 3, **5**)
조건 확인: R4 > R2 (5 > 2) ➞ ✅ Yes, 계속 진행
```

### ➡️ 두 번째 반복
시작 값: (R1, R2, R3, R4) = (3, 2, 3, 5)
```bash
**계산 순서**
R2 = R1 + R1 (3 + 3 = 6) ➞ (3, **6**, 3, 5)
R3 = R2 + R1 (6 + 3 = 9) ➞ (3, 6, **9**, 5)
R4 = R3 - R1 (9 - 3 = 6) ➞ (3, 6, 9, **6**)
R1 = R1 | R2 (3 | 6 = 7) ➞ (**7**, 6, 9, 6)
R4 = R4 & R3 (6 & 9 = 0) ➞ (7, 6, 9, **0**)
R4 = R2 + R3 (6 + 9 = 15) ➞ (7, 6, 9, **15**)
조건 확인: R4 > R2 (15 > 6) ➞ ✅ Yes, 계속 진행
```

### ➡️ 세 번째 반복
시작 값: (R1, R2, R3, R4) = (7, 6, 9, 15)
```bash
**계산 순서**
R2 = R1 + R1 (7 + 7 = 14) ➞ (7, **14**, 9, 15)
R3 = R2 + R1 (14 + 7 = 21) ➞ (7, 14, **21**, 15)
R4 = R3 - R1 (21 - 7 = 14) ➞ (7, 14, 21, **14**)
R1 = R1 | R2 (7 | 14 = 15) ➞ (**15**, 14, 21, 14)
R4 = R4 & R3 (14 & 21 = 4) ➞ (15, 14, 21, **4**)
R4 = R2 + R3 (14 + 21 = 35) ➞ (15, 14, 21, **35**)
조건 확인: R4 > R2 (35 > 14) ➞ ✅ Yes, 계속 진행
```

### ➡️ 네 번째 반복
시작 값: (R1, R2, R3, R4) = (15, 14, 21, 35)
```bash
**계산 순서**
R2 = R1 + R1 (15 + 15 = 30) ➞ (15, **30**, 21, 35)
R3 = R2 + R1 (30 + 15 = 45) ➞ (15, 30, **45**, 35)
R4 = R3 - R1 (45 - 15 = 30) ➞ (15, 30, 45, **30**)
R1 = R1 | R2 (15 | 30 = 31) ➞ (**31**, 30, 45, 30)
R4 = R4 & R3 (30 & 45 = 12) ➞ (31, 30, 45, **12**)
R4 = R2 + R3 (30 + 45 = 75) ➞ (31, 30, 45, **75**)
조건 확인: R4 > R2 (75 > 30) ➞ ✅ Yes, 계속 진행
```

### ➡️ 다섯 번째 반복
시작 값: (R1, R2, R3, R4) = (31, 30, 45, 75)
```bash
**계산 순서**
R2 = R1 + R1 (31 + 31 = 62) ➞ (31, **62**, 45, 75)
R3 = R2 + R1 (62 + 31 = 93) ➞ (31, 62, **93**, 75)
R4 = R3 - R1 (93 - 31 = 62) ➞ (31, 62, 93, **62**)
R1 = R1 | R2 (31 | 62 = 63) ➞ (**63**, 62, 93, 62)
R4 = R4 & R3 (62 & 93 = 28) ➞ (63, 62, 93, **28**)
R4 = R2 + R3 (62 + 93 = 155) ➞ (63, 62, 93, **155**)
조건 확인: R4 > R2 (155 > 62) ➞ ✅ Yes, 계속 진행
```

### ➡️ 여섯 번째 반복 (마지막)
시작 값: (R1, R2, R3, R4) = (63, 62, 93, 155)
```bash
**계산 순서**
R2 = R1 + R1 (63 + 63 = 126) ➞ (63, **126**, 93, 155)
R3 = R2 + R1 (126 + 63 = 189) ➞ (63, 126, **189**, 155)
R4 = R3 - R1 (189 - 63 = 126) ➞ (63, 126, 189, **126**)
R1 = R1 | R2 (63 | 126 = 127) ➞ (**127**, 126, 189, 126)
R4 = R4 & R3 (126 & 189 = 120) ➞ (127, 126, 189, **120**)
R4 = R2 + R3 (126 + 189 = 315)
8-bit Wrapping (오버플로우): 8비트 레지스터는 255까지만 표현 가능하므로, 315는 256을 뺀 나머지 값인 59가  된다. (315 - 256 = 59)
R4 최종 값 ➞ (127, 126, 189, **59**)
조건 확인: R4 > R2 (59 > 126) ➞ ❌ No, 중단 (Halt)
```
