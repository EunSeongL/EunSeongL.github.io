---
title: "DedicatedProcessor Counter"
date: "2025-08-07"
thumbnail: "/assets/img/CPU/cpu_thumb.png"
---

# DedicatedProcessor Counter

---

## ✅ DedicatedProcessor Counter

```verilog
`timescale 1ns / 1ps

module DedicatedProcessor_Counter(
    input  logic       clk,
    input  logic       reset,
    output logic [7:0] OutBuffer
    );

    logic ASrcMuxsel, AEn, OutBufEn, ALt10;
    logic [$clog2(100_000_000 / 10)-1:0] div_counter;
    logic clk_10hz;

    always_ff @(posedge clk or posedge reset) begin
        if(reset) begin
            div_counter <= 0;
        end
        else begin
            if(div_counter == 10_000_000 - 1) begin
                div_counter <= 0;
                clk_10hz <= 1'b1;
            end
            else begin
                div_counter <= div_counter + 1;
                clk_10hz <= 0;
            end
        end
    end

    DataPath U_DataPath (
        .clk    (clk_10hz),
        .*
    );

    ControlUnit U_ControlUnit (
        .clk    (clk_10hz),
        .*
    );

    endmodule


module DataPath(
        input  logic       clk,
        input  logic       reset,
        input  logic       ASrcMuxsel,
        input  logic       AEn,
        output logic       ALt10,
        input  logic       OutBufEn,
        output logic [7:0] OutBuffer
    );

    logic [7:0] AdderResult, ASrcMuxOut, ARegOut;

    mux_2X1 U_ASrcMux (
        .sel    (ASrcMuxsel),
        .x0     (8'h0),
        .x1     (AdderResult),
        .y      (ASrcMuxOut)
    );

    register U_Register (
        .clk    (clk),
        .reset  (reset),
        .en     (AEn),
        .d      (ASrcMuxOut),
        .q      (ARegOut)
    );

    register U_OutReg (
        .clk    (clk),
        .reset  (reset),
        .en     (OutBufEn),
        .d      (ARegOut),
        .q      (OutBuffer)
    );

    comparator U_Comparator(
        .a      (ARegOut),
        .b      (8'd10),
        .lt     (ALt10)
    );

    adder U_Adder (
        .a      (ARegOut),
        .b      (8'd1),
        .sum    (AdderResult)    
    );

    /*
    OutBuf U_OutBuf (
        .en     (OutBufEn),
        .x      (ARegOut),
        .y      (OutBuffer)
    );
    */
    endmodule


module register (
    input  logic       clk,
    input  logic       reset,
    input  logic       en,
    input  logic [7:0] d,
    output logic [7:0] q
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

module mux_2X1 (
    input  logic       sel,
    input  logic [7:0] x0,
    input  logic [7:0] x1,
    output logic [7:0] y
    );

    always_comb begin
        y = 8'b0;
        case (sel)
            1'b0: y = x0; 
            1'b1: y = x1;
        endcase
    end
    
    endmodule

module adder (
    input  logic [7:0] a,
    input  logic [7:0] b,
    output logic [7:0] sum    
    );

    assign sum = a + b;
    
    endmodule

module comparator (
    input  logic [7:0] a,
    input  logic [7:0] b,
    output logic       lt
    );
    
    assign lt = a < b;

    endmodule

module OutBuf (
    input  logic       en,
    input  logic [7:0] x,
    output logic [7:0] y
    );
    
    assign y = en ? x : 8'bx;

    endmodule


module ControlUnit(
    input  logic clk,
    input  logic reset,
    input  logic ALt10,
    output logic ASrcMuxsel,
    output logic AEn,
    output logic OutBufEn
    );

    typedef enum {
        S0,
        S1, 
        S2, 
        S3, 
        S4  
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
        ASrcMuxsel = 0;
        AEn        = 0;
        OutBufEn   = 0;
        case (state)
            S0:begin
                ASrcMuxsel = 0;
                AEn        = 1;
                OutBufEn   = 0;
                next_state = S1;
            end 
            S1:begin
                ASrcMuxsel = 1;
                AEn        = 0;
                OutBufEn   = 0;
                if(ALt10)  next_state = S2;
                else       next_state = S4;
            end  
            S2:begin
                ASrcMuxsel = 1;
                AEn        = 0;
                OutBufEn   = 1;
                next_state = S3;
            end  
            S3:begin
                ASrcMuxsel = 1;
                AEn        = 1;
                OutBufEn   = 0;
                next_state = S1;
            end  
            S4:begin
                ASrcMuxsel = 1;
                AEn        = 0;
                OutBufEn   = 0;
                next_state = S4;
            end  
        endcase
    end

    endmodule

```