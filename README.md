# synfifo

// Code your design here
module FIFO(
    input clk, rst, wr_en, rd_en,
    input [7:0] buf_in,
    output reg [7:0] buf_out,
    output reg buf_empty, buf_full,
    output reg [6:0] fifo_counter
);
    // FIFO Memory (64 locations of 8-bit width)
    reg [7:0] buf_mem[63:0];
    reg [5:0] rd_ptr, wr_ptr;  
    // Use 6-bit pointers (0-63)

   // FIFO empty and full conditions
    always @(*) begin
        buf_empty = (fifo_counter == 0);
        buf_full  = (fifo_counter == 64);
    end

  // FIFO Counter Logic
    always @(posedge clk or posedge rst) begin
        if (rst)
            fifo_counter <= 0;
        else if (wr_en && !buf_full && !rd_en)  // Write only
            fifo_counter <= fifo_counter + 1;
        else if (rd_en && !buf_empty && !wr_en) // Read only
            fifo_counter <= fifo_counter - 1;
    end

  // Read Operation
    always @(posedge clk or posedge rst) begin
        if (rst)
            buf_out <= 0;
        else if (rd_en && !buf_empty)
            buf_out <= buf_mem[rd_ptr];
    end

  // Write Operation
    always @(posedge clk) begin
        if (wr_en && !buf_full)
            buf_mem[wr_ptr] <= buf_in;
    end

  // Read and Write Pointers
    always @(posedge clk or posedge rst) begin
        if (rst) begin
            wr_ptr <= 0;
            rd_ptr <= 0;
        end else begin
            if (wr_en && !buf_full)
                wr_ptr <= wr_ptr + 1;
            if (rd_en && !buf_empty)
                rd_ptr <= rd_ptr + 1;
        end
    end
endmodule





// Code your testbench here
// or browse Examples
module tb_FIFO;
  reg clk, rst, wr_en, rd_en;
  reg [7:0] buf_in;
  wire [7:0] buf_out;
  wire buf_empty, buf_full;
  wire [6:0] fifo_counter;

  // Instantiate FIFO
  FIFO uut (
    .clk(clk), 
    .rst(rst), 
    .buf_in(buf_in), 
    .buf_out(buf_out), 
    .wr_en(wr_en),
    .rd_en(rd_en), 
    .buf_empty(buf_empty), 
    .buf_full(buf_full),
    .fifo_counter(fifo_counter)
  );

  // Generate waveform dump
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars(0, tb_FIFO);
  end

  // Clock Generation (10-time unit period)
  initial clk = 0;
  always #5 clk = ~clk; 


  // Stimulus
  initial begin
    // Initialize
    rst = 0; wr_en = 0; rd_en = 0; buf_in = 8'd0;
    
  // Apply reset
    rst = 1; #20; rst = 0;

  // **TEST 1: Write Multiple Values to FIFO**
    wr_en = 1;
    repeat (10) begin // Writing 10 values
      buf_in = $random % 256; #10;
    end
    wr_en = 0;

  // **TEST 2: Read Multiple Values**
    #20;
    rd_en = 1;
    repeat (10) #10; // Read 10 values
    rd_en = 0;

  // **TEST 3: Read from Empty FIFO (Underflow Test)**
    #10;
    rd_en = 1; #10; // Attempt to read when empty
    rd_en = 0;

  // **TEST 4: Fill FIFO Completely**
    wr_en = 1;
    repeat (64) begin
      buf_in = $random % 256; #10;
    end
    wr_en = 0;

  // **TEST 5: Write to Full FIFO (Overflow Test)**
    wr_en = 1; buf_in = 8'd255; #10;
    wr_en = 0;

  

   // End Simulation
    #500;
    $finish;
  end
endmodule


