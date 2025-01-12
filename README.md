# Digital VLSI SoC Design and Planning 
![vlsi workshop main ss](https://github.com/user-attachments/assets/201ba6f4-0ebe-40b6-b54c-999fdae75a2f)

## Day 1 - Inception of Open-Source EDA, OpenLANE and Sky130nm PDK
Tasks:- 
  1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
  2. Calculate the Flop ratio and the percentage of D-FFs.

Commands to activate the OpenLANE flow and perform synthesis: 
```
# change the directory to OpenLANE flow directory
cd Desktop/work/tools/openlane_woking_dir/openlane

# To run the OpenLANE docker sub-system
docker

# Invoking the OpenLANE flow in the interactive mode
./flow.tcl -interactive

# Required package
package require openlane 0.9

# To prep the design for running the specific 'picorv32a' design
prep -design picorv32a

# To run synthesis
run_synthesis
```
Screenshots of running the commands:
![screenshot 1](https://github.com/user-attachments/assets/a4a605b6-b61d-4a7c-9973-6bee374ce595)
![screenshot 2](https://github.com/user-attachments/assets/031b20c8-2881-4c35-9d37-edc846671048)

Calculation of the Flop Ratio:

Screenshots of the synthesis statistics report file 
![screenshot 3](https://github.com/user-attachments/assets/c6231913-20d0-4c1c-b28d-5b28a7f1c828)

$$
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops\}{Total\ number\ of\ cells\} = \frac{1613}{14876} = 0.108429685
$$

$$
Percentage\ of\ DFF's = 0.108429685 \times 100 = 10.84296854\%
$$

## Day 2 - Good Floorplan vs bad floorplan and introduction to library cells
Tasks:- 
1. Run the 'picorv32a' design floorplan using OpenLANE Flow
2. Calculate the die area in microns from the floorplan def
3. Load the generated floorplan in magic tool

Command for generating floorplan:
```
run_floorplan
```
Screenshots of running placement 
![screenshot 4](https://github.com/user-attachments/assets/f0b056af-63a2-4435-8ab3-e781485eff86)
![screenshot 12](https://github.com/user-attachments/assets/361e5f62-3260-4405-8266-a512de61f25b)

Screenshot of floorplan def
![screenshot 5](https://github.com/user-attachments/assets/bef0b8f9-c9b4-4338-8d27-6f926bd59681)

According to floorplan,  1000 unit distance = 1 micron

$$
Die\ width = 660685
$$

$$
Die\ height = 671405
$$

$$
Die\ width\ in\ microns\ = \frac{660685}{1000} = 660.685\ microns
$$

$$
Die\ height\ in\ microns\ = \frac{671405}{1000} = 671.405\ microns
$$

$$
Area\ of\ die\ in\ microns\ = 660.685 \times 671.405 
$$

Command to load floorplan def in magic tool:-  
```
# Command for opening the floorplan directory 
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/floorplan/

#command for opening the floorplan in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
Screenshots of floorplan def on magic tool:- 
![screenshot 6](https://github.com/user-attachments/assets/6d510bf5-ed8f-44a4-b08a-eecda1cd8a4d)
Vertical and horizontal port cells
![screenshot 7](https://github.com/user-attachments/assets/c2b6eaa3-4272-46d0-990a-d129e11dc3b2)
![screenshot 8](https://github.com/user-attachments/assets/f7e64e7c-0c1c-4090-a872-cee6945a0217)
Decap cells and tap cells
![screenshot 9](https://github.com/user-attachments/assets/200eb6cb-7f10-406c-9379-4ee9d4505213)
Diagonally equidistant tap cells
![screenshot 10](https://github.com/user-attachments/assets/00586a6a-1a47-4b28-8aee-09d9bbcce722)
![screenshot 11](https://github.com/user-attachments/assets/44b484c4-4864-445d-9439-d72aede37d40)




