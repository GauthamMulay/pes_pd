# Day5

## Power Distribution Network and Routing

**Build Power Distribution Network**

- To do this first we type
```
gen_pdn
```
<img width="485" alt="image" src="https://github.com/GauthamMulay/pes_pd/assets/113660503/6b0ce03a-8041-4d39-8e72-6ebd54a607d8">


<img width="341" alt="image" src="https://github.com/GauthamMulay/pes_pd/assets/113660503/a3741f25-24dd-4204-ab4a-c3235eb7bc6d">


- We see that there is a change in the DEF.
- To run the rounting we type
  ```
  run_routing
  ```
- To check for DRC errors we need to check the 'tritonRoute.drc' folder
- To extract the parasitics we need to use an extractor engine.
- We use the SPEF Extraction.

**SPEF Extraction**
- To use this engine we need to go to
```
cd Desktop/work/tools/SPEF_Extractor
```
- Next we need to use this command
```
python3 /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/18-09_06-12/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/16-09_19-58/results/routing/picorv32a.def
```
- SPEF file is created in ```/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/18-09_06-12/results/routing/```
