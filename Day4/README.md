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

