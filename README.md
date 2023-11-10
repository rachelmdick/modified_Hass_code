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
This file sets all simulation parameters for IDNetSim.m, except for the optional connectivity matrix.
All parameters are passed to SimPar.

Inputs:
– N1:           Number of simulated neurons
– M1:           Number of input neurons (prescribed spike trains)
– Nstripes:     Number of columns/stripes
– RunTime:      Simulation time in ms
- I:            Background input (1x16 vector, one entry for each neuron type)
- s:            Scaling factors for synaptic weights (1x4 vector)
- p:            Scaling factors for synaptic connectvity (1x4 vector)
- ss_str:       Structure generated by selectSimulation to differentiate several simulations 
- RState:       State for Random number generation, optional (pass for reproduction of random numbers)

Ouptput:
- SimPar:       Simulation parameters (structure)

setCRISPRparams is called in this file to set the number of neurons in populations 1, 8, 15, and 16 (magic numbers) as well as the strength of synaptic connections.
In the section titled "Set network parameters," the identities of the 16 different neuron types and their magic numbers are defined. Copied below:

% (1) L23-E  (2) L23-E-Grin1 (3) L23-I-L  (4) L23-I-L-d  (5) L23-I-CL   
% (6) L23-I-CL-AC  (7) L23-I-CS   (8) L23-I-F 
% (9) L5-E  (10) L5-E-Grin1 (11) L5-I-L  (12) L5-I-L-d  (13) L5-I-CL   
% (14) L5-I-CL-AC  (15) L5-I-CS   (16) L5-I-F 

NTypes, which is the number of neurons of each type, is set here. Variables (L23_E_control, L23_E_Grin1) are used for the pyramidal neuron populations in the modified code.

Neuron parameters set in this file include:
– Mean independent neuron parameters
– Neuron parameter correlation matrices (magic numbers)
– Box-Cox transformation exponents for neuron parameters
– SDs of independent neuron parameters and minima and maxima of all neuron parameters

Synapse parameters are also set in this file, using the following order: (1) AMPA (2) GABA (3) NMDA
STypPar sets the parameters for the synapse type, including controlling the magneusium block for NMDA receptors.
Short-term synaptic plasticity parameters (STSP) and connection probabilities (for random networks) are set in this section. ConParSTSP and pCon both contain magic numbers.
Standard deviations of synaptic weights (first element in the third dimension of S_sig) and standard deviations of synaptic delays (second element in the third dimension of S_sig) also contain magic numbers.
Noise parameters for each synapse type and stripe parameters are set.



# selectSimulation.m
This sets the name of a particular simulation (e.g. wild type, NMDA receptors ablated from pyramidal cells, etc.) to "true." The names match field names in setCRISPRparams and correspond to specific simulation runs.
This code also allows us to replicate experiments with different random initializations

# IDNet.c
This is a MEX function written in C to simulate arbitrary biological neural networks, which is used with the MATLAB wrapper IDNetSim.m
NumNeuPar is set to 13 instead of 12 in the modified code to reflect the addition of another row to NumNeuPar.
