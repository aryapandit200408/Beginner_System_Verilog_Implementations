# Beginner_System_Verilog_Implementations

This is a repository showcasing some of my beginner implementations using System Verilog for design and verification.

### Dataflow Modelling of an half adder

top.sv
```
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
```
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



