# modified_Hass_code

# Introduction
This is a modification of code adapted from the 2016 paper titled "A Detailed Data-Driven Network Model of Prefrontal Cortex Reproduces Key Features of In Vivo Activity" by Joachim Hass, Loreen Hertäg, and Daniel Durstewitz.
The purpose of the original code is to create a biologically realistic spiking network with interconnected excitatory and inhibitory neurons with AMPA, NMDA, and GABA receptors in two layers (II/III and V) of prefrontal cortex.
The purpose of the modified code is to selectively ablate NMDA receptors in different cell types in order to examine the effects upon population spiking activity, using data collected from patch clamp electrophysiology 
and confocal imaging experiments. The titles of all modified code files end with RMD (initials of Rachel Dick). 

# RunIDNetRMD.m
The main file is RunIDNetRMD.m, which runs the simulation and creates a raster plot of the resulting spike trains. 
This is where we set:
– Simulation time
– Number of neurons, input neurons, and columns/stripes
– Simulation conditions set in the CRISPRparams file (e.g. wild type, NMDA receptors ablated from pyramidal cells, etc.)
– Synaptic weight scales
– Connectivity scales
– Background input and constant DC current

It opens various other code files stored in the IDNet_RMD folder.
Outputs:
- STMtx: Spike times in ms, (cell array, one cell per neuron)
- V: Membrane potential in mV, (cell array, one cell per neuron)
- T: Simulation time in ms (vector)

Magic numbers are found in the second section, where we set I_ex and I_inh (constant DC current).

# ConfigIDNetRMD.m
This file sets all simulation parameters for IDNetSim.m, except for the optional connectivity matrix

# selectSimulation.m
This sets the name of a particular simulation (e.g. wild type, NMDA receptors ablated from pyramidal cells, etc.) to "true." The names match field names in setCRISPRparams and correspond to specific simulation runs.
This code also allows us to replicate experiments with different random initializations

# IDNet.c
This is a MEX function written in C to simulate arbitrary biological neural networks, which is used with the MATLAB wrapper IDNetSim.m
NumNeuPar is set to 13 instead of 12 in the modified code to reflect the addition of another row to NumNeuPar.
