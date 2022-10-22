2-bit Full Adder

CONTENTS :
1. [Design Code](#1-design-code)
2. [Test Bench](#2-test-bench)
3. [Syntax Used](#3-syntax-used)

#1. Design Code :

module fullAdder (sum_FA, carry_FA, a, b, c);

output sum_FA, carry_FA;
input a, b, c;
wire sum_HA1, carry_HA1, carry_HA2;

halfAdder H1 (sum_HA1, carry_HA1, a, b);
halfAdder H2 (sum_FA, carry_HA2, sum_HA1, c);
or O1 (carry_FA, carry_HA1, carry_HA2);

endmodule

module halfAdder (sum_HA, carry_HA, a, b);

output sum_HA, carry_HA;
input a, b;

xor X1 (sum_HA, a, b);
and A1 (carry_HA, a, b);

endmodule

#2. Test Bench :

module fullAdder_tb;

reg a, b, c;
wire sum, carry;

fullAdder FA1 (.sum_FA(sum), .carry_FA(carry), .a(a), .b(b), .c(c));

initial
begin
	a = 1'b0;
	b = 1'b0;
	c = 1'b0;
end

always
	#5 c = ~c;
always
	#10 b = ~b;
always
	#20 a = ~a;

initial $monitor( $time, "a = %b, b = %b, c = %b, Sum = %b, Carry = %b ", a, b, c, sum, carry);
initial #40 $finish;

endmodule

Simulation Screenshots :

![Screenshot (277)](https://user-images.githubusercontent.com/110777645/197330422-90ae7d19-2653-4348-bc23-dfc8b905d35d.png)
![Screenshot (278)](https://user-images.githubusercontent.com/110777645/197330423-5af1bf5a-e57d-4519-b439-e07abbea3dff.png)
![Screenshot (276)](https://user-images.githubusercontent.com/110777645/197330426-1de989d4-e2a6-421d-ad7b-76377e71dcd8.png)

#3.Syntax Used :

#3.1. Design Code :

In this code, the module for the full adder is first defined. It is declared by the line
"module fullAdder (sum_FA, carry_FA, a, b, c);" and it contains the list of outputs and inputs within paranthesis. Inside the module, the keywords "input" and "output" are used to classify the ports as input and output. The signals which are intermediate results inside the module are declared under "wire" datatype.

As the Full Adder consists of 2 half adders, 2 instances of half adders are instantiated inside the full adder module.

The inputs to the first half adder are 'a' and 'b', whereas the inputs to the second half adder are input 'c' and the sum result of the first half adder stored in the wire "sum_HA1"

The carries of the half adders "carry_HA1" and "carry_HA2" are then ORed using an OR gate to produce the final carry "carry_FA".
Now at the end of the full adder module, an "endmodule" statement is included.
Similar to the module of full adder, a half adder module is declared and defined


#3.2. Test Bench :

In the test bench, a separate module is created to test our DUT. This module need not contain a list of input and outputs as it is just a test file.
The inputs to the DUT module are declared as register data type - as the input value has to be stored until it is changed - and the output variables of the DUT module are declared as wires - as their values have to be calculated continuously.

Next, the DUT module is instantiated in the test bench code.
Under the initial begin block, an initial value is assigned to the inputs "a", "b" and "c". In the always block, the values of the inputs are toggled after 5, 10 and 20 time units so that the output values of the module under test are calculated for all possible inputs. 
A monitor statement is then used to print the values of input and output variables and the simulation is finished after 40 time units.
