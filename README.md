# Phase Space Theory Potential Factor Fit Script

### Theory
##### Morse Potential
* The bond's stretch energy is calculated by the Morse Potential Function which is the function of stretch length r.   
$V_{Morse}(r) = D_{E}(1 - exp(-\alpha(r - r_{0})))^{2}$
* To calculate Morse Potneital following parameters are required:
    * Bond Dissociation Energy       --- $D_{E}$
    * The bond stretch length        --- $r$
    * The equilibrium bond length     --- $r_{0}$
    * Curvature Parameter            --- $\alpha = 2\pi*\nu\sqrt{(\mu/2D_{E})}$
        * Reduced Mass               --- $\mu$
        * Average Stretch Frequencies --- $\nu$
* Optimization job (M062x) in Gaussian, generates sufficient data to calculate the Morse Potential.
* For getting the data to calculate Morse Potential for all bond fissions of the molecule, the optimization job (M062X) of that molecule is required.
* Frequencies can be get from the frequency table of Gaussian.
* Frequencies selected should represent bond stretching frequencies of the Bond Fission in interest. For example, if interest is in C-H bond Fission from a particular site then look for C-H stretching frequency by observing animation for the selected frequency from the table.

##### Attraction Potential in MESS
$V_{attr}(r) = -a/r^{b}$

##### Molecular Dynamics
To better understand the molecular dynamics, the momentum balance was reduced in terms of kinetic and potential energy and by the conversion of coordinates, the cartesian were converted to the polar and it can be shown by momentum conservation that the whole system (system containing two molecules in interest) dynamics happens on one plane and hence the energies can be the function of the distance between two molecules r.

As the motion is look in relative terms, the kinetic energy of the system for an elastic collision will not change. This makes understanding the Potential energy change of the system more important as the collision will depend on it. In the past, many functions were developed to calculate the potential energy by considering simple cases like Hard Spheres, Hard Spheres with attraction, Hard spheres with attraction with quantum effects, simple collision theory, modified collision theory, etc. 

In any case, the calculation of $V_eff(r) = V(r) + \epsilon b^{2}/r^{2}$ where b is the impact parameter which is the perpendicular distance from the extension of ν to the origin where origin being the molecule to which system is relative and r is the distance between two molecules, is important as it considers the centrifugal potential term ($\epsilon b^{2}/r^{2}$) and depending on its value it is determined if the collision is reactive or not. The condition for reactive collision is collision energy (Kinetic Energy) should be greater than centrifugal potential. While fitting the potential we will not consider the centrifugal potential as it is calculated by MESS and the potential we fit will be $V(r)$ or $V_eff(r)$.

Various potential functions have been used to give an estimate of the Potential energy. A few of the functions are the Lennard Jones, Buckingham, fitting via power law function, etc. These functions were cross-validated against experimental data of the Nobel gas attractions. 

##### Lennard Jones (6-12) Potential 
$V_{LJ}(r) = \epsilon((\sigma/r)^{12} - (\sigma/r)^{6})$

##### Buckingham Potential
$V_(r) = A exp(-B r) - C / r^{6}$

Buckingham Potential is an improvement on LJ 6-12 potential by modifying the repulsive term $\epsilon((\sigma/r)^{12}$ of Lennard-Jones with $A exp(-B r)$ as physically the repulsive potential changes exponentially with distance r.

The key point to remember is these potentials were developed to estimate the van-der wall attraction potential energy and since the experimental data of Nobel gas were used, the forces in play will be dispersion forces or induced dipole forces which are inversely proportional to $r^{-6}$. However, in reality, electrostatic energy will play an important role as well in defining attraction and repulsive potential. Apart from dispersion forces and induced diploe, the dipole-diploe ($r^{-3}$), quadruple interaction ($r^{-4}$ & $r{-5}$) etc. multi-pole interactions can happen. This will require the Taylor series expansion of $(-Cexp(-Dr) = -C\sum{r^{-n}/n!})$ to consider all the electrostatic interactions happening.

As MESS only considers the -C/r^n term, n can be considered as a float instead of an integer to factor in the inaccuracies caused by the removal of the terms of the Taylor series.

##### Modified Buckingham Potential
$V_(r) = A exp(-B r) - C / r^{n}$

Hence, for the fitting purpose, we use Modified Buckingham Potential, which has a physically correct expression for repulsion and a mathematically convenient expression for attraction potential. Adding a repulsion term enables the curve_fit routine to fit the Buckingham Potential to Morse Potential for the whole range of r values.

While fitting, the repulsive potential of Buckingham will give a 99% overlap with the repulsive potential of Morse while the attraction potential will not be that accurate as few terms are omitted to simplify the math.

### Instructions
* Please read the comments which starts with **#PSS** and the code dividing comments like **# -------- Comment -------- #**. Other comments are for debugging purpose and can be ignored!
* $1^{st}$ cell contains the import packages. It is not advisable to create a duplicate of this cell while Fitting multiple plots. Importing packages multiple times will only slow down the code and will create redundancy.
* If required to add new packages for any reason please add in the $1^{st}$ cell or create a new cell below $1^{st}$ cell.
* It is recommended to copy the Fit script into new cells as many times as the number of molecules' potential being fitted. However, for individual bond fissions of a molecule keep all Morse Potential data in the respective molecule cell and comment out the data not being used for the fit. 
* **Shortcomings of Morse Potential: The Morse Potential always shows less repulsion and more attraction potential than it should in real. Hence while fitting the Buckingham Potential, if the attraction potential of Buckingham is on the little left to the Morse Potential's attraction then it might be closer to the real potential and that fit should be considered for MESS calculations.**
