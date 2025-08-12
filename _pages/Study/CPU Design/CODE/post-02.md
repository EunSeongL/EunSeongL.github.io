---
title: "DedicatedProcessor Adder"
date: "2025-08-07"
thumbnail: "/assets/img/CPU/cpu_thumb.png"
---

# DedicatedProcessor Adder

---

## ✅ top.sv

```verilog
`timescale 1ns / 1ps

module top(
   input  logic       clk,
   input  logic       reset,
   output logic [3:0] fndCom,
   output logic [7:0] fndFont
   );

   logic [ 7:0] OutPort;
   logic        clk_10hz;
   
   clk_divider U_CLK_DIV (
       .clk            (clk),
       .reset          (reset),
       .clk_10hz       (clk_10hz)
   );

   DedicatedProcessor_Adder U_DedicatedProcessor_Adder (
       .clk            (clk_10hz),
       .reset          (reset),
       .OutPort        (OutPort)
   );

   fndController U_fndController (
       .clk            (clk),
       .reset          (reset),
       .number         (OutPort),
       .fndCom         (fndCom),
       .fndFont        (fndFont)
   );
    
endmodule

module clk_divider (
   input  logic  clk,
   input  logic  reset,
   output logic  clk_10hz
   );

   logic [$clog2(10_000_000)-1:0] div_counter;

   always_ff @(posedge clk or posedge reset) begin
       if(reset) begin
           div_counter <= 0;
           clk_10hz    <= 0;
       end
       else begin
           if(div_counter == 10_000_000 - 1)begin
               div_counter <= 0;
               clk_10hz    <= 1;
           end
           else begin
               div_counter <= div_counter + 1;
               clk_10hz    <= 0;
           end
       end
   end
   
   endmodule
```

## ✅ DedicatedProcessor_Adder.sv

```verilog
`timescale 1ns / 1ps

module DedicatedProcessor_Adder(
   input  logic        clk,
   input  logic        reset,
   output logic [ 7:0] OutPort
    
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

## ✅ DataPath.sv

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

## ✅ ControlUnit.sv

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

