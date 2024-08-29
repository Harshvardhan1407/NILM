# NILM

Brief NILM approach :

FHMM Model :


The FHMM is an advanced approach to model the combined behavior of multiple hidden Markov models (HMMs), each representing the state of an individual appliance.
Here's an explanation of how the model works:
1. FHMM Overview
FHMM: A Factorial Hidden Markov Model is essentially a combination of several independent HMMs. In this case, each appliance in the household is modeled as an HMM. The FHMM models the overall system (e.g., the household's total power consumption) by combining these individual HMMs.
HMM: A Hidden Markov Model is a statistical model where the system being modeled is assumed to follow a Markov process with unobservable (hidden) states.
2. Code Structure
FHMMExact Class: This is the main class that encapsulates the FHMM model. It has methods for training the model (partial_fit) and for disaggregating the power consumption (disaggregate_chunk).

partial_fit Method:

Training Phase:
The method first concatenates the training data for the main power signal and the individual appliances.
For each appliance, it trains a separate Gaussian HMM on the power consumption data. The number of states in each HMM is either provided or determined based on the data.
After training the individual HMMs, the method sorts the learned parameters (start probabilities, means, covariances, transition matrices) to ensure consistent ordering.
Finally, it combines the individual HMMs into a single FHMM using the create_combined_hmm function.
disaggregate_chunk Method:

Disaggregation Phase:
This method is used to predict the power consumption of individual appliances from the aggregate power signal (the total power consumed by the household).
It uses the combined FHMM to predict the sequence of states for the entire household (which corresponds to a combination of states of individual appliances).
These states are then decoded to estimate the power consumption of each appliance at each time step.
3. Key Functions and Methods
sort_learnt_parameters: This function sorts the parameters learned by the individual HMMs (e.g., means, start probabilities) to ensure consistency when combining them into the FHMM.
compute_A_fhmm, compute_means_fhmm, compute_pi_fhmm: These functions calculate the combined transition matrix, means, and start probabilities for the FHMM by taking the Kronecker product of the corresponding parameters from the individual HMMs.
create_combined_hmm: This function creates a new HMM that represents the combined behavior of all the individual HMMs (i.e., the FHMM).
decode_hmm: This function decodes the combined states predicted by the FHMM into individual appliance states and power consumption estimates.
4. Training Process
During training, the model learns how each appliance behaves independently by fitting a separate HMM to each appliance's power data.
These individual HMMs are then combined into a single FHMM that can model the total power consumption of the household as a combination of the individual appliances' consumption patterns.
5. Disaggregation Process
Given a sequence of total power consumption, the FHMM predicts the most likely combination of appliance states at each time step.
The model then estimates how much power each appliance consumed based on these predicted states.
6. Applications and Utility
This approach is useful for energy monitoring systems where the goal is to understand the contribution of each appliance to the overall energy consumption without having to measure each one individually.
The model can help in energy-saving efforts by identifying which appliances consume the most power and at what times.



CO Model:

 The CO model works by finding the best combination of appliance states that can explain the observed aggregate power consumption at each time step.

How the CO Model Works
1. Basic Assumptions
The total power consumption at any given time is the sum of the power consumption of individual appliances.
Each appliance has a finite number of discrete states (e.g., off, low, medium, high) and corresponding power consumption levels.
2. Input Data
Aggregate Power Consumption: The total power usage of the household, typically measured over time.
Appliance Power Signatures: The power consumption levels associated with each possible state of each appliance.
3. Optimization Process
State Enumeration: For each appliance, enumerate all possible states and their corresponding power consumption.

Combinatorial Search: For each time step, the model searches through all possible combinations of appliance states to find the combination that best matches the observed total power consumption.

Objective Function: The CO model typically minimizes an error function, such as the difference between the observed total power and the sum of the predicted appliance power consumption. This can be formalized as:

Best Fit Selection: The combination of states that minimizes the error function for each time step is selected as the estimated state of all appliances.

4. Disaggregation Output
After the optimization process, the model outputs the estimated power consumption of each appliance at each time step.




