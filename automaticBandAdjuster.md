## Project description
Find the optimal constant $const.$ in the expression $$E'=E-const.$$ so that the RMS of the $^{135}$Pr energy sets is minimal.
The constant is used for adjusting the experimental bands (both wobbling and yrast) by subtracting or incrementing the energy states in a certain region that the program finds *unoptimized*.
 
 * Construct a stack of RMS function for an arbitrary set parameters and adjustment constant -> `std::vector<T> RMS_stack`. 
 * Obtain a set of parameters $X=\{A_1,A_2,A_3,\theta\}$ and a value for $const.$ which corresponds to the minimum in the RMS stack.
 * Redo the graphical representation of the energies using the original experimental energies.

### Worflow
-----
-> start with the pure experimental energies (**normalized to the first state**)
->get an arbitrary set of parameters $X_k=\{A_1,A_2,A_3,\theta\}$ and compute $E_\text{RMS}=f(E^{exp}_{X_k},E^{th}_{X_k})$ 
-> check where the highest *unoptimized* region exists: 1st half, 2nd half or middle:
->**create plot for *exp. vs. theory* comparison** (step G1)
-> calculate the normal differences $\Delta_E=E_{th}-E_{exp}$ 
* if $\Delta \geq 0$ then subtract a constant value from $E_{exp}$
* if $\Delta < 0$ then add a constant value from $E_{exp}$

-> apply the adjustment for the experimental bands  
-> recalculate RMS and obtain a new `RMS_stack`
-> obtain the new set of parameters $X_k^{new}$ for which the RMS stack is minimal
-> redo the **G1** step: the graphical representation of the energies with the new parameters, but using the initial experimental energies.

### Current status of the project 
-----
#### Band adjustment and `sidePicker`
* Algorithm is able to successfully select the part where there is the biggest deviation from the experimental energies.
* The selection is done by computing the average deviation $\Delta_E=E_{th}-E_{exp}$.
	* Depending on which side the deviation is bigger, that side will be *adjusted*
	* The experimental data set will be adjusted only
	* Operations are done per band (each band has its own deviation and adjustment parameter $q$)
* The calculation is based on an initial set of containers `exp` and `th`, where `th` is determined by obtaining the minimal set $X$ for which the RMS is minimal.
	* The obtained parameters are used to generate the `th` set by applying `energyExpression(A1,A2,A3,theta)`
#### RMS recalculation
* after the first theoretical data set is constructed -> we apply the adjustment once, and obtain a new experimental data set -> `expData_subtracted`
* with the new set of exp data, we re-apply the calculation of `searchMinimum` in order to get another set of parameters $X'$
#### `searchMinimum` function -> the approach
This function finds the best set of parameters $X$ such that the RMS between `E_EXP` and `E_TH` is minimal
> searchMinimum must depend on the input data and it must update a parameter conta
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExODc5NzE2OTIsLTE1ODI3NzMwMTksLT
Y1OTkxNjkwOSwtMTkxMTcyMzU5NywxMzkyODkxNjkzXX0=
-->