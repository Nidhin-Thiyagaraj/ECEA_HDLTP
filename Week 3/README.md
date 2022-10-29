
# 2-bit Full Adder using Gate Level, Behavioural and Data Flow modelling styles

1. [Modelling Styles in Verilog](#1-modelling-styles-in-verilog)
2. [Gate Level implementation of Full Adder](#2-gate-level-implementation-of-full-adder)
3. [Behavioural implementation of Full Adder](#3-behavioural-implementation-of-full-adder)
4. [Data Flow Implementation](#4-data-flow-implementation)
5. [Test Bench Code](#5-test-bench-code)

# 1. Modelling Styles in Verilog

## Gate-Level Modelling  :

Gate level modelling is used to model combinational circuits using basic gates and by instantiating other user-defined modules.
Basic gates that can be used in gate-level modelling include and, nor, not, nand, or, xor and xnor.
Gate-level modelling is mostly used to model the lowest level of modules in verilog whose 

## Data Flow Modelling   :

Data flow modelling is also used to model combinational circuits. In this style of modelling basic assignment is carried out by using the assign statement, called as continuous assignment. In continuous assignment, the values are assigned to variables of net data-type.
This type of modelling is used when theexpressions for boolean operations performed by the modules is known.

## Behavioural Modelling :

Behavioural modelling is used to model both combinational and sequential circuits. It makes use of inital and always statements to assign values to registers.
It is used to model sequential and complex combinational circuits. All sequential circuits are modelled using this technique.

# 2. Gate Level implementation of Full Adder

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

	xor X1 (sum_HA, a, b); and A1 (carry_HA, a, b);

endmodule

![Screenshot (277)](https://user-images.githubusercontent.com/110777645/198817089-581876fe-544f-4659-8c3a-32c34421ed17.png)
![Screenshot (276)](https://user-images.githubusercontent.com/110777645/198817085-b0d3f46f-a8b0-4d5a-8f60-6b9e90cc84bc.png)

# 3. Behavioural implementation of Full Adder

module fullAdder (sum_FA, carry_FA, a, b, c);

	output reg sum_FA, carry_FA;
	input wire a,b,c;

	always @( a or b or c )
		begin
			if ( a==0 && b==0 && c==0 )
			     begin
			         sum_FA = 0;
			         carry_FA = 0;
			     end
			else if ( a==0 && b==0 && c==1 )
			     begin
			         sum_FA = 1;
			         carry_FA = 0;
			     end
			else if ( a==0 && b==1 && c==0 )
			     begin
			         sum_FA = 1;
			         carry_FA = 0;
			     end
			else if ( a==0 && b==1 && c==1 )
			     begin
			         sum_FA = 0;
			         carry_FA = 1;
			     end
			else if ( a==1 && b==0 && c==0 )
			     begin
			         sum_FA = 1;
			         carry_FA = 0;
			     end
			else if ( a==1 && b==0 && c==1 )
			     begin
			         sum_FA = 0;
			         carry_FA = 1;
			     end
			else if ( a==1 && b==1 && c==0 )
			     begin
			         sum_FA = 0;
			         carry_FA = 1;
			     end
			else if ( a==1 && b==1 && c==1 )
			     begin
			         sum_FA = 1;
			         carry_FA = 1;
			     end
		end
endmodule

![Screenshot (283)](https://user-images.githubusercontent.com/110777645/198817301-99978fab-9b2b-42ac-afc0-7ee9ec55efb9.png)
![Screenshot (282)](https://user-images.githubusercontent.com/110777645/198817305-4b49a4ea-7ac5-42ad-8288-cf1ece37f88d.png)

# 4. Data Flow Implementation

module fullAdder (sum_FA, carry_FA, a, b, c);

	output wire sum_FA, carry_FA;
	input wire a,b,c;

	assign sum_FA = a ^ b ^ c;
	assign carry_FA = (a&b)|(b&c)|(c&a);

endmodule

![DataFlow - Schematic](https://user-images.githubusercontent.com/110777645/198817277-0bb5ea95-730f-4321-9cd4-108a5e46d385.png)
![DataFlow - Simulation](https://user-images.githubusercontent.com/110777645/198817276-b1123140-8e3a-492d-b65d-17e99bc5e8a3.png)

# 5. Test Bench Code

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
