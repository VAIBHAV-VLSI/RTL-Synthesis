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
	<summary><strong>Day 3</strong></summary>

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


Screenshots from Termianl:
<img width="644" alt="Screenshot 2023-08-15 at 5 57 45 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/c78b11b3-485a-4d4f-af15-e8f21202c54e">

<img width="818" alt="Screenshot 2023-08-15 at 12 18 53 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/ba3b35ee-4f86-45b4-8417-fc2a232d533e">

<img width="960" alt="Screenshot 2023-08-15 at 6 00 07 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/a7b70cc6-4735-4fd5-bcdf-624467c40855">

<img width="598" alt="Day3 last" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/6654da4f-353d-422a-afe6-ea01e0d55473">
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

Screenshots from Terminal:
<img width="791" alt="Screenshot 2023-08-15 at 12 27 04 PM" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/09d823f7-0eb5-4e9f-8ec9-fd245b046f58">

<img width="1100" alt="opt2-2" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/fc5090bd-37c5-460a-a4d2-93e11b25f255">

<img width="613" alt="opt2-last" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/b7525fdb-4f2c-47d0-ba1a-9224064a2d24">

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

Screenshots from Terminal:
<img width="1238" alt="opt3" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/1bcc3233-0777-421f-af07-9637ca728c11">

<img width="1165" alt="opt3-2" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/2eeb51ec-af92-49d3-8e50-fdbe85876d78">

<img width="617" alt="opt3-3" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/2107f9ea-e218-4125-a504-65e2ceab6904">

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

Screenshots from Terminal:
<img width="838" alt="opt4-1" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/ac8b1378-dd8e-4050-a280-60bc5960adcb">

<img width="1178" alt="opt4-2" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/25f3442e-2382-4bf2-9a7c-f3d9af2f5807">

<img width="647" alt="opt4-3" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/e50e504b-44e7-4017-b9ba-2e96e3c5d038">

<img width="960" alt="opt 4-4" src="https://github.com/VaibhavTiwari-IIITB/IIITB-ASIC-Vaibhav/assets/140998525/70cfa066-53fc-4122-ac23-bdad84fe8565">

</details>




<br>
