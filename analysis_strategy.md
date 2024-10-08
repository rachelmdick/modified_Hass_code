High-level parameters, including a chosen set of synapse parameters from SetCRISPRparams.m, are set in RunIDNetRMD.m   
These are passed to ConfigIDNetRMD.m, where detailed neuron parameters are set and passed to IDNetSimRMD.m   
The actual simulation is run at the end of IDNetSimRMD.m   

__Part One: Determining experimental parameters__

Set in RunIDNetRMD.m
* Simulation time
* Number of neurons
* Number of columns/stripes
* Synapse parameter regime (true_field) from SetCRISPRparams.m
  * e.g. CaMKII_Grin1_50 refers to abolition of the NMDA current in 50% of postsynaptic excitatory neurons

Set in SetCRISPRparams.m
* Details of synapse parameter regime – weights for AMPA, NMDA, and GABA for each synapse type
* dtax (meaning unknown)
* p_fail (failure probability)
* srec (scaling factors for recurrent and input connection strengths)

Set in ConfigIDNetRMD.m
* Types of neurons and their respective numbers within the overall populations
* This is where we call setCRISPRparams
* SimPar (structure) is the output 

__Part Two: Running simulations for a set of parameters__

TO DO:
* Make sure output files are being saved to the right folder based on simulation type from SetCRISPRparams
* Check for existance of simulation results before running a new one with the same name so that we don't overwrite files

This takes place in IDNetSimRMD.m

Inputs: 
* SimPar, a set of parameters (neurons, synapses, control parameters)
* Optional: ConMtx, a network structure (random connectivity specified in SimPar is used if this is empty)

Outputs: 
* STMtx, a cell array of spike times t_sp; each cell contains t_sp of one neuron
* T, the vector of simulation time steps for visualizing voltage traces
* V, a cell array of voltage traces for each neuron in the viewlist
* Ninp, the number of simulated neurons

__Part Three: Analyzing outputs of a single simulation and producing single-run figures__

TO DO:
* Make sure graphs are being saved to the right folder 
* useful functions: mkdir, exist(filename, 'type') – type can be dir, file, var, mat, etc.
* look at pairwise correlations between excitatory neurons – 0-lag synchrony

FUTURE ANALYSIS:
* covariance matrix of spiking activity for entire population
* plot Eigenvalue spectra

The spike raster plot is produced in RunIDNetRMD.m using STMtx

Summary statistics (ISI and CV) are produced in ISImath.m
* the output file (filename) is loaded along with all of the associated variables
* optional: sort out excitatory neurons with at least ten spikes
* ISI and CV are calculated and plotted
* can also plot voltage for a single cell
  
__Part Four: Combined analysis of multiple simulations and generating summary figures__

Use a similar format as ephys data analysis 
* A metadata file sets the file path and different fields of a struct based on the file characteristics – not necessary because SimPar is a struct with all the metadata that we need
* One function (matlabsweeps in open_file.m) exists to import data from the metadata file into an array – not necessary to convert file types  
* run_ephys_analysis takes all the metadata from the array and puts relevant measurements into a .mat file
* The .mat file is loaded in the graph_sweeps function to produce a figure
* Then run_ephys_analysis loops through each entry

* d = dir('name') will return a list of everything in the directory with that name
* d = dir('x/x3_*.mat') pulls all the files that have x3 as part of their name
