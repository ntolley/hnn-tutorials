# Event Related Potentials (ERPs)

## Getting Started

In order to understand the workflow and initial parameter sets provided with this tutorial, we must first briefly describe prior studies that led to the creation of the provided data and evoked response parameter set that you will work with. This tutorial is based on results from our 2007 study where we recorded and simulated tactile evoked responses source localized to the primary somatosensory cortex (SI) [1].

In our 2007 study, we investigated the early evoked activity (0-175 ms) elicited by a brief tap to the D3 digit and source localized to an an equivalent current dipole in the contralateral hand area of the primary somatosensory cortex (SI) [1]. The strength of the tap was set at either suprathreshold (100% detection probability) or perceptual  threshold (50% detection) levels (see Figure 1, left panel below). Note, to be precise, this data represents source localized event related field (ERF) activity because it was collected using MEG. We use the terminology ERP for simplicity, since the primary current dipoles generating evoked fields and potentials are the same.

We found that we could reproduce evoked responses that accurately reflected the recorded waveform in our neocortical model from a layer specific sequence of exogenous excitatory synaptic drive to the local SI circuit, see Figure 1right panel below. This drive consisted of “feedforward” / proximal input at ~25 ms post-stimulus, followed by “feedback” / distal input at ~60 ms, followed by a subsequent “feedforward” / proximal input at ~125 ms (Gaussian distribution of input times on each simulated trial). This sequence of drive generated spiking activity and intracellular dendritic current flow in the pyramidal neuron dendrites to reproduce the current dipole signal. This sequence of drive can be interpreted as initial “feedforward” input from the lemniscal thalamus, followed by “feedback” input from higher order cortex or non-lemniscal thalamus, followed by a re-emergent leminsical thalamic drive. Intracranial recordings in non-human primates motivated and supported this assumption [2].

