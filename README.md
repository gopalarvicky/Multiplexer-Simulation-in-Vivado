## SIMULATION AND IMPLEMENTATION OF LOGIC GATES

## AIM:
```
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.
```
## APPARATUS REQUIRED:
```
Vivado 2023.1
```
## Procedure
```
1. Launch Vivado
Open Vivado 2023.1 by double-clicking the Vivado icon or searching for it in the Start menu.
2. Create a New Project
Click on "Create Project" from the Vivado Quick Start window.
In the New Project Wizard:
Project Name: Enter a name for the project (e.g., Mux4_to_1).
Project Location: Select the folder where the project will be saved.
Click Next.
Project Type: Select RTL Project, then click Next.
Add Sources:
Click on "Add Files" to add the Verilog files (e.g., mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.).
Make sure to check the box "Copy sources into project" to avoid any external file dependencies.
Click Next.
Add Constraints: Skip this step by clicking Next (since no constraints are needed for simulation).
Default Part Selection:
You can choose a part based on the FPGA board you are using (if any).
If no board is used, you can choose any part, for example, xc7a35ticsg324-1L (Artix-7).
Click Next, then Finish.
3. Add Verilog Source Files
In the "Sources" window, right-click on "Design Sources" and select Add Sources if you didn't add all files earlier.
Add the Verilog files (mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.) and the testbench (mux4_to_1_tb.v).
4. Check Syntax
Expand the "Flow Navigator" on the left side of the Vivado interface.
Under "Synthesis", click "Run Synthesis".
Vivado will check your design for syntax errors. If any errors or warnings appear, correct them in the respective Verilog files and re-run the synthesis.
5. Simulate the Design
In the Flow Navigator, under "Simulation", click on "Run Simulation" → "Run Behavioral Simulation".
Vivado will open the Simulations Window, and the waveform window will show the signals defined in the testbench.
6. View and Analyze Simulation Results
The simulation waveform window will display the signals (S1, S0, A, B, C, D, Y_gate, Y_dataflow, Y_behavioral, Y_structural).
Use the time markers to verify the correctness of the 4:1 MUX output for each set of inputs.
You can zoom in/out or scroll through the simulation time using the waveform viewer controls.
7. Adjust Simulation Time
To run a longer simulation or adjust timing, go to the Simulation Settings by clicking "Simulation" → "Simulation Settings".
Under "Simulation", modify the Run Time (e.g., set to 1000ns).
8. Generate Simulation Report
Once the simulation is complete, you can generate a simulation report by right-clicking on the simulation results window and selecting "Export Simulation Results".
Save the report for reference in your lab records.
9. Save and Document Results
Save your project by clicking File → Save Project.
Take screenshots of the waveform window and include them in your lab report to document your results.
You can include the timing diagram from the simulation window showing the correct functionality of the 4:1 MUX across different select inputs and data inputs.
10. Close the Simulation
Once done, close the simulation by going to Simulation → "Close Simulation".
```
## Logic Diagram

![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

## Truth Table

![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

## VERILOG CODE:

## 4:1 MUX Gate-Level Implementation
```
 module multiplexer(s1,s0,a,b,c,d,y);
input s1,s0,a,b,c,d;
output y;
wire[3:0]w;
and g1(w[0],~s1,~s0,a);
and g2(w[1],~s1,s0,b);
and g3(w[2],s1,~s0,c);
and g4(w[3],s1,s0,d);
or g5(y,w[0],w[1],w[2],w[3]);
endmodule
```
## OUTPUT:

 ![image](https://github.com/user-attachments/assets/3cd9f551-ed32-4ca4-b836-8aa6fa5e9421)
  
## 4:1 MUX Data Flow Implementation
```
module mul_data( Y,I0,I1,I2,I3,S0,S1);
output Y;       
input I0, I1, I2, I3;
input S0, S1;     
assign Y = (~S1 & ~S0 & I0) |  
          (~S1 & S0 & I1)  |  
           (S1 & ~S0 & I2)  |  
           (S1 & S0 & I3);
endmodule
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/bf6e774d-fd6e-4c33-8195-103fff0c6820)

## 4:1 MUX Behavioral Implementation
```
module mux(s, i, y);
input [1:0] s;
input [3:0] i;
output reg y;  
always @(s or i)  
begin
 case (s)
        2'b00: y = i[0];  
        2'b01: y = i[1];   
        2'b10: y = i[2];
        2'b11: y = i[3];  
        default: y = 1'b0;
    endcase
end
endmodule
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/0344dee4-857f-4a9b-ba6e-db34ebd1ed76)

## 4:1 MUX Structural Implementation
```
module mux_4to1 (a,b,c,d,S0,S1,Y);
input a,b,c,d;
input  S0, S1;       
output  Y ;          
assign Y = (S1 == 0 && S0 == 0) ? a :
 (S1 == 0 && S0 == 1) ? b :
(S1 == 1 && S0 == 0) ? c :
(S1 == 1 && S0 == 1) ? d:
endmodule
```
## OUTPUT:

![image](https://github.com/user-attachments/assets/6184e256-32de-468e-9036-ac2d2221b34a)

## Testbench Implementation
```
module multiplexer_tb;
 // Declare inputs as reg and outputs as wire
 reg s1, s0, a, b, c, d;
 wire y;

 // Instantiate the multiplexer module
 multiplexer uut (
   .s1(s1), 
   .s0(s0), 
   .a(a), 
   .b(b), 
   .c(c), 
   .d(d), 
   .y(y)
 );

 // Test cases
 initial begin
   // Monitor changes in inputs and output
   $monitor("s1 = %b, s0 = %b, a = %b, b = %b, c = %b, d = %b, y = %b", s1, s0, a, b, c, d, y);
   
   // Apply test vectors
   s1 = 0; s0 = 0; a = 1; b = 0; c = 0; d = 0; #10;  // Test case 1
   s1 = 0; s0 = 1; a = 0; b = 1; c = 0; d = 0; #10;  // Test case 2
   s1 = 1; s0 = 0; a = 0; b = 0; c = 1; d = 0; #10;  // Test case 3
   s1 = 1; s0 = 1; a = 0; b = 0; c = 0; d = 1; #10;  // Test case 4
   
   // Finish simulation
   $finish;
 end
endmodule
```
 ## OUTPUT:

![image](https://github.com/user-attachments/assets/1dfc69a0-6636-4681-ae58-b24e32144994)

## Conclusion:
```
In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural. The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.
```
  
