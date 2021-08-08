# README #
Artifacteval eval repo for sc2021

Each github repo, associated to the Zenodo DOIs, for EscapeVC, SPIN, SWAP, DRAIN, and SEEC/mSEEC contains a script by name: ae_sc2021.py at top level of the repo
This is used to generate the results present in _Figure 8_ of the paper. 

Evaluators need to run it using python ae_sc2021.py

After running the script Evaluators will observe “average_packet_latency” data which is used to create the graph for both 8x8 Mesh and 16x16 Mesh for Bit Rotation, Shuffle and Transpose traffic patterns.

More information is present in the respective repository's README

### DOI information ###
Artifact 1:
Persistent ID (DOI, GitHub URL, etc.)
DOI: 10.5281/zenodo.5171429 <br>
Artifact name:
artifacteval_sc2021


Artifact 2:
Persistent ID (DOI, GitHub URL, etc.)
https://github.com/noc-deadlock/seec <br>
Artifact name:
seec

Artifact 3:
Persistent ID (DOI, GitHub URL, etc.)
https://github.com/noc-deadlock/drain <br>
Artifact name:
drain

Artifact 4:
Persistent ID (DOI, GitHub URL, etc.)
https://github.com/noc-deadlock/swap <br>
Artifact name:
swap

Artifact 5:
Persistent ID (DOI, GitHub URL, etc.)
https://github.com/noc-deadlock/spin <br>
Artifact name:
spin

Artifact 6:
Persistent ID (DOI, GitHub URL, etc.)
https://github.com/noc-deadlock/escape_vc <br>
Artifact name:
escape_vc

Artifact 7:
Persistent ID (DOI, GitHub URL, etc.)
https://www.dropbox.com/sh/uyjuy9mybq8wagu/AAD9YRKEw-nC3XtP73xglTlNa?dl=0 <br>
Artifact name:
dropbox-backup link

--------

### 1.	SEEC ### 

SEEC is a technique to improve performance (reduced average packet latency) with  deadlock freedom guarantee in NoCs.


### 2.	Dependencies ### 

SEEC is implemented in gem5 simulator, it modifies the on-chip simulator: Garnet present inside gem5 simulator. 

### Dependencies to compile and run the code are as follows: ###

* Ubuntu 16.04 LTS:
* sudo apt install build-essential git m4 scons zlib1g zlib1g-dev libprotobuf-dev protobuf-compiler libprotoc-dev libgoogle-perftools-dev python-dev python

### Git: ###
* sudo apt-get install git

### gcc 4.8+: ###
* sudo apt-get install build-essential

### Install scons: ###
* sudo apt-get install scons

### Python 2.7+: ###
* sudo apt-get install python-dev

### Protobuf 2.1+: ###
* sudo apt-get install libprotobuf-dev python-protobuf protobuf-compiler libgoogle-perftools-dev

### M4 macro processor not installed: ###
* sudo apt-get install automake

### 3.	Get Code ####

Each repository is a submodule under this repository

`git clone https://github.com/noc-deadlock/artifacteval_sc2021.git`

SEEC: ${gem5_seec}

DRAIN: ${gem5_drain}

SWAP: ${gem5_swap}

SPIN: ${gem5_spin}

EscapeVC: ${gem5_esc}

### 4.	Compile the code: ###

scons ${gem5_*}/build/Garnet_standalone/gem5.opt -j 9

### 5.	Reproduce/Validate results ###
Below information may be outdated <br>
Please refer to the specific instructions in associated repository 

Build Synthetic: $gem5_seec/scons build/Garnet_standalone/gem5.opt -j9

### Run Synthetic: ###

#### Baseline: #### 
routing algorithm includes: (XY, WestFirst, Adaptive WestFirst, Escape VC)

${gem5_seec}/build/Garnet_standalone/gem5.opt configs/example/garnet_synth_traffic.py --topology=Mesh_XY --num-cpus=64 --num-dirs=64 --mesh-rows=8 --network=garnet2.0 --router-latency=1  --sim-cycles=$cycles --inj-vnet=0 --vcs-per-vnet=${vc_} --seec=0 --one-pkt-bufferless=0 --num-bufferless-pkt=0 --injectionrate=${k} --synthetic=${bench[$b]} --routing-algorithm=${routAlgo} &