In our model, the exogenous driving inputs were simulated as predefined trains of action potentials (pre-synaptic spikes) that activated excitatory synapses in the local cortical circuit in proximal and distal projection patterns (i.e. feedforward, and feedback, respectively, as shown schematically in Figure 1 right, and in the HNN GUI Model Schematics). The number, timing and strength (post-synaptic conductance) of the driving spikes were manually adjusted in the model until a close representation of the data was found (all other model parameters were tuned and fixed based on the morphology, physiology and connectivity within layered neocortical circuits [1]. Note, a scaling factor was applied to net dipole output to match to the magnitude of the recorded ERP data and used to predict the number of neurons contributing to the recorded ERP (purple circle, Figure 1, right panel). The dipole units were in nAm, with a one-to-one comparison between data and model output due to the biophysical detail in our model.

<div class="stylefig">

### Figure 1

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/erp/images/image8.png"><img class="imgcenter100" src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/master/erp/images/image8.png" alt="image8" style="max-width:650px;"/></a>

<p style="text-align:justify;"> Adapted from Jones et al. 2007 [1]. Comparison of SI evoked response in experiment and neural model simulation. Left: MEG data showing tactile evoked response (ERP) source localized to the hand area of SI. Red: suprathreshold stimulation; Blue: Threshold stimulation (avg. n=100 trials). Right: Neural model simulation depicting proximal/distal inputs needed to replicate the ERP waveform (avg. n=25 trials) </p>
</div>

In summary, to simulate the SI evoked response, a sequence of exogenous excitatory synaptic drive was simulated (by creating presynaptic spikes that activate layer specific synapses in the neocortical network) consisting of proximal drive at ~25 ms, followed by distal drive at ~60 ms, followed by a second proximal drive at ~122 ms. Given this background information, we can now walk you through the steps of simulating a similar ERP, using a subset of the data shown in Figure 1.

## Tutorial Table of Contents

1. [Load/view data](#toc_1)

2. [Load/view parameters to define network structure &  to “activate” the network](#toc_2)

3. [Running the simulation and visualizing net current dipole](#toc_3)

4. [A closer look inside the simulations: contribution of layers and cell types](#toc_4)

5. [Comparing model output and recorded data](#toc_5)

6. [Adjusting parameters](#toc_6)

7. [Have fun exploring your own data!](#toc_7)

8. [Additional documents](#toc_8)

    8.1. [Parameter files](#toc_8.1)

    8.2. [Video walkthrough (2018)](#toc_8.2)

<a id="toc_1"></a>

## 1. Load/view data

An example ERP dataset is provided in the <a href="https://github.com/jonescompneurolab/hnn-data">hnn-data GitHub repository</a>. We use the data as an orienting example for where to begin to simulate an ERP. 

This dataset represents early evoked activity (0-175 ms) from an equivalent current dipole source localized to the hand area of the primary somatosensory cortex (SI), elicited by a brief perceptual threshold level tap to the contralateral D3 digit (read Getting Started above for details). The example dataset provided was collected at 600Hz and contains only averaged data from 100 trials in which the tap was detected. (Note, when loading your own data, if it was not collected at 600Hz, you must first downsample to 600Hz to view it in the HNN GUI).

To load and view this data, navigate to the main GUI window and on the bottom left corner click:
```
Load data
```

If you have cloned the hnn-data repository, navigate to hnn-data folder on your desktop and select `MEG_detection_data/yes_trial_S1_ERP_all_avg.txt`. HNN will then load the data and display the waveform in the dipole window as shown below.

Alternatively, if you have not cloned the hnn-data repository, you can download the file directly by clicking <a href="https://github.com/jonescompneurolab/hnn/blob/master/data/MEG_detection_data/yes_trial_S1_ERP_all_avg.txt">here</a>.

<div class="stylefig">

### Figure 2

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/core-gui/coregui_erp/images/erp_fig_02.png"><img class="imgcenter100" src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/core-gui/coregui_erp/images/erp_fig_02.png" alt="image7" style="max-width:500px;"/></a>

</div>

Note, the software can be used without loading data. If you wish to play with simulations without data, proceed to Step 2 first.

<a id="toc_2"></a>

## 2. Load/view parameters to define network structure &  to “activate” the network

An initial parameter set that will simulate the evoked drives that generate an evoked response in close agreement with the SI data described in Step 1 is distributed in the hnn-data repository.  Click on the `External drives` tab at the top of the GUI and then click the `Load external drives` button. Navigate to the `hnn-data/network-configurations` folder on your computer and select  'ERPYesTrials.param', or click <a href="">here</a> to download the parameter file directly and load it into the GUI. 

<div class="stylefig">

### Figure 3

<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/core-gui/coregui_erp/images/erp_fig_03_01.png"><img class="imgcenter100" src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/core-gui/coregui_erp/images/erp_fig_03_01.png" alt="image7" style="max-width:500px;"/></a>

</div>

The template cortical column networks structure for this simulation is described in the <a href="https://hnn.brown.edu/under-the-hood/">"HNN Template Model" page</a> on the hnn.brown.edu website. Several of the network parameter can be adjusted via the HNN GUI (e.g., local excitatory and inhibitory connection strengths) under the `Network connectivity` tab, but we will leave them fixed for this tutorial and only adjust the inputs that “activate” the network.

The values of the parameters that you loaded to “activate” the network in a manner that will generate an evoked response can now be viewed under the `External drives` tab.   As described in the “Getting Started” section, the evoked response can be simulated with a sequence of exogenous driving inputs consisting of a proximal input at ~26 ms (evprox1), followed by a distal input at ~64 ms (evdist1), followed by a subsequent proximal input at ~137 ms (evprox). 

To see the detailed parameter values defining each of these drives click on the dropdown button next to the name of each drive.  Note: additional evoked proximal or distal inputs can be added to your simulation for your hypothesis testing goals by using the `Add drive` button and specifying the drive as "Evoked" and the location as either "proximal" or "distal". Other types of drives can also be defined including poisson, rhythmic, and tonic, as detailed in other tutorials.

Each evoked input consists of a Gaussian distributed train of presynaptic action potentials that will target all of the cells in the post-synaptic network, with several adjustable parameters, including  the mean arrival and standard deviation of the time each spike activates the network (in milliseconds), the number of the driving spikes on each trial of the simulation, and a random number seed that enables reproducibility of simulation results across trials.  You can also adjust the postsynaptic conductance of the drive onto the  postsynaptic cell. For example, under the "AMPA weights" section in the "evprox1" dropdown menu, the L5_pyramidal field represents the the post synaptic AMPA conductance of the proximal input onto the layer 5 pyramidal neuron at each location targeted by the proximal drive. Note that the Synchronous Inputs checkbox allows specification of whether each cell/synapse receives inputs at the same time or whether each cell/synapse receive inputs independently. In either case, the synaptic input times are drawn from the same distribution. Schematic representations of the postsynaptic location of each input is shown below. For further details on the connectivity structure of the network, see the <a href="https://hnn.brown.edu/under-the-hood/">"HNN Template Model"</a> page.

<div class="stylefig">
<table>
<h3>Figure 3</h3>
<tr>
<td>
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/core-gui/coregui_erp/images/erp_fig_03_02.png"><img class="imgcenter100" src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/core-gui/coregui_erp/images/erp_fig_03_02.png" alt="image17"/>
</a>
</td>
<td>
<a href="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/core-gui/coregui_erp/images/erp_fig_03_03.png"><img class="imgcenter100" src="https://raw.githubusercontent.com/jonescompneurolab/hnn-tutorials/core-gui/coregui_erp/images/erp_fig_03_03.png" alt="image25"/>
</a>
</td>
</tr>
</table>
</div>

<a id="toc_3"></a>

## 3. Running the simulation and visualizing net current dipole

Now that we have an initial parameter set, we can run a simulation for a set number or trials. Let's start by defining 3 trials, by clicking on the simulation tab and defining Trials=3.  On each simulated trial, the timings of the evoked inputs (i.e., spikes) are chosen from a Gaussian distribution with mean and stdev (standard deviation) as defined in the “Evoked drives” tap. Histograms of each of the evoked inputs will be displayed at the top of the Figure tab after the simulations run. 

Before running the simulation, we’ll first change the simulation name (i.e., the name under which the simulated data will be saved) to a new descriptive-name for the simulation here. Under the 'Simulation' tab change 'Name' from default to ERPyes-1trial. Note the default simulation is in fact 1 run of the ERP yes simulation.   There are several other adjustable simulate parameters, in the 'Simulation' tab. These parameters control the duration (stop), integration time step (dt), number of trials (Trials), and the choice of the simulation backend of either MPI (parallel across neurons) or Joblib (parallel across trials), assuming both backends are installed. 

Hit 'Run" button to run the simulation. A simulation log is shown under the Run button that will tell you the status of your simulaion. 

Once complete, a new Figure 2 window will appear showing the output of the simulation as in the figure below. The thin blue traces are net current dipole signals from individual trials while the thick blue trace is the average ERP, with histograms of the proximal and distal driving spikes shown above. 

To view the simulation on top of the data and examine the goodness of fit, click on the 'Visualization' tab. Under Figure 2 you will see that Figure 2 has two subplots defined by ax0 and ax1. ax0 describes the adjustable features of the histogram subplot, while ax1 describes the adjustable features of the net current dipole subplot. Note you can change what is shown in either of these subplots by selecting 'clear axis', picking the 'Type' of data from the pulldown menu, and clicking 'Add plot'.  For now, we're going to continue to visualize the net current dipole plot. 

To overlay the data shown in Figure 1 in Figure 2, go to Figure 2 and select ax1. in the 'Data to Compare:' pull down menu choose yes_trial_S1_ERP_all_avg, then click add plot. The data will now be overlaid in Figure 2 with the root mean square error (RMSE) displayed. 

 
You can remove the data or simulation output from the figure by clicking "clear axis' 