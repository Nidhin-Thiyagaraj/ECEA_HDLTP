
# 2-bit Full Adder using Gate Level, Behavioural and Data Flow modelling styles

# 1. Gate Level implementation of Full Adder

module fullAdder (sum_FA, carry_FA, a, b, c);

output sum_FA, carry_FA; input a, b, c; wire sum_HA1, carry_HA1, carry_HA2;

halfAdder H1 (sum_HA1, carry_HA1, a, b); halfAdder H2 (sum_FA, carry_HA2, sum_HA1, c); or O1 (carry_FA, carry_HA1, carry_HA2);

endmodule

module halfAdder (sum_HA, carry_HA, a, b);

output sum_HA, carry_HA; input a, b;

xor X1 (sum_HA, a, b); and A1 (carry_HA, a, b);

endmodule

![Screenshot (277)](https://user-images.githubusercontent.com/110777645/198817089-581876fe-544f-4659-8c3a-32c34421ed17.png)
![Screenshot (276)](https://user-images.githubusercontent.com/110777645/198817085-b0d3f46f-a8b0-4d5a-8f60-6b9e90cc84bc.png)
