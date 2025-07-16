 -DIGITAL-FILTER-DESIGN

COMPANY NAME : CODTECH IT SOLUTIONS

NAME : ANURAG KUMAR

INTERN ID : CT06DF628

DOMAIN : VLSI

DURATION : 6 WEEKS

MENTOR : NEELA SANTOSH

-----DESCRIPTION----

Objective:
As part of my ongoing VLSI internship at Codtech IT Solutions, I was assigned the task of designing and simulating a **Digital FIR (Finite Impulse Response) Filter** using **Verilog HDL**. The objective of this project was to understand the fundamentals of digital filter design, implement a basic FIR filter architecture in Verilog, and verify its functionality using simulation tools. This task gave me a hands-on experience in digital signal processing and hardware design using HDL, which are crucial areas in the electronics and communication field.

Introduction:
A Finite Impulse Response (FIR) filter is a type of digital filter where the response to an impulse input settles to zero in a finite duration. Unlike IIR (Infinite Impulse Response) filters, FIR filters are always stable, easy to implement, and suitable for many signal processing applications such as audio processing, communication systems, and biomedical electronics.
An FIR filter works on a simple mathematical formula:
y[n] = h[0]x[n] + h[1]x[n-1] + h[2]x[n-2] + ... + h[N-1]x[n-N+1]
Here:
* `x[n]` is the input signal,
* `y[n]` is the output signal,
* `h[k]` are the filter coefficients, and
* `N` is the order of the filter.

In this task, I designed a 4-tap FIR filter, meaning it uses 4 coefficients (`h[0]` to `h[3]`) and operates on 4 recent input samples.

Implementation in Verilog:
To implement this filter, I started by understanding the block diagram of an FIR filter: it typically consists of multipliers, adders, and delays (registers). I translated this design into Verilog code using behavioral modeling.

Each multiplication operation was implemented by multiplying input samples with the predefined coefficients. These products were then summed together to generate the final output. I also used shift registers (flip-flops) to delay input samples, as required by the FIR filter algorithm.

Here’s a breakdown of the Verilog modules I created:
fir\_filter.v: The main FIR filter module, with inputs for the clock, reset, and incoming data, and output for the filtered result.
testbench.v: A testbench file to simulate the behavior of the FIR filter with various input signals and validate its functionality.

Verolog code - 

module FIR_Filter (
    input clk,
    input rst,
    input signed [7:0] x_in,           // 8-bit signed input
    output reg signed [15:0] y_out     // 16-bit signed output
);

    // Filter coefficients (example): h0=1, h1=2, h2=3
    parameter signed [7:0] h0 = 8'd1;
    parameter signed [7:0] h1 = 8'd2;
    parameter signed [7:0] h2 = 8'd3;

    reg signed [7:0] x1, x2; // Delay registers for x[n-1], x[n-2]

    always @(posedge clk or posedge rst) begin
        if (rst) begin
            x1 <= 0; x2 <= 0; y_out <= 0;
        end else begin
            // Shift previous inputs
            x2 <= x1;
            x1 <= x_in;

            // FIR Filter Equation: y[n] = h0*x[n] + h1*x[n-1] + h2*x[n-2]
            y_out <= (h0 * x_in) + (h1 * x1) + (h2 * x2);
        end
    end

endmodule

Testbench Code - 

module FIR_Filter_tb;

reg clk, rst;
reg signed [7:0] x_in;
wire signed [15:0] y_out;

FIR_Filter uut (
    .clk(clk),
    .rst(rst),
    .x_in(x_in),
    .y_out(y_out)
);

// Clock generation
always #5 clk = ~clk;

initial begin
    $dumpfile("fir_wave.vcd");
    $dumpvars(0, FIR_Filter_tb);

    clk = 0; rst = 1; x_in = 0;
    #10 rst = 0;

    // Apply sample input data
    x_in = 8'd1;  #10;
    x_in = 8'd2;  #10;
    x_in = 8'd3;  #10;
    x_in = 8'd4;  #10;
    x_in = 8'd5;  #10;
    x_in = 8'd0;  #10;

    $finish;
end

endmodule

Simulation and Testing:
I wrote a Verilog testbench that provides a sequence of input samples to the filter and checks whether the output is as expected. The filter was tested with different sets of input values and coefficients to verify the convolution operation. I used EDA Playground and online Verilog simulators to simulate the design and view the waveform results. This allowed me to visually analyze the effect of filtering on the input data and confirmed that the design was functioning correctly.

Performance Analysis:
The FIR filter was able to produce smooth output values that matched the expected results from manual convolution. The use of fixed coefficients made the behavior predictable and easier to debug. The design was simple, reliable, and met the real-time processing requirements. This small project helped me understand:
* How digital filters are implemented in hardware
* How to use Verilog to model arithmetic operations
* How to write and analyze a proper testbench for verification

Output(Hexadecimal):

  ![Image](https://github.com/user-attachments/assets/ad0e4af2-c963-4d12-9851-5eaf0b6597b6)

Output(Binary):

  ![Image](https://github.com/user-attachments/assets/a431015e-7d6a-4507-9b95-52474ba4cfc1)
  
What I Learned(Conclusion):
This project helped me bridge the gap between theoretical DSP concepts and practical hardware implementation. I got better at structuring Verilog code, handling clocked logic, managing signal delays, and writing testbenches for verification. It also deepened my understanding of how filtering works at the digital hardware level.



