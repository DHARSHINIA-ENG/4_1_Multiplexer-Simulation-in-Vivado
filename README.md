# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation

```
module mux4(I,S,y);
input [3:0]I;
input [1:0]S;
output y;
wire [4:1]W;
and g1(W[1],(~S[0]),(~S[1]),I[0]);
and g2(W[2],(~S[1]),S[0],I[1]);
and g3(W[3],S[1],(~S[0]),I[2]);
and g4(W[4],S[0],S[1],I[3]);
or g5(y,W[1],W[2],W[3],W[4]);
endmodule

```
### 4:1 MUX Gate-Level Implementation- Testbench
```
`timescale 1ns / 1ps
module mux4_tb;
reg [3:0]I;
reg [1:0]S;
wire y;
mux4 uut(I,S,y);
initial
begin
I=4'b1011;
S=2'b00;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b01;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b10;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b11;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
$finish;
end 
endmodule

```
## Simulated Output Gate Level Modelling

<img width="1920" height="1080" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/e9e2a574-ce48-436f-adb0-6e9a8e89f63c" />


---
### 4:1 MUX Data flow Modelling
```
module mux4_1(I,S,y);
input [3:0]I;
input [1:0]S;
output y;
wire [4:1]W;
assign W[1]=(~S[0])&(~S[1])&I[0];
assign W[2]=(~S[1])&S[0]&I[1];
assign W[3]=S[1]&(~S[0])&I[2];
assign W[4]=S[0]&S[1]&I[3];
assign y=W[1]|W[2]|W[3]|W[4];
endmodule

```
### 4:1 MUX Data flow Modelling- Testbench
```
`timescale 1ns / 1ps
module tb_mux4_1;
reg [3:0]I;
reg [1:0]S;
wire y;
mux4_1 uut(I,S,y);
initial
begin
I=4'b1011;
S=2'b00;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b01;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b10;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b11;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
$finish;
end 
endmodule


```
## Simulated Output Dataflow Modelling

<img width="1920" height="1080" alt="Screenshot (3)" src="https://github.com/user-attachments/assets/41caf6d5-a2d1-404e-ac06-c7ea8388dedd" />


---
### 4:1 MUX Behavioral Implementation
```
module Behav(I,S,y);
input [3:0]I;
input [1:0]S;
output reg y;
always @ (*)
  begin
      case(S)
        2'b00: y =I[0];
        2'b01:y=I[1];
        2'b10:y=I[2];
        2'b11:y=I[3];
      endcase
   end
endmodule


```
### 4:1 MUX Behavioral Modelling- Testbench
```
`timescale 1ns / 1ps
module tb_Behav;
reg [3:0]I;
reg [1:0]S;
wire y;
Behav uut(I,S,y);
initial
begin
I=4'b1011;
S=2'b00;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b01;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b10;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b11;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
$finish;
end 
endmodule


```
## Simulated Output Behavioral Modelling

<img width="1920" height="1080" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/5ddd40e8-692f-4dd7-b710-ebe6c4e20e3c" />




![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)

### 4:1 MUX Structural Implementation

```
module mux(a,b,s,x);
input a,b,s;
output x;
wire w1,w2;
and g1(w1,a,~s);
and g2(w2,b,s);
or g3(x,w1,w2);
endmodule

module mux4(I,S,y);
input [3:0]I;
input [1:0]S;
output y;
wire W1,W2;
mux m1(I[0],I[1],S[1],W1);
mux m2(I[2],I[3],S[1],W2);
mux m3(W1,W2,S[0],y);
endmodule
```
### Testbench Implementation
```timescale 1ns / 1ps
module mux4_tb;
reg [3:0]I;
reg [1:0]S;
wire y;
mux4 uut(I,S,y);
initial
begin
I=4'b1011;
S=2'b00;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b01;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b10;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
S=2'b11;
#10;
$display("selection is %b %b , output is %b",S[1],S[0],y);
$finish;
end 
endmodule

```
## Simulated Output Structural Modelling

<img width="1920" height="1080" alt="Screenshot (3)" src="https://github.com/user-attachments/assets/3c8e59e3-1bf4-4553-974c-afccdbce2589" />


---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.