#### SEEC: #### 
${gem5_seec}/build/Garnet_standalone/gem5.opt configs/example/garnet_synth_traffic.py --topology=Mesh_XY --num-cpus=64 --num-dirs=64 --mesh-rows=8 --network=garnet2.0 --router-latency=1  --sim-cycles=$cycles --inj-vnet=0 --vcs-per-vnet=${vc_} --seec=1 --one-pkt-bufferless=1 --num-bufferless-pkt=1 --bufferless-router=1 --injectionrate=${k} --synthetic=${bench[$b]} --routing-algorithm=${routAlgo} &

#### mSEEC: #### 
${gem5_seec}/build/Garnet_standalone/gem5.opt configs/example/garnet_synth_traffic.py --topology=Mesh_XY --num-cpus=64 --num-dirs=64 --mesh-rows=8 --network=garnet2.0 --router-latency=1  --sim-cycles=$cycles --inj-vnet=0 --vcs-per-vnet=${vc_} --seec=1 --one-pkt-bufferless=1 --num-bufferless-pkt=1 --bufferless-router=8 --injectionrate=${k} --synthetic=${bench[$b]} --routing-algorithm=${routAlgo} &

#### SWAP: #### 
${gem5_swap}/build/Garnet_standalone/gem5.opt configs/example/garnet_synth_traffic.py --network=garnet2.0 --num-cpus=64 --num-dirs=64 --topology=Mesh_XY --mesh-rows=8 --interswap=1 --whenToSwap=1 --whichToSwap=1 --sim-cycles=100000 --injectionrate=${k} --vcs-per-vnet=${vc_} --no-is-swap=1 --occupancy-swap=0 --inj-vnet=0 --synthetic=${bench[$b]} --routing-algorithm=${r}

#### DRAIN: #### 
${gem5_drain}/build/Garnet_standalone/gem5.opt configs/example/garnet_synth_traffic.py --topology=irregularMesh_XY --num-cpus=64 --num-dirs=64 --mesh-rows=8 --network=garnet2.0 --router-latency=1 --sim-cycle=$cycles --spin=1 --conf-file=${conf} --spin-freq=1024 --spin-mult=1 --uTurn-crossbar=1 --inj-vnet=${vnet} --vcs-per-vnet=${vc_} --injectionrate=${k} --synthetic=${bench[$b]} --routing-algorithm=0

#### SPIN: #### 
${gem5_spin}/build/Garnet_standalone/gem5.opt configs/example/garnet_synth_traffic.py --topology=Mesh_XY --num-cpus=64 --num-dirs=64 --mesh-rows=8 --network=garnet2.0  --sim-cycles=$cycles --vcs-per-vnet=${vc_} --inj-vnet=0 --injectionrate=${k} --synthetic=${bench[$b]} --enable-spin-scheme=1 --dd-thresh=1024 --routing-algorithm=table --max-turn-capacity=40 --enable-variable-dd=0 --enable-rotating-priority=1

#### EscapeVC: ####  
(uses WestFirst Routing Algorithm in Escape VC, Normal VC use Adaptive Random Routing):

${gem5_esc}/build/Garnet_standalone/gem5.opt configs/example/garnet_synth_traffic.py --topology=Mesh_XY --num-cpus=64 --num-dirs=64 --mesh-rows=8 --network=garnet2.0 --router-latency=1  --sim-cycles=$cycles  --inj-vnet=${vnet} --vcs-per-vnet=${vc_} --injectionrate=${k} --synthetic=${bench[$b]} --routing-algorithm=WEST_FIRST_


### Full System: ###

#### SEEC (4x4 Mesh): #### 

${gem5_seec}/build/X86_MOESI_hammer/gem5.opt --remote-gdb-port=0  ./configs/my_configs/boot_from_checkpoint.py --script=/usr/scratch/mayank/benchs/${benchmarks[$b]} -n 16 --topology=Mesh_XY --num_dirs=16 --mesh-rows=4 --network=garnet2.0 --vcs-per-vnet=${vc_} --routing-algorithm=${rout_} --seec=1 --one-pkt-bufferless=1 --num-bufferless-pkt=1 --bufferless-router=1 --inj-single-vnet=${single_vnet} --ruby &

#### mSEEC (4x4 Mesh): #### 

${gem5_seec}/build/X86_MOESI_hammer/gem5.opt --remote-gdb-port=0  ./configs/my_configs/boot_from_checkpoint.py --script=/usr/scratch/mayank/benchs/${benchmarks[$b]} -n 16 --topology=Mesh_XY --num_dirs=16 --mesh-rows=4 --network=garnet2.0 --vcs-per-vnet=${vc_} --routing-algorithm=${rout_} --seec=1 --one-pkt-bufferless=1 --num-bufferless-pkt=1 --bufferless-router=4 --inj-single-vnet=${single_vnet} --ruby &
