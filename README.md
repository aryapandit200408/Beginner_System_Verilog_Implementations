# Beginner_System_Verilog_Implementations

This is a repository showcasing some of my beginner implementations using System Verilog for design and verification.

### Dataﬂow modeling of half adder

top.sv
```sv
`timescale 1ns / 1ps
module top( a, b, cout, sum);
    input logic a, b;
    output logic cout, sum;
    
    assign sum = a ^ b;
    assign cout = a & b;
    
endmodule
```

I use a file based testbench here, so I declare a memory file, tf.mem

tf.mem
```
0, 0, 0, 0
0, 1, 0, 1
1, 0, 0, 1
1, 1, 1, 0
```

tb.sv
```sv
`timescale 1ns / 1ps
module tb;
    logic a, b, cout, sum, coutest, sumtest;
    
    top DUT( a, b , cout, sum);
    
    integer tf;
    initial begin
        tf = $fopen("tf.mem", "r");
        if (tf == 0) $fatal("ERROR: Could Not Open Test File");
    end
    
    integer line_num = 0;
    integer num = 0;
    initial begin 
        while (!$feof(tf)) begin 
            line_num++;
            $fscanf(tf, "%b, %b, %b, %b", a, b, coutest, sumtest);
            #10;
            $display ("a = %0b b =%0b cout = %0b sum = %0b", a, b, cout , sum );
            if ((coutest == cout) & (sumtest == sum)) begin
                $display("Test Case Passed");
            end
            
            else begin
                num++;
            end
        end 
        
        if (num == 0) begin
            $display ("All test passed!");
        end
        else begin
            $display ("%b errors in %d lines", num, line_num);
        end       
    end
endmodule
```

Output:

![image](https://github.com/user-attachments/assets/0b6e2e12-9612-4036-89e6-a44999fbd831)



Waveform:

![image](https://github.com/user-attachments/assets/bc611695-f3ba-45a9-9f4a-546c1b0aebe9)


### Behavioral modeling of 2-to-4 decoder

top.sv
```sv
`timescale 1ns / 1ps
module top(en, S1, S0, Out[3:0] );
    
    input logic en, S1, S0;
    output logic [3:0]Out;
    logic [1:0]inp;
    
    always@(en,S1,S0) 
    begin
        inp = {S1, S0};
        if (~en) begin
            Out = 4'b0000;
        end
        else begin
            case (inp)
                2'b00: Out = 4'b1110;
                2'b01: Out = 4'b1101;
                2'b10: Out = 4'b1011;
                2'b11: Out = 4'b0111;
                default: Out = 4'b0000;
            endcase
        end
    end
    
endmodule

```

I use a file based testbench here, so I declare a memory file, tf.mem

tf.mem
```
0, 0, 0, 0000
0, 0, 1, 0000
0, 1, 0, 0000
0, 1, 1, 0000
1, 0, 0, 1110
1, 0, 1, 1101
1, 1, 0, 1011
1, 1, 1, 0111
```

tb.sv
```sv
`timescale 1ns / 1ps

module tb;
    logic en, S1, S0;
    logic [3:0]Out;
    logic [3:0]Outtest;
    top DUT(en, S1, S0, Out);
    
    integer tf;
    initial begin
        tf = $fopen("tf.mem", "r");
        if (tf == 0) $fatal("ERROR: Could Not Open Test File");
    end
    
    integer line_num = 0;
    integer num = 0;
    initial begin 
        while (!$feof(tf)) begin 
            line_num++;
            $fscanf(tf, "%b, %b, %b, %b", en, S1, S0, Outtest[3:0]);
            #10;
            $display ("en = %0b S1 =%0b S0 = %0b Out = %4b", en, S1, S0 , Out);
            if (Out == Outtest) begin
                $display("Test Case Passed");
            end
            
            else begin
                num++;
            end
        end 
        
        if (num == 0) begin
            $display ("All test passed!");
        end
        else begin
            $display ("%b errors in %d lines", num, line_num);
        end       
    end
endmodule

```

Output:


![image](https://github.com/user-attachments/assets/294ab009-f984-4e27-80c6-5dfa491c6032)



Waveform:

![image](https://github.com/user-attachments/assets/92ad9692-b9b6-433b-b945-eae441f3d16e)


