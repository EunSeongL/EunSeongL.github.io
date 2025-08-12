---
title: "DedicatedProcessor Adder"
date: "2025-08-07"
thumbnail: "/assets/img/CPU/cpu_thumb.png"
---

# CODE

---

#### top.sv

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

