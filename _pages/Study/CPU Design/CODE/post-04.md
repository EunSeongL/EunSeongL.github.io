---
title: "RV32I_R_Type"
tags:
    - Code
    - CPU Design
date: "2025-08-14"
thumbnail: "/assets/img/CPU/cpu_thumb.png"
---

# RV32I_R_Type

---

## ✅ DataPath.sv

```verilog
`timescale 1ns / 1ps

module DataPath (
    input  logic        clk,
    input  logic        reset,
    input  logic [31:0] instrCode,
    input  logic        regFileWe,
    input  logic [ 1:0] aluControl,
    output logic [31:0] instrMemAddr
);

    logic [31:0] aluResult;
    logic [31:0] RFData1, RFData2;
    logic [31:0] PCSrcData, PCOutData;

    assign instrMemAddr = PCOutData;

    RegisterFile U_RegFile (
        .clk       (clk),
        .we        (regFileWe),
        .RA1       (instrCode[19:15]),
        .RA2       (instrCode[24:20]),
        .WA        (instrCode[11: 7]),
        .WD        (aluResult),
        .RD1       (RFData1),
        .RD2       (RFData2)
    );

    alu U_ALU (
        .aluControl(aluControl),
        .a         (RFData1),
        .b         (RFData2),
        .result    (aluResult)
    );

    register U_PC (
        .clk       (clk),
        .reset     (reset),
        .en        (1'b1),
        .d         (PCSrcData),
        .q         (PCOutData)   
    );

    adder U_PC_Adder(
        .a         (32'd4),
        .b         (PCOutData),
        .y         (PCSrcData)
    );

endmodule

module alu (
    input  logic  [1:0] aluControl,
    input  logic  [31:0] a,
    input  logic  [31:0] b,
    output logic  [31:0] result
    );

    always_comb begin
        result = 32'bx;
        case (aluControl)
            2'b00: result = a + b;
            2'b01: result = a - b;
            2'b10: result = a & b;
            2'b11: result = a | b; 
        endcase
    end
    
    endmodule

module RegisterFile (
    input  logic        clk,
    input  logic        we,
    input  logic [ 4:0] RA1,
    input  logic [ 4:0] RA2,
    input  logic [ 4:0] WA,
    input  logic [31:0] WD,
    output logic [31:0] RD1,
    output logic [31:0] RD2
    );
    
    logic [31:0] mem [0:2**5-1];

    always_ff @(posedge clk) begin
        if(we) begin
            mem[WA] <= WD;
        end
    end

    assign RD1 = (RA1 != 0) ? mem[RA1] : 32'b0;
    assign RD2 = (RA2 != 0) ? mem[RA2] : 32'b0;
    
    endmodule

module register (
    input  logic        clk,
    input  logic        reset,
    input  logic        en,
    input  logic [31:0] d,
    output logic [31:0] q    
    );

    always_ff @(posedge clk or posedge reset) begin
        if(reset) begin
            q <= 0;
        end
        else begin
            if(en) begin
                q <= d;
            end
        end
    end
    endmodule

module adder (
    input  logic [31:0] a,
    input  logic [31:0] b,
    output logic [31:0] y
    );
    
    assign y = a + b;

    endmodule
```

## ✅ ControlUnit.sv
> // Single-Cycle은 combinational logic으로 구현
wire로 선언한 이유는 logic으로는 {instrCode[30], instrCode[14:12]}이게 안됌.
사용하려면 assign으로 연결
logic [6:0] opcode;
logic [3:0] operator;
assign opcode = instrCode[6:0];
assign operator = {instrCode[30], instrCode[14:12]};

```verilog

```

## ✅ ROM.sv
> 4의 배수로 나눈 값이랑 같다.
<img src="/assets/img/CPU/rom.png" style="width:100%; object-fit:contain;">

```verilog
`timescale 1ns / 1ps

module ROM (
    input  logic [31:0] addr,
    output logic [31:0] data    
    );
    
    logic [31:0] rom[0:61];

    assign data = rom[addr[31:2]];

endmodule
```

---

## ✅ MCU.sv

```verilog
`timescale 1ns / 1ps

module MCU(
    input logic clk,
    input logic reset  
    );

    logic [31:0] instrMemAddr, instrCode;

    CPU_RV32I U_CPU_RV32I (
        .clk            (clk),
        .reset          (reset),
        .instrCode      (instrCode),
        .instrMemAddr   (instrMemAddr)
    );

    ROM U_ROM(
        .addr   (instrMemAddr),
        .data   (instrCode)
    );

endmodule
```
