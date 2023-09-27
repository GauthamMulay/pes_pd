## Day2
### Chip Floor Planning Considerations
**Utilization Factor and Aspect Ratio**
1) Defining width and height of core and die
Consider a netlist with flip-flops and combinational logic. We use combinational logic in terms of blocks to calculate the area.


- We consider a simple netlist with a Launch and Capture Flop. It also has an AND and an OR gate.
- We then convert it into squares since we need appropriate dimensions
  


- Let us consider the areas of the gates and Flops as 1 sq unit


- Clubbing them together, we get an area of 4 square units
- The 'core' section of a chip is where the fundamental logic design is placed.
- The 'die' area contains the core and is a small semiconductor area on which the fundamental circuit is fabricated.



- Now we put the netlist in the 'core' area and check the utilization. Using the formula

  ```Utilization Factor = Area Occupied by the Netlist/Total Area of the Core```
  
- As we can see here, there is 100% utilization, and ```Utilization Factor = 1.```
- In practical scenarios, we don't go for such a high utilization factor.
- The 'Aspect Ratio = Height/Width = 1'.
**Concept of Pre Placed Cells**
- We take the below combinational logic as an example


**Running floorplan**
Use Command ```run_floorplan``` to run the placement 
To view the schematic use command ```magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &```


After Running this command, the placemnt def file is created inside the dir runs/placememt



 Use magic commands to veiw the schematic of placed design

 ``` bash = ?
 magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```


- We can see from the i/o's are placed at equidistance

  

- To Align press 's' and 'v' and to zoom press 'z'
- Instances are this
  
  
**Placement**
  - To run placement run the command ```run_placement```  command to do placement and routing after this a def file is created inside the results directory which is used to view the shcematic


- To open the schematic of placed design runthe following command
  ``` bash=?
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
  ```


- If we zoom in we can see the routing betweeen the instances 
 


**Cell Design and Characterization Flow**
**Cell Design Flow**

- Inputs -> Process design kits(PDKs) : DRC and LVS rules, SPICE models, library and user-defined specs.
- Design Steps -> Circuit Design, Layout Design(Euler Path and Stick Diagram), Characterization.
- Outputs -> CDL(Circuit Description Language), GDSII, LEF, extracted spice netlist(.cir)

**Characterization Flow For Inverter**

 Step-1:Read the model files.
 
 Step-2:Read the extracted SPICE netlist.
 
 Step-3:Recognize the behaviour of the buffer.
 
 Step-4:Attaching the necessary power sources
 
 Step-5:Apply the stimulus, which is the input signal to the circuit.
 
 Step-6:Read the sub-circuit of the inverter.
 
 Step-7:Provide necessary output capacitances.
 
 Step-8:Provide the necessary simulation commands
 
** Timing Characterization**
- slew_low_rise_thr = 20%
- slew_high_rise_thr = 80%
- slew_low_fall_thr = 20%
- slew_high_fall_thr = 80%
- in_rise_thr = 50%
- in_fall_thr = 50%
- out_rise_thr = 50%
- out_fall_thr = 50%
- Propogation delay = time(out_fall_thr) - time(in_rise_thr)
- Transition Time
  - On rise: time(slew_high_rise_thr) - time(slew_low_rise_thr)
  - On fall : time(slew_high_fall_thr) - time(slew_low_fall_thr)
