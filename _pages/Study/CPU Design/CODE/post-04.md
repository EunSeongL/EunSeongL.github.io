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
    input  logic [ 3:0] aluControl,
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
    input  logic  [ 3:0] aluControl,
    input  logic  [31:0] a,
    input  logic  [31:0] b,
    output logic  [31:0] result
    );

    always_comb begin
        result = 32'bx;
        case (aluControl)
            4'b0000: result = a + b;            // add
            4'b0001: result = a - b;            // sub
            4'b0010: result = a & b;            // and
            4'b0011: result = a | b;            // or
            4'b0100: result = a << b;           // sll
            4'b0101: result = a >> b;           // srl
            4'b0110: result = $signed(a) >>> b; // sra
            4'b0111: result = ($signed(a) < $signed(b)) ? 1 : 0;  // slt
            4'b1000: result = (a < b) ? 1 : 0;  // sltu
            4'b1001: result = a ^ b;            // xor
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

> Test를 위한 regFile 수정

```verilog
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

    initial begin   // for Simulation Test
        for (int i = 0; i < 32; i++) begin
            mem[i] = 10 + i;
        end
    end

    always_ff @(posedge clk) begin
        if(we) begin
            mem[WA] <= WD;
        end
    end

    assign RD1 = (RA1 != 0) ? mem[RA1] : 32'b0;
    assign RD2 = (RA2 != 0) ? mem[RA2] : 32'b0;
    
    endmodule
```

---

## ✅ ControlUnit.sv
> Single-Cycle은 combinational logic으로 구현.<br>
wire로 선언한 이유?<br> 
=> logic으로는 {instrCode[30], instrCode[14:12]}이게 안됌.<br>
=> 사용하려면 assign으로 연결<br>

```verilog
logic [6:0] opcode;
logic [3:0] operator;
assign opcode = instrCode[6:0];
assign operator = {instrCode[30], instrCode[14:12]};
```

```verilog
`timescale 1ns / 1ps

module ControlUnit (
    input  logic [31:0] instrCode,
    output logic        regFileWe,
    output logic [ 3:0] aluControl
    ); 

    wire [6:0] opcode = instrCode[6:0];
    wire [3:0] operator = {instrCode[30], instrCode[14:12]}; // function

    always_comb begin
        regFileWe = 1'b0;
        case (opcode)
            7'b0110011: regFileWe = 1'b1;   // R-Type
        endcase
    end

    always_comb begin
        aluControl = 2'bx;
        case (opcode)
            7'b0110011: begin   // R-Type
                aluControl = 2'bx;
                case (operator)
                    4'b0000: aluControl = 4'b0000;    // ADD
                    4'b1000: aluControl = 4'b0001;    // SUB
                    4'b0111: aluControl = 4'b0010;    // AND
                    4'b0110: aluControl = 4'b0011;    // OR
                    4'b0001: aluControl = 4'b0100;    // SLL 
                    4'b0101: aluControl = 4'b0101;    // SRL 
                    4'b1101: aluControl = 4'b0110;    // SRA
                    4'b0010: aluControl = 4'b0111;    // SLT 
                    4'b0011: aluControl = 4'b1000;    // SLTU
                    4'b0100: aluControl = 4'b1001;    // XOR
                endcase
            end
        endcase
    end

endmodule
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

> 어셈블리어에 대한 머신 코드이다.
```verilog
`timescale 1ns / 1ps

module ROM (
    input  logic [31:0] addr,
    output logic [31:0] data    
    );
    
    logic [31:0] rom[0:61];

    initial begin   // for Simulation Test
        // rom[x] = 32'b funct7 _ rs2 _ rs1 _ funct3 _ rd _ op
        // 어셈블리어에 대한 머신 코드이다.
        rom[0] = 32'b0000000_00001_00010_000_01000_0110011; // add  x8, x2, x1
        rom[1] = 32'b0100000_00010_00001_000_01001_0110011; // sub  x9, x1, x2
        rom[2] = 32'b0000000_00100_00011_111_01010_0110011; // and  x10, x3, x4
        rom[3] = 32'b0000000_00011_00100_110_01011_0110011; // or   x11, x4, x3
        rom[4] = 32'b0000000_00101_00001_001_01100_0110011; // sll  x12, x1, x5
        rom[5] = 32'b0000000_00101_00100_101_01101_0110011; // srl  x13, x4, x5
        rom[6] = 32'b0100000_00101_00100_101_01110_0110011; // sra  x14, x4, x5
        rom[7] = 32'b0000000_00010_00100_010_01111_0110011; // slt  x15, x4, x2
        rom[8] = 32'b0000000_00000_00011_011_10000_0110011; // sltu x16, x3, x0
        rom[9] = 32'b0000000_00100_00011_100_10001_0110011; // xor  x17, x3, x4
    end

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

## ✅ TestBench

```verilog
`timescale 1ns / 1ps

module tb_RV32I();

    logic clk;
    logic reset;

    MCU U_DUT (.*);

    always#5 clk = ~clk;

    initial begin
        clk = 0;
        reset = 1;
        #20;
        reset = 0;
        #60;
        $finish();
    end

endmodule
```