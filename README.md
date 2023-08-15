<details>
 <summary> <strong> DAY 0</strong> </summary>
<details>
 <summary> Summary </summary>
 Did the installation of all the required tools.

</details>	
	
 <details>
 <summary> Yosys </summary>
Installed Yosys using the following commands:
     
```
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys-master 
$ sudo apt install make 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make 
$ sudo make install
```
     
Below is the screenshot showing sucessful launch:
<img width="813" alt="Screenshot 2023-07-31 at 10 20 20 AM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/19fb74cb-29dd-4a9a-ab16-5cdf595536ad">

</details>
<details>  
<summary> Iverilog </summary>
    
Installed iverilog using the following command:

```
sudo apt-get install iverilog
```

Below is the screenshot showing sucessful launch:
<img width="813" alt="Screenshot 2023-07-31 at 10 20 20 AM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/19fb74cb-29dd-4a9a-ab16-5cdf595536ad">
</details>
<details>  
    
<summary> Gtkwave </summary>
Installed iverilog using the following command:

```
sudo apt-get install gtkwave
```
Below is the screenshot showing sucessful launch:
<img width="1470" alt="Screenshot 2023-07-31 at 10 13 21 AM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/4bec3f01-5140-48b4-9096-e78c65e40e1e">
</details>
</summary>
</details>






<details>
<summary><strong>DAY 1</strong></summary>

<details>
 <summary> Summary </summary>

This section shows how I simulated and synthesized a 2x1 mux using iverilog and yosys respectively. iverilog generates from the RTL design and its testbench a value changing dump file (vcd). gtkwave is the tool used to plot the simulation results of the design. Yosys is a tool which synthesizes RTL designs into a netlist. It is also used to test the synthesized netlist when we provide it with a testbench.

</details>	
	
<details>
 <summary> Verilog codes </summary>
The verilog codes of the 2x1 mux (good_mux.v) and its testbench (tb_good_mux.v) are taken from https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

</details>

 <details>
 <summary> Simulation: iverilog and gtkwave </summary>
 
 I used the following commands to simulate and view the plots of the RTL design:
	
 ```bash
 iverilog <name verilog: good_mux.v> <name testbench: tb_good_mux.v>
 ./a.out
 gtkwave tb_good_mux.vcd
 ```
	
 Below is the screenshot of the gtkwave plots:
	
<img width="997" alt="Day1-3" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/af19dda0-a2fe-451e-90a2-67c8787b55ee">

 </details>

<details>
 <summary> Synthesis: Yosys </summary>
	
 In the directory of the verilog files, I used the following commands to synthesize and view the synthesized deisgn:
	
 ```bash
yosys> read_liberty -lib <path to lib file>
yosys> read_verilog <path to verilog file>
yosys> synth -top <top_module_name>
yosys> abc -liberty <path to lib file>
yosys> show
 ```
 Below is the screenshot of the synthesized design:
	
<img width="676" alt="synthesis_design" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/3959ec90-02d3-4a2e-8003-17b32dc275e8">


	
 I used the following commands to generate the netlist:
 ```bash
 yosys> write_verilog <file_name_netlist.v>
 yosys> write_verilog -noattr <file_name_netlist.v>
 ```
 
Generated Netlist
<br><br>

```
/* Generated by Yosys 0.23 (git sha1 7ce5011c24b) */

module good_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule
```
</p>

 </details>
</details>




 



<details>
	<summary><strong>DAY 3</strong></summary>

 <details>
 <summary> Summary </summary>

Synthesis of projects with optimizations. Combinational logic optimizations include:<br>
1-) Constant propagation (when the combination is just constant propagation)<br>
2-) Boolean logic optimization (when boolean rules are used to simplify an expression).<br>

Sequential logic optimizations include:<br>
1-) Sequential constant propagation (when the constant is propagated with the clock enabled<br>
2-) State optimization (when unused states are optimized<br>
3-) Retiming (when the logic is split by shortening the time ofother logics becomes). divide and increase frequency) <br>
4-) Sequential logical cloning (when mindful physical synthesis is performed to optimize the moving plane)<br>
</details>	
 
 <br>

<details>
 <summary> Verilog codes </summary>

The verilog codes used (opt_*, dff_const*, tb_dff_const*, and counter_opt*) are taken from https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

</details>

<details>
 <summary> Combinational logic optimizations: opt_check.v </summary>
