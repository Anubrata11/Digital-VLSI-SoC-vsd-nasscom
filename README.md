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
5. Editing the SPICE model file and graphical simulation
6. Find the error in the magic tech file and fix them
   
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

Dimensions of a unit box in the layout grid
![screenshot 14](https://github.com/user-attachments/assets/cdaf5083-c963-4d28-ad8a-7ee97f150524)
Final edited SPICE file for analysis
![screenshot 15](https://github.com/user-attachments/assets/1f42ef06-6a40-4f71-ae7b-c9d2bfd50494)

#### Commands for ngspice simulation of the SPICE file
```
# To load the SPICE file in the ngspice simulator
ngspice sky130_inv.spice
# To load the plot of the output vs time
plot y vs time a
```

Screenshot of running commands
![screenshot 16](https://github.com/user-attachments/assets/11260a45-2d24-479a-a95c-dfbe34915bac)
Screenshot of generated plot
![screenshot 17](https://github.com/user-attachments/assets/7c4c8de2-3f80-4c06-8e30-469b62bebcda)

Rise transition time calculation:

$$
Rise\ transition\ time\ = Time\ taken\ for\ output\ to\ rise\ to\ 80 \\% - time\ taken\ for\ output\ to\ rise\ to\ 20 \\%
$$

$$ 
80 \\%\ of\ output\ = 2.664\ V
$$

$$
20 \\%\ of\ output\ = 0.666\ V
$$

$$
Therefore,\ rise\ transition\ time\ = 2.2413\ - 2.1802\ = 0.061\ ns\ = 61.1\ ps
$$

Screenshot of rise to 80%
![80 % rise](https://github.com/user-attachments/assets/9c7403e5-ddb4-444c-9276-ce5b031b2ff0)
Screenshot of rise to 20%
![20 % rise](https://github.com/user-attachments/assets/625c487e-6a4b-4fe0-aecb-108eae746b01)
Screenshot of time values
![screenshot 18](https://github.com/user-attachments/assets/b18f3660-ea0f-4bf8-a4f1-a5bf52c8ce58)

Fall transition time calculation:

$$
Fall\ transition\ time\ calculation\ = Time\ taken\ for\ output\ to\ fall\ to\ 20 \\% - Time\ taken\ for\ output\ to\ fall\ to\ 80 \\%
$$

$$
4.09331\ - 4.05024\ = 0.04307\ ns\ = 43.07\ ps
$$

Screenshot of fall to 20%
![20 % fall](https://github.com/user-attachments/assets/72e679c4-4ecb-4301-99ad-f4926c771cb8)
Screenshot of fall to 80%
![80 % fall](https://github.com/user-attachments/assets/4ec664e7-567a-43d1-872a-e4e14165e57c)
Screenshot of time values
![screenshot 19](https://github.com/user-attachments/assets/8ba575ae-5be3-41ba-9c5c-d96b74ed0fdc)

Cell rise delay calculation:

$$
Cell\ rise\ delay\ = Time\ taken\ for\ output\ to\ rise\ to\ 50 \\% - Time\ taken\ for\ input\ to\ fall\ to\ 50 \\%
$$

$$
2.20763\ - 2.14948\ = 0.05815\ ns = 58.15\ ps
$$

Screenshot of rise of output and fall of input to 50%
![50 % for rise delay](https://github.com/user-attachments/assets/14349e30-4d29-4f5b-a4c7-ce96f3a1eff2)
Screenshot of time values
![screenshot 20](https://github.com/user-attachments/assets/a3c14720-1633-41d4-bcc8-91fe9b7b3fc9)

Cell fall delay calculation:

$$
Cell\ fall\ delay\ = Time\ taken\ for\ output\ to\ fall\ to\ 50 \\% - Time\ taken\ for\ input\ to\ rise\ to\ 50 \\%
$$

$$
4.07567\ - 4.04997\ = 0.0257\ ns = 25.7\ ps
$$

Screenshot of rise of input and fall of output to 50%
![50 % for fall delay](https://github.com/user-attachments/assets/f261192a-4f0d-418b-bc60-3c8f801cd5fa)
Screenshot for time values
![screenshot 21](https://github.com/user-attachments/assets/bed08bed-37ba-4958-a873-6fcbca91da7a)

#### Commands to download the folder having corrupted magic layouts
```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
cd drc_tests
gvim .magicrc  # To view the .magicrc file

magic -d XR &  # To open magic tool
```

Screenshots of running commands
![screenshot 22](https://github.com/user-attachments/assets/d24d58b7-8e50-4ff6-86ec-88009719d513)
![screenshot 23](https://github.com/user-attachments/assets/cae2d414-ffbe-42cc-9b2c-29b64b7afa26)
Screenshot of .magicrc file
![screenshot 24](https://github.com/user-attachments/assets/1c9a13e6-27df-498c-9954-41bc236f4357)

```
# Command for seeing the DRC errors
drc why
```

Metal3 design rules
![m3 rules](https://github.com/user-attachments/assets/0ce382b4-82ec-40da-9e28-d2051dbcff8f)
Screenshot of metal3 file loaded onto magic tool
![screenshot 25](https://github.com/user-attachments/assets/e2a38227-0069-4cd6-ac83-251927d540b8)
Violation of design rule m3.4 (Via2 must be enclosed by Met3 by at least 0.065 µm)
![screenshot 26](https://github.com/user-attachments/assets/47a7a287-3f3d-4ae0-8732-93c4903d3d15)

Poly Rules
![poly rules](https://github.com/user-attachments/assets/8c2a9a81-b32b-407b-b932-f3d1421ce986)
Incorrect implementation of rule poly.9 (Poly resistor spacing < 0.48 um)
![incorrect poly 9](https://github.com/user-attachments/assets/f0c12a24-f4cd-427c-9d0b-7420e813ad6d)
![screenshot 27](https://github.com/user-attachments/assets/22711aa2-5211-46fb-b0b0-10b4a36a8c16)

To overcome these errors, we need the following changes to the tech file
![polynonres 1](https://github.com/user-attachments/assets/5ed13de8-cc7c-410a-a005-a1d86072c355)
![polynonres 2](https://github.com/user-attachments/assets/2af582d4-99d5-4a97-ab11-43d76a462e6e)

#### Commands to run in the tkcon terminal
```
tech load sky130A.tech
drc check
drc why
```

Screenshots 
![screenshot 28](https://github.com/user-attachments/assets/7211cd64-cbf8-4329-ab9d-61cfab411bd4)
![screenshot 29](https://github.com/user-attachments/assets/0df8a5cc-5665-4581-bd6b-2255b753900b)

#### Exercise to describe DRC error as a geometrical construct 

Screenshot of nwell rules
![nwell rules](https://github.com/user-attachments/assets/da2476e1-59e4-4b07-8a35-a7c6c420bd3a)
![dnwell rules](https://github.com/user-attachments/assets/baa8f580-3e98-4edf-bc26-8901577aa7fa)

```
# Commands to be written in tkcon terminal
cif ostyle drc
cif see dnwell_shrink
feed clear
cif see nwell_missing
feed clear
```

![screenshot 30](https://github.com/user-attachments/assets/3b666b0e-d305-4de4-a4a2-17e85fe56299)
![screenshot 31](https://github.com/user-attachments/assets/029ea8cb-939f-455d-a241-45e69d66e598)

#### Exercise to find missing or incorrect rules and fixing them

Modification of code inside the tech file:
```
cifmaxwidth nwell_untapped 0 bend_illegal \
  "Nwell missing tap (nwell.4)"
```

Screenshots of the changes 
![final change 1](https://github.com/user-attachments/assets/27068ead-60ff-4dc6-98f5-f41efd412630)
![final change 2](https://github.com/user-attachments/assets/849961df-3119-4962-a3c4-b4da38f759db)

Commands to be given after modification:
```
tech load sky130A.tech
drc style drc(full)
drc check
drc why
```

Screenshot after implementation of rules
![screenshot 32](https://github.com/user-attachments/assets/7926143c-4b82-4d6f-88bd-863fbf7680c6)

## Day 4 - Pre layout timing analysis and importance of good clock tree

#### Fixing DRC errors

Screenshot of tracks.info file
![ss 2](https://github.com/user-attachments/assets/acded65c-cbcc-4471-8e5e-f74d092d5e14)

```
#Command to rectify the errors
grid 0.46um 0.34um 0.23um 0.17um
```
![ss 3](https://github.com/user-attachments/assets/eb42938f-d661-49d2-bbea-e4c4ee3e3555)

Condition 1: The input and output ports of the standard cell should lie on the intersection of the vertical and horizontal tracks
![correction 1](https://github.com/user-attachments/assets/62b29c35-2916-4a87-8778-a125c352601f)

Condition 2: Width of the standard cell should be odd multiples of the horizontal track pitch
![correction 2](https://github.com/user-attachments/assets/aca56f0f-01d5-4850-8bad-a69e2f6ff90a)

Condition 3: Height of the standard cell should be even multiples of the vertical track pitch.
![correction 3](https://github.com/user-attachments/assets/6677c2fa-df76-4955-a0dc-ff4c30eb47b7)

#### Generation of lef from the layout
![ss 5](https://github.com/user-attachments/assets/6ce92ccc-a9ea-43d6-af4a-4e7e98136503)
![ss 6](https://github.com/user-attachments/assets/0424e3e2-c477-4776-a1b9-1ef23b3ca4d9)
Screenshot of lef file
![ss 7](https://github.com/user-attachments/assets/b18282da-d0bf-4b7b-a5cb-276db8a9307a)

Copying lef files and other libs into 'src' directory
![ss 8](https://github.com/user-attachments/assets/d7de7e7a-5d32-45bf-a08b-657fd8750f64)
Edited config.tcl file
![ss 9](https://github.com/user-attachments/assets/8580c28c-8f47-4193-ab1f-af24ebde636f)

#### Commands to run OpenLANE by introducing custom inverter
```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]    # Adiitional commands to include newly added lef to openlane flow
add_lefs -src $lefs
run_synthesis
```

![ss 10](https://github.com/user-attachments/assets/d4a1c2b2-faf6-4325-ab75-fc78770738ad)
![ss 11](https://github.com/user-attachments/assets/0cb6b6eb-43f1-4713-b8d1-274c84511b25)

#### Commands to edit the design parameters to improve timing
```
echo $::env(SYNTH_BUFFERING)
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_DRIVING_CELL)
```
![ss 12](https://github.com/user-attachments/assets/33c62c97-51ad-43d4-8ea4-8ee15b392808)
![ss 13](https://github.com/user-attachments/assets/6db4ed56-b168-4a0e-8ee6-461003c7000a)

Running Floorplan
![ss 15](https://github.com/user-attachments/assets/5ae3b22c-f66f-4100-adba-9fcac006e339)
![ss 16](https://github.com/user-attachments/assets/d3865982-4b9c-4028-8724-f0b5c0347831)
![ss 17](https://github.com/user-attachments/assets/219db7c1-02b8-43bd-8c04-1986f7c5b1af)

Running Placement
![ss 18](https://github.com/user-attachments/assets/be9ae1e9-f7fa-48aa-a170-afce657a0a56)

Screenshot of placement def in magic 
![placement](https://github.com/user-attachments/assets/5588385d-65d2-43cc-88c4-1bf46bbcaecb)
Screenshot of custom inverter
![ss 22](https://github.com/user-attachments/assets/6a328b9d-0381-4b0a-8218-b7809562bb3f)

#### Commands to perform timing analysis using OpenSTA tool
```
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
````

Screenshot of my_base.sdc
![sdc file](https://github.com/user-attachments/assets/f541a1ef-fe70-4f26-8244-a30090af0de8)
Screenshot of pre_sta.conf
![sta file](https://github.com/user-attachments/assets/9075cb59-f5fc-421f-8024-b67605af5243)

#### Commands to generate Clock Tree Synthesis
```
run_cts
```
#### Commands to run the OpenROAD tool in OpenLANE
```
openroad
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/03-07_11-25/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock (all_clocks)
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

Slack at the end of the timing analysis
![cts](https://github.com/user-attachments/assets/5573adcc-a892-44df-81cd-67191a5c77a3)

## Day 5 - Final steps for RTL2GDS using tritonRoute and OpenSTA
```
# Command to create power dristibution network
gen_pdn
```

Screenshot of running pdn
![ss1](https://github.com/user-attachments/assets/400bdd71-6e9e-49cf-a3ea-60ef9a672526)

Commands for running routing
```
echo $::env(CURRENT_DEF)
echo $::env(ROUTING_STRATEGY)
run_routing
```

Commands for performing parasitic extraction using SPEF extractor
```
cd Desktop/work/tools/SPEF_EXTRACTOR
python3 main.py /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/tmp/merged.lef /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/26-03_08-45/results/routing/picorv32a.def
```

## Acknowledgements
1. Kunal Ghosh - Co Founder at VLSI System Design
2. Nickson P Jose
3. Timothy Edwards 
