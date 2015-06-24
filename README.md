mc_shms_single
==============
 
Hall C single arm Monte Carlo 



To compile type "make"

Main code is mc_shms_single.f

If include files are changed it is best to do "make Clean" and then "make"
since the Makefile is not smart enough to look for dependencyy

Running code
------------

mc_shms_single 
(ask for input file name ( assumed in infiles subdirectory)

* Input file : infile_name.inp
* Output file is at outfiles/infile_name.out 
* The hbook file is at worksim/infile_name.rzdat 

Code flow
--------- 
* Read in the input file
* Starts event loop
* The seed for the random number generator is picked by the time. each run of the code will be different set of random numbers.
* The lab or beam coordinate system has +z along the target towards the beam dump, +y is vertical pointing down and +x is pointing beam left.
* Randomly selects beam-target interaction point.
* The coordinate system of the SHMS is +z_s along the central ray, +x_s is vertical pointing down and +y_s is horizontal point to large angle. 
* Randomly selects particle delta=(p-pcent)/p, dy_s/dz_s amd dx_s/dz_s.
* Drifts to z_s=0 plane
* Calculated multiple scattering the target using the input radiation length
** If target length greater than 3cm assume LH2 cryotarget for multiple scattering calculation.
**  If target length less than 3cm assume simple solid target for multiple scattering calculation.
* calulated mutliple scattering in scattering chamber window, air and spectrometer entrance window.* Call the mc_shms subroutine to transport the particle through SHMS apertures and to the focal plane.
* At the focal plane drift the particle through  1st Cerenkov or vacuum pipe ( depended in flag in input file),the drift chamber, scintillators, 2nd cerenkov and calorimeter to check if it hits them . 
* As it passes through the drift chamber randomly change the position according to the presumed DC resolution and fit the positions and calculate the focal plane positions and angles.
* From focal plane calculate the reconstructed target quantities ytar,yptar,xptar and delta using the optics matrix. Note that for now use the known x target position in the optics matrix calculations. To mimic what would be in the analysis, need to calculate xtar from the reconstructed quantities using the raster vertical position and then recalculate the reconstructed quantities with updated xtar.
* If particle makes it to the focal plane and hits all detectors the event variables are put in ntuple.



Sub Directories
---------------
* shms  : SHMS subroutines
* infiles : input files
* outfiles : the output files 
* worksim : rzdat files

Info on infiles
---------------
* Mostly self explanatory
* dp/p down and up should be at least -15.0 and 25.0 
* theta down and up is dy/dz , horizontal angle relative to central ray keep at least -55,55mr
* phi down and up is the dx/dz, vertical angle relative to central ray ,keep at least -50, 50mr
* Remember to keep the Dp/p,theta,phi and ztgt reconstruction cut larger than thrown. 
* The target length just 
* need to set flag if aerogel will be in the detector stack
* Up to the experiment to decide if the 1st Cerenkov detector will be needed for the experiemnt
* If 1st Cerenkov not used then can replace with vacuum pipe. Option of helium bag is availble. this was for study.  

Ntuple variables in hut ntuple ntuple id = 1411 
---------------------
* hsxfp  Focal plane vertical position 
* hsyfp  Focal plane horizontal position
* hsxpfp Focal plane vertical angle=dx/dz
* hsypfp Focal plane horizontal angle=dy/dz
* hsztari  Initial random position along the target
* hsytari  Initial y (horizontal) position at the z=0 plane perpendicular to central ray of SHMS
* hsdeltai Initial random  delta = (p-pcent)/pcent
* hsyptari Initial random  horizontal angle=dy/dz
* hsxptari Initial random  vertical angle=dx/dz
* hsztar  Reconstructed position along the target
* hsytar   Reconstructed 
* hsdelta   Reconstructed 
* hsyptar  Reconstructed 
* hsxptar Reconstructed 
* hsxtari  Initial x (vertical) position at the z=0 plane perpendicular to central ray of SHMS
* yrast   Initial random vertical raster position