I used the below commands to view the synthesized design of opt_check.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check.v>
yosys> synth -top <name: opt_check>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design, as we can see a 2-input and gate is realized as was expected when optimizations are applied:
	
<img width="676" alt="opt_check" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/349b4ade-b014-41ca-9e91-54a4c9e0031c">


Screenshots from Termianl:<br>
<img width="644" alt="Screenshot 2023-08-15 at 5 57 45 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/c78b11b3-485a-4d4f-af15-e8f21202c54e">
<br>
<img width="818" alt="Screenshot 2023-08-15 at 12 18 53 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/ba3b35ee-4f86-45b4-8417-fc2a232d533e">
<br>
<img width="960" alt="Screenshot 2023-08-15 at 6 00 07 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/a7b70cc6-4735-4fd5-bcdf-624467c40855">
<br>
<img width="598" alt="Day3 last" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/6654da4f-353d-422a-afe6-ea01e0d55473"><br>
</details>


<details>
 <summary> Combinational logic optimizations: opt_check2.v </summary>
	I used the below commands to view the synthesized design of opt_check2.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check2.v>
yosys> synth -top <name: opt_check2>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
Below is the screenshot of the obtained optimized design, as we can see a 2-input or gate is realized as was expected when optimizations are applied:
	
<img width="676" alt="opt_check1" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/8c8156e3-ddc7-48d8-9021-276747bb4f0d">

Screenshots from Terminal:<br>
<img width="791" alt="Screenshot 2023-08-15 at 12 27 04 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/09d823f7-0eb5-4e9f-8ec9-fd245b046f58">
<br>
<img width="1100" alt="opt2-2" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/fc5090bd-37c5-460a-a4d2-93e11b25f255">
<br>
<img width="613" alt="opt2-last" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/b7525fdb-4f2c-47d0-ba1a-9224064a2d24">
<br>
</details>



<details>
 <summary> Combinational logic optimizations: opt_check3.v </summary>
	
I used the below commands to view the synthesized design of opt_check3.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check3.v>
yosys> synth -top <name: opt_check3>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design, as we can see a 3-input and gate is realized as was expected when optimizations are applied:
	
<img width="444" alt="opt_check3" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/1fa4e553-835a-4f6e-a6ec-4db72ef88a0b">

Screenshots from Terminal:<br>
<img width="1238" alt="opt3" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/1bcc3233-0777-421f-af07-9637ca728c11">
<br>
<img width="1165" alt="opt3-2" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/2eeb51ec-af92-49d3-8e50-fdbe85876d78">
<br>
<img width="617" alt="opt3-3" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/2107f9ea-e218-4125-a504-65e2ceab6904">
<br>

</details>


<details>
 <summary> Combinational logic optimizations: opt_check4.v </summary>
	
I used the below commands to view the synthesized design of opt_check4.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check4.v>
yosys> synth -top <name: opt_check4>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design, as we can see a 2-input xnor gate is realized as was expected when optimizations are applied:
	
<img width="474" alt="opt_check4" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/932eeffc-52a7-4eef-b276-dc35be519074">

Screenshots from Terminal:<br>
<img width="838" alt="opt4-1" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/ac8b1378-dd8e-4050-a280-60bc5960adcb">
<br>
<img width="1178" alt="opt4-2" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/25f3442e-2382-4bf2-9a7c-f3d9af2f5807">
<br>
<img width="647" alt="opt4-3" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/e50e504b-44e7-4017-b9ba-2e96e3c5d038">
<br>
<img width="960" alt="opt 4-4" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/70cfa066-53fc-4122-ac23-bdad84fe8565">
<br>
</details>


</details>

<details>
 <summary> <strong> DAY 4</strong> </summary>
 
<details>
 <summary> Summary </summary>

