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
4. Run the 'picorv32a' design congestion aware placement in the OpenLANE flow
5. Load the generated placement in the magic tool

#### Command for generating floorplan:
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
Area\ of\ die\ in\ microns\ = 660.685 \times 671.405 = 443587.212425\ square\ microns
$$

#### Command to load floorplan def in magic tool:-  
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

#### Command to run design congestion aware placement:- 
```
run_placement
```
Screenshot of running placement:
![screenshot 13](https://github.com/user-attachments/assets/77b240e5-b43d-4e4c-a841-ae07ce312311)

#### Command to load placement def in magic tool:-
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshot of loading placement in magic tool
![screenshot 14](https://github.com/user-attachments/assets/2d60d025-dacb-471e-bec7-80f4afd24d6d)
![screenshot 15](https://github.com/user-attachments/assets/58c91c72-318e-486b-82e3-d8b2477b500a)

## Day 3 - Design library cell using Magic Layout and ngspice characterization
Tasks:- 
1. Using the IO placer to change the distance between tap cells
2. Clone the standard cell design for an inverter from a github repository
3. Load the invertor layout in magic tool
4. Performing spice extraction of invertor in magic tool
   
#### Command for changing the distance between cells
```
set env(FP_IO_MODE) 2
```
Screenshot of command and new spacing between cells
![screenshot 12](https://github.com/user-attachments/assets/bbaa252d-e752-4acb-aadc-3133ba92952f)
![screenshot 1](https://github.com/user-attachments/assets/42fa8612-840c-4527-af17-6c3006f7f9bc)

#### Commands for creating clone of github repository and opening layout in magic tool
```
# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign.git

# Copy the magic tech file to the directory where repository is present
cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign

# Open the invertor design in magic tool
magic -T sky130A.tech sky130_inv.mag &
```
Screenshots of command run
![screenshot 13](https://github.com/user-attachments/assets/d941bba1-3c58-4053-830d-49398592a2de)
Invertor layout in magic tool
![screenshot 3](https://github.com/user-attachments/assets/ef5b366d-b77f-4ef9-8b70-56f3b3e77112)
PMOS and NMOS identification
![screenshot 4](https://github.com/user-attachments/assets/bee33676-53ed-4d8f-9e7a-b11646772868)
![screenshot 5](https://github.com/user-attachments/assets/7aa428f1-ff81-427b-810c-5ca94c108c45)
Connectivity of Y to drain of PMOS and NMOS
![screenshot 6](https://github.com/user-attachments/assets/25e5a1fa-d76f-4cb4-a074-5b82e2958c1f)
Connection of source of PMOS to Vdd
![screenshot 7](https://github.com/user-attachments/assets/c940d862-789f-444e-9a18-29ae98d3c70e)
Connection of source of NMOS to Ground
![screenshot 8](https://github.com/user-attachments/assets/6aa074b0-790c-4715-bd87-e84c2bcf4d77)
DRC error upon deletion of part
![screenshot 9](https://github.com/user-attachments/assets/113b01f9-0d46-4bc7-a960-3d847a4fe3f5)

#### Commands for SPICE extraction of invertor design in tkcon window 
```
extract all
# This command enables parasitic extraction
ext2spice cthresh 0 rthresh 0
ext2spice
```

Screenshot of running command
![screenshot 10](https://github.com/user-attachments/assets/74467a86-f7ef-4e16-baf9-3791bb70ae64)
Screenshot of SPICE file 
![screenshot 11](https://github.com/user-attachments/assets/eb65e7d2-299a-41c7-bac0-cc0e2488ff70)

