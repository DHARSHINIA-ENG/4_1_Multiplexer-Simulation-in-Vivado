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
```timescale 1ns / 1ps
module mux4(I,S,y);
input [3:0]I;
input [1:0]S;
output y;
wire [4:1]W;
and g1(W[1],(~S[0]),(~S[1]),I[0]);
and g2(W[2],(~S[0]),S[1],I[1]);
and g3(W[3],S[0],(~S[1]),I[2]);
and g4(W[4],S[0],S[1],I[3]);
or g5(y,W[1],W[2],W[3],W[4]);
endmodule
```
### 4:1 MUX Gate-Level Implementation- Testbench
```
module mux_4_1_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
MUX_4_1 uut(I,S,Y);
initial
begin
I=4'b0101;
S=2'b00;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b01;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b10;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b11;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
$finish;
end 
endmodule
```
## Simulated Output Gate Level Modelling

![WhatsApp Image 2025-09-17 at 21 15 52_bb8f4f11](https://github.com/user-attachments/assets/13c57c28-22a5-48c6-a53e-f29f95b72b3a)


---
### 4:1 MUX Data flow Modelling
```timescale 1ns / 1ps
module mux4(I,S,y);
input [3:0]I;
input [1:0]S;
output y;
wire [4:1]W;
assign W[1]=(~S[0])&(~S[1])&I[0];
assign W[2]=(~S[0])&S[1]&I[1];
assign W[3]=S[0]&(~S[1])&I[2];
assign W[4]=S[0]&S[1]&I[3];
assign y=W[1]|W[2]|W[3]|W[4];
endmodule

```
### 4:1 MUX Data flow Modelling- Testbench
```module mux_4_1_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
MUX_4_1 uut(I,S,Y);
initial
begin
I=4'b0101;
S=2'b00;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b01;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b10;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b11;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
$finish;
end 
endmodule


```
## Simulated Output Dataflow Modelling

![WhatsApp Image 2025-09-17 at 21 15 52_bb8f4f11](https://github.com/user-attachments/assets/34ef486a-e483-42e2-bfd2-4d1108fea063)


---
### 4:1 MUX Behavioral Implementation
```
timescale 1ns / 1ps
module MUX_4(I,S,y);
input [3:0]I;
input [1:0]S;
output y;
always @ (*)
   begin
      case(S)
      2'b00:y=I[0];
      2'b01:y=I[1];
      2'b10:y=I[2];
      2'b11:y=I[3];
      endcase
   end
endmodule
```

### 4:1 MUX Behavioral Modelling- Testbench
```module mux_4_1_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
MUX_4_1 uut(I,S,Y);
initial
begin
I=4'b0101;
S=2'b00;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b01;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b10;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b11;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
$finish;
end 
endmodule

```

## Simulated Output Behavioral Modelling

![WhatsApp Image 2025-09-17 at 21 15 52_bb8f4f11](https://github.com/user-attachments/assets/a813a750-a833-4281-89c5-6af7aac265ab)



![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)

### 4:1 MUX Structural Implementation

```timescale 1ns / 1ps
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

```module mux_4_1_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
MUX_4_1 uut(I,S,Y);
initial
begin
I=4'b0101;
S=2'b00;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b01;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b10;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
S=2'b11;
#10;
$display("Selection is %b %b , Output : %b ", S[1],S[0],Y);
$finish;
end 
endmodule

```
## Simulated Output Structural Modelling

![WhatsApp Image 2025-09-17 at 21 15 52_bb8f4f11](https://github.com/user-attachments/assets/87678a88-9606-48c0-a612-936f88930b53)


---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.

