![Screenshot 2025-05-02 181705](https://github.com/user-attachments/assets/ec6f7529-793e-4c75-bdae-c623943395f2)# Traffic_light_controller_Synthesis

## Aim:

Synthesize Traffic Light Controller design using Constraints and analyse area and Power reports.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

![Screenshot 2025-05-25 205322](https://github.com/user-attachments/assets/29682099-857a-4e55-b1df-4614a3bbce33)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

**DEsign code**

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


**Testbench code**

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

![Screenshot 2025-05-25 204645](https://github.com/user-attachments/assets/97d10f6c-adc8-42b6-8d92-e78eb3776c8f)

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.


• In the terminal, initialise the tools with the following commands if a new terminal is being used.

◦ csh
◦ source /cadence/install/cshrc

![Screenshot 2025-05-25 204924](https://github.com/user-attachments/assets/9eb92260-bff8-484f-a9f2-2cab3fd5385e)

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.

![Screenshot 2025-05-25 205017](https://github.com/user-attachments/assets/ae0991bd-040e-4424-b096-e5f6ceaa3628)

• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

Synthesis RTL Schematic :

![Screenshot 2025-05-25 205135](https://github.com/user-attachments/assets/10e7fc5c-6f5e-49d2-939d-11741a117eac)
![Screenshot 2025-05-25 204629](https://github.com/user-attachments/assets/26d4869a-cda3-4112-a52a-feaad13151e5)


Area report:

![Screenshot 2025-05-25 204659](https://github.com/user-attachments/assets/f3088837-a052-4984-ab2c-65da913ec95f)

Power Report:

![Screenshot 2025-05-25 204850](https://github.com/user-attachments/assets/7e665bae-3c15-4b0c-a5d6-516a37dff1d0)

Result:

![Screenshot 2025-05-02 175122](https://github.com/user-attachments/assets/2deafdf7-9966-42ed-8c99-e0de9925b0cd)
![Screenshot 2025-05-02 181705](https://github.com/user-attachments/assets/5b67471c-b40e-4995-afd7-adf0040dac91)

The generic netlist of Traffic Light Controller has been created, and area, power reports have been tabulated and generated using Genus.
