# MUX-Code
16:1 mux using 2:1 mux 
module mux2(
 input wire a,
 input wire b,
 input wire sel,
 output wire y
 );
 assign y = (sel) ? b : a;
endmodule
module mux16x2(
 input wire [15:0] data,
 input wire [3:0] sel,
 output wire y
 );
 wire [7:0] mux_level1;
 wire [3:0] mux_level2;
 wire [1:0] mux_level3;

 genvar i;
 generate
 for (i = 0; i < 8; i = i + 1) begin : level1
 mux2 u1 (
 .a(data[2*i]),
 .b(data[2*i+1]),
 .sel(sel[0]), 
 .y(mux_level1[i])
 );
 end
 endgenerate
 generate
 for (i = 0; i < 4; i = i + 1) begin : level2
 mux2 u2 (
 .a(mux_level1[2*i]),
 .b(mux_level1[2*i+1]),
 .sel(sel[1]),
 .y(mux_level2[i])
 );
 end
 endgenerate

 generate
 for (i = 0; i < 2; i = i + 1) begin : level3
 mux2 u3 (
 .a(mux_level2[2*i]),
 .b(mux_level2[2*i+1]),
 .sel(sel[2]),
 .y(mux_level3[i])
 );
 end
 endgenerate
 mux2 u4 (
 .a(mux_level3[0]),
 .b(mux_level3[1]),
 .sel(sel[3]),
 .y(y)
 );
endmodule




Using 4:1 mux

module mux4x1 (
 input wire [3:0] data,
 input wire [1:0] sel,
 output wire y
);
 assign y = (sel == 2'b00) ? data[0] :
 (sel == 2'b01) ? data[1] :
 (sel == 2'b10) ? data[2] :
 data[3];
endmodule
module mux16x4 (
 input wire [15:0] data,
 input wire [3:0] sel,
 output wire y
);
 wire [3:0] mux_outputs;
 mux4x1 mux0 (.data(data[3:0]), .sel(sel[1:0]), .y(mux_outputs[0]));
 mux4x1 mux1 (.data(data[7:4]), .sel(sel[1:0]), .y(mux_outputs[1]));
 mux4x1 mux2 (.data(data[11:8]), .sel(sel[1:0]), .y(mux_outputs[2]));
 mux4x1 mux3 (.data(data[15:12]), .sel(sel[1:0]), .y(mux_outputs[3]));
 mux4x1 mux4 (.data(mux_outputs), .sel(sel[3:2]), .y(y));

endmodule
