## Day 4
### Timing modeling using delay tables
#### Lab steps to convert grid info to track info:
1.Certain guidelines to follow while making a std cell:

- The input and output ports must lie on the intersection of the vertical and the horizontal tracks.
- The width of the std cell should be odd multiples of track pitch and the height should be odd multiples of track vertical pitch.
- 
2.To view the track file:
```
 cd Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/
 less tracks.info
```
![D4_lef](https://github.com/GauthamMulay/pes_pd/assets/113660503/2ceeb571-b8e0-43c9-a687-6d8fd2087da8)

3.To converge the grid definitions to according to track definitions, in the tkcon window type:
```
grid 0.46um 0.34um 0.23um 0.17um
```
![D4_gridcmd](https://github.com/GauthamMulay/pes_pd/assets/113660503/53431820-74f4-4645-afd3-15320827c475)
![D4_grid](https://github.com/GauthamMulay/pes_pd/assets/113660503/eb9caec0-34fe-4035-b3d3-0084746adc28)


4.To write the inverter into a lef file use command 
```
write lef
```
![D4_leffile](https://github.com/GauthamMulay/pes_pd/assets/113660503/ecef7bd1-ef6c-4c55-9cb7-a46452439e99)

**Synthesis Using the designed Inverter**

1)Copy the lef file created to the directory of picorv32a using the command
``` bash
cp sky_130_vsdinv.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
```
2)now copy the lib files into the src folder using command
``` bash
cp sky130_fd_sc_hd__* /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
```
3)Modify the config file in the picorv32a as follows
``` bash=?

# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILE) "./designs/picorv32a/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "12.000"
set ::env(CLOCK_PORT) "clk"


set ::env(CLOCK_NET) $::env(CLOCK_PORT)


set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1} {
	source $filename
}

```
4)Run the synthesis 

![D4_synthesis](https://github.com/GauthamMulay/pes_pd/assets/113660503/1ae366bc-dd06-4ae3-a42c-24f25b9c102b)

![D4_syn_succ](https://github.com/GauthamMulay/pes_pd/assets/113660503/1ba21680-9089-4d72-b065-374289c93a80)


5)Run floorplan and routing and view the schematic by commands given below:

![D4_echo](https://github.com/GauthamMulay/pes_pd/assets/113660503/af8a67c4-922b-4ea2-bd0f-b786b3c491c1)

![D4_placement](https://github.com/GauthamMulay/pes_pd/assets/113660503/6883f31a-2dbc-48cf-93e9-962bb5c22e26)

![D4_placement_p1](https://github.com/GauthamMulay/pes_pd/assets/113660503/9902b8a9-59b3-40a5-838a-59991c7ae891)

### Timing analysis with ideal clocks using openSTA:
#### Setup time analysis and introduction to flip-flop setup time:
- Timing analysis with ideal clocks is a fundamental aspect of digital circuit design and verification.
- "Ideal clocks" refer to clock signals that are assumed to have zero skew and zero jitter, simplifying the analysis by disregarding real-world clock signal imperfections.
- The primary objective of timing analysis with ideal clocks is to ensure that a digital design operates correctly within specified timing constraints.
- This analysis involves assessing whether signals meet setup and hold time requirements, as well as verifying that maximum clock-to-q delays in flip-flops or latches are not exceeded.
#### Introduction to clock jitter and uncertainty:
- Clock jitter can result from various sources, including electronic noise, power supply fluctuations, and signal interference.
- Clock jitter refers to the deviation or variability in the timing of a clock signal from its ideal, periodic waveform.
- On the other hand, clock uncertainty encompasses various sources of timing variability, including jitter, but also factors like clock skew and clock-to-clock variations.
     + Clock Skew: Clock skew refers to the difference in arrival times of the clock signal at various points in the system. It can result from variations in trace lengths or delays in clock distribution networks.
    + Clock-to-Clock Variations: Clock-to-clock variations account for differences between multiple clock domains within a system. These variations can affect the synchronization and data transfer between different parts of a design.
      +Jitter and Phase Noise: Clock jitter contributes to clock uncertainty by introducing variations in the clock's rising and falling edges. Phase noise, a type of jitter, impacts the clock's phase and can affect communication systems' spectral purity and data integrity.
      + Temperature and Voltage Variations: Changes in temperature and supply voltage can affect the clock's frequency and jitter characteristics. These variations can be especially important in mobile devices and other systems exposed to varying environmental conditions.
        
### Clock tree synthesis tritonCTS and signal integrity:


#### Clock tree routing and buffering using H-tree algorithm:
* Clock tree routing and buffering using the H-tree algorithm is a common technique in digital integrated circuit design.
* The H-tree is a specific topology used to distribute clock signals efficiently and evenly to various parts of the chip while minimizing clock skew.
  + H-Tree Topology: H-tree topology is used for clock distribution, resembling the letter "H" with a balanced hierarchical structure.
  + Buffer Insertion: Clock buffers are inserted along the clock tree to maintain signal integrity and compensate for signal attenuation.
  + Balanced Routing: The routing process aims to balance clock tree branches to minimize delays and clock skew.
  + Clock Skew Minimization: H-tree structures inherently minimize clock skew, ensuring consistent clock arrival times.
  + Timing Analysis: Post-routing timing analysis verifies that the clock tree meets setup and hold time constraints.
  + Optimization: Iterative optimization may involve buffer sizing, placement changes, or re-routing to meet timing goals.
  + Verification: Clock tree verification ensures the design meets power, performance, and reliability requirements.
  + Iterative Refinement: Designers iterate on routing and buffering to resolve timing violations if detected during verification.
### Crosstalk and clock net shielding:
* Crosstalk is unwanted interference between adjacent signal traces, causing signal distortion or corruption.
* Types of Crosstalk: Common types include capacitive and inductive crosstalk, affecting signal integrity.
* Techniques like spacing, shielding, and differential signaling can reduce crosstalk effects.
* Clock net shielding involves isolating clock signals to minimize interference and crosstalk.
### Labs steps to run CTS using tritonCTS:
* To run the CTS type the command: `run_cts`
  
![D4_cts](https://github.com/GauthamMulay/pes_pd/assets/113660503/a0034118-7fbd-4e16-b40c-1a9f53363828)