I have performed Gate Level Simulation (GLS). GLS is when the testbench is run with the netlist as design under test to ensure there are no synthesis and simulation mismatches, and it is important as it 1-) verifies the logical correctness of the post-synthesis design and 2-) ensures the timing of design is met. Synthesis and simulation mismatches can happen due to a lot of reasons including missing sensitivity list (some signal changes are not captured by the circuit because they are missing from the sensitivity list), blocking vs non-blocking assignments (inside an always block, "=" statements inside it are blocking meaning they are executed in order they are written, assignments (<=) on the other hand are non-blocking so they are executed in parallel => non-blocking should be used with sequential circuits. Note that the synthesis will yield same circuit with blocking and non-blockin; it will yield what would be obtained as if the statements where written in non-blocking format, so in case they weren't written as such a mismatch will occur with the simulation), and non-standard verilog coding.
	
</details>
	
<details>
 <summary> Verilog codes </summary>
The verilog codes (*_mux.v and blocking_caveat.v) are taken from https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

</details>
	
<details>
 <summary> Simulation, synthesis, and GLS: ternary_operator_mux.v </summary>

I used the below commands to simulate the design of ternary_operator_mux.v:
	
```bash
iverilog <name verilog: ternary_operator_mux.v> <name testbench: tb_ternary_operator_mux.v>
./a.out
gtkwave tb_ternary_operator_mux.vdc
```	

Below is the screenshot of the obtained simulation, we can see that when sel is high y follows i1, and when sel is low y follows i0:

<img width="997" alt="day4-0" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/eb01f81f-b3e0-465b-942c-e158302446e8">

I used the below commands to synthesize the design into a netlist and view the synthesized design of ternary_operator_mux.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: ternary_operator_mux.v>
yosys> synth -top <name: ternary_operator_mux>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <name of netlist: ternary_operator_mux_net.v>
yosys> show
```
	
Below is the screenshot of the obtained design:

<img width="442" alt="ternary_operator_mux_synth" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/73846fac-7996-4f80-994c-a2f6595da14c">

<br>	
<h4>Netlist Obtained</h4>
<br>

```
/* Generated by Yosys 0.23 (git sha1 7ce5011c24b) */

module ternary_operator_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule

```


I used the below commands to carry out GLS of ternary_operator_mux.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to sky130_fd_sc_hd__tt_025C_1v80.lib: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <name netlist: ternary_operator_mux_net.v> <name testbench: tb_ternary_operator_mux.v>
./a.out
gtkwave tb_ternary_operator_mux.vdc
```	
	
	
</details>

<details>
 <summary> Simulation, synthesis, and GLS: bad_mux.v </summary>

I used the below commands to simulate the design of bad_mux.v:
	
```bash
iverilog <name verilog: bad_mux.v> <name testbench: tb_bad_mux.v>
./a.out
gtkwave tb_bad_mux.vdc
```	

Below is the screenshot of the obtained simulation, we can see that when inputs change, y is not evaluated which is wrong behavior:

<img width="778" alt="bad__mux" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/92be8263-5116-4230-9a5e-99515a521e40">


I used the below commands to synthesize the design into a netlist and view the synthesized design of bad_mux.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: bad_mux.v>
yosys> synth -top <name: bad_mux>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <name of netlist: bad_mux_net.v>
yosys> show
```
	
Below is the screenshot of the obtained design:

<img width="430" alt="Screenshot 2023-08-15 at 9 42 20 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/9e667311-821c-410b-8ef4-681b3db4a62f">

	
	
I used the below commands to carry out GLS of bad_mux.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to sky130_fd_sc_hd__tt_025C_1v80.lib: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <name netlist: bad_mux_net.v> <name testbench: tb_bad_mux.v>
./a.out
gtkwave tb_bad_mux.vdc
```	
	
Below is the screenshot of the obtained simulation, and this mismatches with pre-synthesis simulation:
	
<img width="566" alt="Screenshot 2023-08-15 at 9 45 07 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/21b7ff38-c823-454e-abd3-16c7d82a92a8">


	
</details>

<details>
 <summary> Simulation, synthesis, and GLS: blocking_caveat.v </summary>

I used the below commands to simulate the design of blocking_caveat.v:
	
```bash
iverilog <name verilog: blocking_caveat.v> <name testbench: tb_blocking_caveat.v>
./a.out
gtkwave tb_blocking_caveat.vdc
```	

Below is the screenshot of the obtained simulation, and as we can see d is seeing the precious values, and hence it is acting as if there was a flop in the circuit which is not the case (incorrect behavior):

<img width="993" alt="Screenshot 2023-08-15 at 9 49 51 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/37f143f5-bc4f-4c71-a5f0-6a29cd1c2c50">

I used the below commands to synthesize the design into a netlist and view the synthesized design of blocking_caveat.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: blocking_caveat.v>
yosys> synth -top <name: blocking_caveat>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <name of netlist: blocking_caveat_net.v>
yosys> show
```
	
Below is the screenshot of the obtained design:

<img width="427" alt="Screenshot 2023-08-15 at 9 50 16 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/2064efa2-c126-4eb5-b82f-7b6505b7d16d">


I used the below commands to carry out GLS of blocking_caveat.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: blocking_caveat_net.v> <name testbench: tb_blocking_caveat.v>
./a.out
gtkwave tb_blocking_caveat.vdc
```	
	
Below is the screenshot of the obtained simulation, and this mismatches with pre-synthesis simulation due to blocking statement:
	
<img width="993" alt="Screenshot 2023-08-15 at 9 49 51 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/37f143f5-bc4f-4c71-a5f0-6a29cd1c2c50">
	
</details>
 </details>


<details>
	
<summary><strong>DAY 5</strong></summary>

  <details>
	  <summary><strong>Incomplete If</strong></summary>
	  <h2>Incomplete if</h2>
	  <h3> Design-1 Verilog Code</h3>

```
module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
end
endmodule

```
<p>Here action corresponding to else condition is not specified so output will remain same when if condition is not met and a latch will be inferred.</p>
<h3>Waveform</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260558368-7655ac13-3d1c-40e4-a7dd-a59953bdee11.png">
</div>
<br>
<p>If i0 is 1 , y follows i1 but otherwise it remains the same.</p>
<br>
<h4>Below is the screenshot of the Components Inferred</h4>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260558356-3d69c7a5-1fad-4715-bb9c-3b2440dcc2a2.png">
</div>
<h4>Below is the screenshot of the obtained Synthesis</h4>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260558365-b4c88c2d-d5fc-408b-9d39-a3cd35c8d5c0.png">
</div>

<br>
<h4>Design-2 Verilog Code</h4>

```

module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
	else if (i2)
		y <= i3;

end
endmodule

```
<h4>Waveform :</h4>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260563487-6698f39b-2399-4c71-a4f9-0c4629b19332.png">
</div>
<br>
<p>When i0 is 1 output y follows i1.</p>
<br>
<p>When i0 is 0 and i2 is 1 output y follows i3.</p>
<br>
<p>When i0 = 0 and i2= 0 output y remains the same.</p>

<h4>Below is the screenshot of the Components Inferred</h4>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260563479-9f4e0e11-9b90-464e-9284-fe8d9964a733.png">
</div>
<br>

<h4>Output Circuit</h4>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260563485-2f0c632b-9e8c-4caa-864d-a367fa47ac1e.png">
</div>
<br>


  </details>
  
  <details>
	  <summary><strong>Incomplete Case</strong></summary>
          <h2>Various Case Statements</h2>
	  <h4>Verilog Code</h4>

 ```

module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
	endcase
end
endmodule

```
<br>
<p>Action corresponding to sel = 10 and sel = 11 is not specified so ouput remains unchanged and a latch is inferred.</p>
<br>
<h3>Waveform</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260573152-af3bfbd4-b434-4e79-ad90-bc99002adc0f.png">
	
</div>
<br>
<h3>Components Inferred</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260573136-aa2646cc-aa50-434e-a88b-946f7e7f8a2f.png">
	
</div>
<br>
<h3>Output Circuit</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260573147-e7edee64-eac8-411e-8ee4-b43fc6d3c4cb.png">
	
</div>
<br>

 <h3>Design 2 Verilog Code</h3>

 ```



module comp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
		default : y = i2;
	endcase
end
endmodule

```
<br>

<br>
<h3>Waveform</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260581582-7667b7cd-8668-432b-9239-49bbfee8e831.png">
	
</div>
<br>
<h3>Components Inferred</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260581573-b7f3c6db-5136-45ab-9f4f-93a1acc0c6df.png">
	
</div>
<br>
<h3>Output Circuit</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260581579-5cfaa54a-503a-4084-b890-210cef96845a.png">
	
</div>
<br>
<p>In this RTL, case statement is specified completely so no latch is inferred.</p>	
<br>
 <h3>Design 3 Verilog Code</h3>

 ```
module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
always @ (*)
begin
	case(sel)
		2'b00 : begin
			y = i0;
			x = i2;
			end
		2'b01 : y = i1;
		default : begin
		           x = i1;
			   y = i2;
			  end
	endcase
end
endmodule




```
<br>

<br>
<h3>Waveform</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260618651-341e2c05-fe92-40be-82e7-1a51424ddb54.png">
	
</div>
<br>
<p>The value of x remains unchanged for the period when sel = 01.</p>
<br>
<h3>Components Inferred</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260618641-0f009e41-472e-4eea-9d5c-4f3ec7a595ac.png">
	
</div>
<br>
<h3>Output Circuit</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260618647-9c418c26-8de4-4368-9ebe-d1664514edbd.png">
	
</div>
<br>

<br>
 <h3>Design 4 Verilog Code</h3>

 ```

module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
always @(*)
begin
	case(sel)
		2'b00: y = i0;
		2'b01: y = i1;
		2'b10: y = i2;
		2'b1?: y = i3;
		//2'b11: y = i3;
	endcase
end

endmodule


```
<br>
<p>For sel = 10 we have two matching conditions which is not a good coding practice and produces errorneous results as we see in following text.</p>

<br>
<h3>Waveform of RTL SImulation</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260622555-1c29e6d8-cb8c-4df5-a4c7-ac89613d412d.png">
	
</div>
<br>

<br>
<h3>Components Inferred</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260622560-697eb737-c7b5-4a44-a4ef-6b61cbe472c0.png">
	
</div>
<br>
<h3>Output Circuit</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260622561-58a23e3d-d288-4d01-b2df-24094fcf830b.png">
	
</div>
<br>

<h3>Waveform of Above Netlist</h3>
<div align = "center">

<img src = "https://user-images.githubusercontent.com/140998787/260622559-6b035481-4c0e-4e21-87b2-8e841ffdf12e.png">
	
</div>
<br>
In the above waveform, for sel = 10 output is mirroring i2 and for sel = 11 output is mirroring i3 whereas incase of RTL simulation waveform output remains constant so it is producing a mismatch.
It is due to a badly written case statement where we have more than one conditions matching for the same input.
<br>

  </details>
  <details>
	  <summary><strong>For Loop and For generate</strong></summary>
	  <h3>Design 1 MUX Verilog Code</h3>

```

module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
wire [3:0] i_int;
assign i_int = {i3,i2,i1,i0};
integer k;
always @ (*)
begin
for(k = 0; k < 4; k=k+1) begin
	if(k == sel)
		y = i_int[k];
end
end
endmodule


```

<h3>Waveform Output</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260643572-be3db5ac-9fa6-4855-8571-bb695ea3db85.png">
</div>

<h3>Components Inferred</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260644536-345441ec-ff0a-48cb-921b-711937524a54.png">
</div>

<h3>Synthesis Output</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260645038-3dea201d-411b-4cb1-b89f-33b742d2a97c.png">
</div>
<br>
<h3>Design 2 DeMux Verilog Code</h3>

```

module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
reg [7:0]y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @ (*)
begin
y_int = 8'b0;
for(k = 0; k < 8; k++) begin
	if(k == sel)
		y_int[k] = i;
end
end
endmodule


```

<h3>Waveform </h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260647219-e83fb946-ea2d-4cbd-abf5-76ae5a7854be.png">
</div>
<br>
<h3>Components Inferred</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260648515-7f020e5a-6d7c-430e-8d12-85513b32ba8d.png">
</div>
<br>
<h3>Synthesis Output</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260648510-76addbab-651c-4b73-bd5b-602f9b413ef0.png">
</div>

<br>
<h3>Design Ripple Carry Adder using generate</h3>

```

module fa (input a , input b , input c, output co , output sum);
	assign {co,sum}  = a + b + c ;
endmodule

module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0]int_co;

genvar i;
generate
	for (i = 1 ; i < 8; i=i+1) begin
		fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
	end

endgenerate
fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));


assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule


```

<h3>Waveform </h3>
<div align = "center">
	<img src = "https://github.com/NiteshIIITB/IIIT_ASIC/assets/140998787/0038dde5-01ec-4ea6-a3e7-d2b493d0eb04">
</div>
<br>
<h3>Components Inferred</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260670384-10128dc7-fd8f-4557-b83f-05a74673ff18.png">
</div>
<br>
<h3>Synthesis Output</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260670388-f28aefae-0fc6-44ca-8432-e23956b2e825.png">
</div>
<br>
<h3>GLS ouput</h3>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260670378-e22dd604-2d96-4e76-9e57-1b55cc6404e2.png">
</div>
<br>
<p>Waveform generated from Netlist is same as the waveform generated by RTL Design Simulation. Hence obtained netlist is appropriate for the defined design.</p>

   
  </details>
  
</details>
 


<h2>References</h2>
 <ul>
<li><a href ="https://github.com/kunalg123/">Kunal Ghosh Github(Mentor)</a></li>
	
 </ul>

