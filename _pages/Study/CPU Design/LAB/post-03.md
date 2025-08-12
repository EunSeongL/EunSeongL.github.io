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

## ✅ 시뮬레이션

<img src="/assets/img/CPU/hwdp.png" style="width:100%; object-fit:contain;">

<img src="/assets/img/CPU/hwcu.png" style="width:100%; object-fit:contain;">

