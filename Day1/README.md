## SoC Design and OpenLANE
**What is a PDK?**

PDK stands for Process Design Kit.
- It is a collection of files used to model a fabrication process for the EDA tools used to design an IC
  - Process Design Rules.
  - Device Models
  - Digital Standard Cell Libraries
  - I/O Libraries
### A simplified RTL to GDSII Flow is :

- Synthesis -> Floor/Power Planning -> Placement -> Clock Tree Synthesis -> Routing -> Signoff
- Synthesis - Converts RTL to a ciruit, out of compomments from the standard cell library.
- Floor and Power Planning - Obejctive here is to plan the silicon area and create robust power distribution network to power the chip.
- Chip Floor Planning - Partition the chip die between different system building blocks and place the I/O pads.
- Macro Floor Planning - We define the macro dimensions, pin locations and rows are defined.
- Power Planning - The power distribution network is contructed.
- Placement - Placing the cells on the floorplan rows, aligned with the sites. There are 2 steps: Global and Detailed.
- Clock Tree Synthesis - To deliver the clock to all sequential elements.
- Routing - Implement the interconnect using the available metal layers.
- Sign Off - Perform physical verification such as DRC(Design Rule Check) and LVS(Layout vs Synthesis). Also perform STA(Static Timiing Analysis).

**OpenLANE ASIC Flow**

<img width="466" alt="image" src="https://github.com/GauthamMulay/pes_pd/assets/113660503/27e8b1a3-edcf-4184-8efc-5ca23c987266">


### Getting fimilarising with OpenSource EDA tools
- To run the openlane in the VM go to the directory ```work/tools/openlane_working_dir/openlane```
- we will be using picorev32a which is present in the directory ```designs/picorv32a```
- initally when we open the file loaction we will not have any runs directory as we havent prepare our design
#### Working with openlane 
- Enter the follow comands to work with openlane pdk and prep the design
  
  #### You should be in the directory
```
  /Desktop/work/openlane_working_dir/openlane
  doker
  ./flow.tcl -interactive
  ```
  ![D1_opelane](https://github.com/GauthamMulay/pes_pd/assets/113660503/57a1577b-092c-4d96-bd8e-9defe01aaf52)

- Now to prep the design run the following command ```prep -design picorev32a``` after running this command we will see a new directry created called as run

- Now to run synthesis run the following command ```run_synthesis```
  
  ![d1_synthesis](https://github.com/GauthamMulay/pes_pd/assets/113660503/8d9d992f-b4a5-4047-a2e0-45a923966dbd)

  
- From these results the flop ratio is calculate by the formula ```number of flops/total number of cells``` in this case it is equal to 0.108
- After running the synthesis there will be a netlist file created under the folder runs

![D1_synpwd](https://github.com/GauthamMulay/pes_pd/assets/113660503/fb149a05-8225-446c-aecf-65f3517bcd1e)
