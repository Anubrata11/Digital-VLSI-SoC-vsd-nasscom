# Digital VLSI SoC Design and Planning 
![vlsi workshop main ss](https://github.com/user-attachments/assets/201ba6f4-0ebe-40b6-b54c-999fdae75a2f)

## Day 1 - Inception of Open-Source EDA, OpenLANE and Sky130nm PDK
Tasks:- 
1. Run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
2. Calculate the Flop ratio and the percentage of D-FFs.

1. Commands to activate the OpenLANE flow and perform synthesis: 
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

2. Calculation of the Flop Ratio:

Screenshots of the synthesis statistics report file 
![screenshot 3](https://github.com/user-attachments/assets/c6231913-20d0-4c1c-b28d-5b28a7f1c828)

$$
Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops\}{Total\ number\ of\ cells\} = \frac{1613}{14876} = 0.108429685
$$

$$
Percentage\ of\ DFF's = 0.108429685 \times 100 = 10.84296854\%
$$
