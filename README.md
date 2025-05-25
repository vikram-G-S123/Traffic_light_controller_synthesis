# Traffic_light_controller_Synthesis

## Aim:

Synthesize Traffic Light Controller design using Constraints and analyse area and Power reports.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

![Screenshot 2025-05-25 205322](https://github.com/user-attachments/assets/9627f133-902b-4a6d-b07b-84da2bfe83ab)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)
**DesignFile**

module traffic_light_controller(
    input clk,             
    input reset,           
    output reg red,        
    output reg yellow,     
    output reg green       
);
parameter RED = 2'b00;
parameter GREEN = 2'b01;
parameter YELLOW = 2'b10;
reg [1:0] state;
always @(posedge clk or posedge reset) begin

    if (reset)
     begin
        state <= RED;  
    end 
    else begin
        case (state)
            RED: state <= GREEN;    
            GREEN: state <= YELLOW; 
            YELLOW: state <= RED;   
        endcase
    end
end
always @(state) begin

    case (state)
        RED:
         begin
            red <= 1;
            yellow <= 0;
            green <= 0;
        end
        GREEN:
         begin
            red <= 0;
            yellow <= 0;
            green <= 1;
        end
        YELLOW: 
        begin
            red <= 0;
            yellow <= 1;
            green <= 0;
        end
    endcase
end
endmodule

**TestCode**

module traffic_light_controller_tb;

reg clk;
reg reset;
wire red;
wire yellow;
wire green;

traffic_light_controller dut (
    .clk(clk),
    .reset(reset),
    .red(red),
    .yellow(yellow),
    .green(green)
);

initial begin

    clk = 0;
    forever #5 clk = ~clk;
end

initial begin

    reset = 1;
    #10;
    reset = 0;
    #100;
    $finish;
end

initial begin

    $dumpfile("traffic_light.vcd");
    $dumpvars(0, traffic_light_controller_tb);
end

endmodule


### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.

![Screenshot 2025-05-25 204645](https://github.com/user-attachments/assets/23a00b3c-a29e-4271-a086-2518a6e83ae3)


### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

![Screenshot 2025-05-25 205322](https://github.com/user-attachments/assets/466033d8-3fe9-4c49-a00e-b1025b7d17a6)


• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being used.

◦ csh
◦ source /cadence/install/cshrc

![Screenshot 2025-05-25 204924](https://github.com/user-attachments/assets/132c6c0a-811f-4f7d-b4bd-d7d67ea1563a)


• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

![Screenshot 2025-05-25 205017](https://github.com/user-attachments/assets/a93131a8-b00e-4e61-a195-bf3c6ad6d64e)


• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

![Screenshot 2025-05-25 205135](https://github.com/user-attachments/assets/fd9157c1-8120-473e-89ab-cfe3c719bb56)


Synthesis RTL Schematic :

![Screenshot 2025-05-02 175122](https://github.com/user-attachments/assets/43582f04-5582-4855-abcf-bff6a3c7f5be)


![Screenshot 2025-05-25 204629](https://github.com/user-attachments/assets/5a639e19-7ad9-452a-afd4-706b3fff1131)


Area report:

![Screenshot 2025-05-25 204659](https://github.com/user-attachments/assets/005e2151-584a-4af2-b8e6-b0fa765a04cb)

Power Report:

![Screenshot 2025-05-25 204850](https://github.com/user-attachments/assets/84733790-9ce4-45ff-ba91-bbe0b630c34c)


Result:

![Screenshot 2025-05-02 181705](https://github.com/user-attachments/assets/db5bf24d-e0d0-49d4-96f7-d9c97cfff5cb)


The generic netlist of Traffic Light Controller has been created, and area, power reports have been tabulated and generated using Genus.
