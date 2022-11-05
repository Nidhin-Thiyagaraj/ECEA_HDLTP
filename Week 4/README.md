# Half Adder

Verilog Code :

  module halfAdder (sum, carry, a, b);

  input a,b;
  output sum, carry;

  xor XOR1 (sum, a, b);
  and AND1 (carry, a, b);

  endmodule


  module halfAdder_tb;

  reg a, b;
  wire sum, carry;

  halfAdder h (.sum(sum), .carry(carry), .a(a), .b(b));

  initial
    begin
      a = 1'b0;
      b = 1'b0;
      #40 $finish;
    end

  always #20 a = ~a;
  always #10 b = ~b;

  always @(a or b)
    #5 $display("a = %b, b = %b, sum = %b, carry = %b", a, b, sum, carry);

  endmodule
    
 ![half adder](https://user-images.githubusercontent.com/110777645/200132849-9c8d1158-8588-454d-bf66-ed74db1e176d.png)

  # Full Adder
  
  module fullAdder (sum_FA, carry_FA, a, b, c);

    output wire sum_FA, carry_FA;
    input wire a,b,c;

    assign sum_FA = a ^ b ^ c;
    assign carry_FA = (a&b)|(b&c)|(c&a);

  endmodule

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

  initial $monitor("a = %b, b = %b, c = %b, Sum = %b, Carry = %b ", a, b, c, sum, carry);
  initial #40 $finish;

  endmodule
  

  ![full adder](https://user-images.githubusercontent.com/110777645/200132828-c6a7a031-e4c3-4e0f-9596-f802ea94166f.png)

  # Multiplexer - 2x1
  
  module mux2to1 (y,i1,i2,s);

  input i1, i2, s;
  output y;
  wire y1, y2;

  not NOT1 (sbar, s);
  and AND1 (y1, i1, sbar);
  and AND2 (y2, i2, s);
  or OR1 (y, y1, y2);

  endmodule

  module mux2to1_tb;

  reg in1, in2, select;
  wire out;

  mux2to1 MUX (.y(out), .i1(in1), .i2(in2), .s(select));

  initial
      begin
          in1 = 1'b0;
          in2 = 1'b0;
          select = 1'b0;
          #80 $finish;
      end

  always
      #10 select = ~select;
  always
      #20 in2 = ~in2;
  always
      #40 in1 = ~in1;

  always @(select)
      #5 $display("in1 = %b, in2 = %b, select = %b, out = %b", in1, in2, select, out);

  endmodule

![mux 2 to 1](https://user-images.githubusercontent.com/110777645/200132815-f4d1e9e6-0ccc-4144-8c56-3003c70d360e.png)

# Demultiplexer 1 to 4

  module dmux1to4 (y0, y1, y2, y3, i, s0, s1);

  input i, s0, s1;
  output y0, y1, y2, y3;

  reg y0, y1, y2, y3;

  always @(i, s0, s1)
  begin
      y0 = 0;
      y1 = 0;
      y2 = 0;
      y3 = 0;
      if({s1, s0} == 0)
          y0 = i;
      else if({s1, s0} == 1)
          y1 = i;
      else if({s1, s0} == 2)
          y2 = i;
      else
          y3 = i;
  end
  endmodule


  module dmux1to4_tb;

  reg s1, s2, in;
  wire y1, y2, y3, y4;

  dmux1to4 DMUX (y1, y2, y3, y4, in, s1, s2);

  initial
      begin
          in = 1'b0;
          s1 = 1'b0;
          s2 = 1'b0;
          #80 $finish;
      end

  always
      #10 s2 = ~s2;
  always
      #20 s1 = ~s1;
  always
      #40 in = ~in;

  always @(in or s1 or s2)
      #5 $display("in = %b, s1 = %b, s2 = %b, y1 = %b, y2 = %b, y3 = %b, y4 = %b", in, s1, s2, y1, y2, y3, y4);

  endmodule

![dmux 1 to 4](https://user-images.githubusercontent.com/110777645/200132805-08af952e-0c00-40d3-915d-2285d4861a5d.png)

# Boolean Expression - X = (A•B) + (A•C) + (A•B•C)

  // X = (A•B) + (A•C) + (A•B•C)

  module calc(x, a, b, c);

  output x;
  input a, b, c;
  wire a1, a2, a3;

  assign x = (a && b) || (a && c) || (a && b && c);

  endmodule

  module calc_tb;

  reg a, b, c;
  wire out;

  calc C (.x(out), .a(a), .b(b), .c(c));

  initial
      begin
          a = 1'b0;
          b = 1'b0;
          c = 1'b0;
          #80 $finish;
      end

  always
      #10 c = ~c;
  always
      #20 b = ~b;
  always
      #40 a = ~a;

  always @(c)
      #5 $display("a = %b, b = %b, c = %b, x = %b", a, b, c, out);

  endmodule

![booleanExpression](https://user-images.githubusercontent.com/110777645/200132791-cc61f500-e0a2-4395-be14-14c87a4634e4.png)

# Encoder - 4x2

  module encoder4x2 (out, in);

  input [3:0] in;
  output [1:0] out;
  input en;

  or (out[1], in[3], in[2]);
  or (out[0], in[3], in[1]);

  endmodule

  module encoder4x2_tb;

  reg [3:0] inp;
  wire [1:0] op;

  encoder4x2 E (op, inp);

  initial
      begin
          inp = 4'b0001;
          #10 inp = 4'b0010;
          #20 inp = 4'b0100;
          #30 inp = 4'b1000;
          #40$finish;
      end

  always @(inp)
      #5 $display(" Input : %b%b%b%b, Output : %b%b", inp[3],inp[2],inp[1],inp[0],op[1],op[0]);

  endmodule

![encoder](https://user-images.githubusercontent.com/110777645/200132782-46e7b432-f9c9-4938-9986-e7d537674878.png)

# Decoder - 2x4

  module decoder4x2 (out, in);

  input [1:0] in;
  output [3:0] out;

  assign out[3] = in[1] && in[0];
  assign out[2] = (!in[0]) && in[1];
  assign out[1] = (!in[1]) && in[0];
  assign out[0] = (!in[0]) && (!in[1]);

  endmodule


  module decoder4x2_tb;

  reg [1:0] inp;
  wire [3:0] op;

  decoder4x2 D (.in(inp), .out(op));

  initial
      begin
          inp = 2'b00;
          #10 inp = 2'b01;
          #20 inp = 2'b10;
          #30 inp = 2'b11;
          #40 $finish;
      end

  always @(inp)
      #5 $display ("Input = %b%b, Output = %b%b%b%b", inp[1], inp[0], op[3], op[2], op[1], op[0]);

  endmodule
  

  ![decoder](https://user-images.githubusercontent.com/110777645/200132772-146d4856-85ed-43ed-a66e-450fed13f7ce.png)

# BCD Adder

  module BCD_Adder (s, c, a, b);

  output [3:0] s;
  output c;
  input [3:0] a, b;

  reg [3:0] s;
  reg c;
  reg [4:0] temp;

  always @(a or b)
  begin
    temp = a + b;
    if(temp > 9)
    begin
      temp = temp + 6;
      c = 1;
      s = temp[3:0];
    end
    else
    begin
      c = 0;
      s = temp[3:0];
    end
  end

  endmodule


  module BCDadder_tb;

  reg [3:0] x,y;
  wire [3:0] s;
  wire c;

  BCD_Adder B (s, c, x ,y);

  initial
      begin
          x = 4'b0101;
          y = 4'b0111;
          #10 x = 4'b0101;
          #10 y = 4'b1000;
          #20 x = 4'b0110;
          #20 y = 4'b1001;
          #30 $finish;
      end

  always @(x or y)
      #5 $display("x = %d, y = %d, x+y = %b%d", x,y,c,s);
  endmodule
  
  ![bcd adder](https://user-images.githubusercontent.com/110777645/200132760-763f1f68-9acc-4a36-b3a8-7c230df55ad4.png)
