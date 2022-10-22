
[Verilog Code](#1 - Verilog Code for 2-bit Full Adder)
(#2 - Test Bench for 2-bit Full Adder)

#1. Verilog Code for 2-bit Full Adder :

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

Verilog Code for Test Bench :

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

Syntax Used in Code :
