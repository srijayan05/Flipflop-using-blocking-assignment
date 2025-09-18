# EXPERIMENT 3: Simulation of All Flip-Flops using Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **blocking assignment (`=`)** inside the `always` block.  
Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Blocking)
```verilog
module sr_ff (
    input wire S, R, clk,
    output reg Q
);
    always @(posedge clk) begin
        if (S & ~R)      Q = 1'b1;   // Set
        else if (~S & R) Q = 1'b0;   // Reset
        else if (S & R)  Q = 1'bx;   // Invalid
        else             Q = Q;      // Hold
    end
endmodule
```
### SR Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_sr_ff;
    reg S, R, clk;
    wire Q;

    sr_ff uut (.S(S), .R(R), .clk(clk), .Q(Q));

    initial clk = 0;
    always #5 clk = ~clk; // 10ns clock

    initial begin
        $monitor("Time=%0t | S=%b R=%b | Q=%b", $time, S, R, Q);

        S=0; R=0; #10;
        S=1; R=0; #10;
        S=0; R=1; #10;
        S=1; R=1; #10;
        S=0; R=0; #10;

        $finish;
    end
endmodule


```
#### SIMULATION OUTPUT

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/a21aa7f7-eb17-4cab-a45a-9db2eb82a8ae" />


### JK Flip-Flop (Blocking)
```verilog
module jk_ff (
    input wire J, K, clk,
    output reg Q
);
    always @(posedge clk) begin
        if (~J & ~K) Q = Q;       // Hold
        else if (J & ~K) Q = 1;   // Set
        else if (~J & K) Q = 0;   // Reset
        else if (J & K) Q = ~Q;   // Toggle
    end
endmodule
```
### JK Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_jk_ff;
    reg J, K, clk;
    wire Q;

    jk_ff uut (.J(J), .K(K), .clk(clk), .Q(Q));

    initial clk = 0;
    always #5 clk = ~clk;

    initial begin
        $monitor("Time=%0t | J=%b K=%b | Q=%b", $time, J, K, Q);

        J=0; K=0; #10;
        J=1; K=0; #10;
        J=0; K=1; #10;
        J=1; K=1; #10;
        J=1; K=1; #10;

        $finish;
    end
endmodule
```
#### SIMULATION OUTPUT

<img width="1915" height="1079" alt="Screenshot 2025-09-17 211559" src="https://github.com/user-attachments/assets/f9de3494-7b0e-408b-96a8-00aaf3ea5a49" />

### D Flip-Flop (Blocking)
```verilog
module d_ff (
    input wire d, clk,
    output reg Q
);
    always @(posedge clk) begin
        Q = d;  // On clock edge, Q follows D
    end
endmodule
```
### D Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_d_ff;
    reg d, clk;
    wire Q;

    d_ff uut (.d(d), .clk(clk), .Q(Q));

    initial clk = 0;
    always #5 clk = ~clk;

    initial begin
        $monitor("Time=%0t | D=%b | Q=%b", $time, d, Q);

        d=0; #10;
        d=1; #10;
        d=0; #10;
        d=1; #10;

        $finish;
    end
endmodule



```

#### SIMULATION OUTPUT

<img width="1912" height="1075" alt="image" src="https://github.com/user-attachments/assets/ae454884-59b4-43c7-a2bb-97046ef01c33" />

### T Flip-Flop (Blocking)
```verilog
module t_ff (
    input wire T, clk,
    output reg Q
);
    always @(posedge clk) begin
        if (T) Q = ~Q;  // Toggle when T=1
        else   Q = Q;   // Hold when T=0
    end
endmodule

```
### T Flip-Flop Test bench 
```verilog
`timescale 1ns/1ps
module tb_t_ff;
    reg T, clk;
    wire Q;

    t_ff uut (.T(T), .clk(clk), .Q(Q));

    initial clk = 0;
    always #5 clk = ~clk;

    initial begin
        $monitor("Time=%0t | T=%b | Q=%b", $time, T, Q);

        T=0; #10;
        T=1; #10;
        T=1; #10;
        T=0; #10;

        $finish;
    end
endmodule



```

#### SIMULATION OUTPUT

<img width="1916" height="1078" alt="image" src="https://github.com/user-attachments/assets/16ed3680-c1fd-43dc-8dc1-04936e17cb16" />


### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.
