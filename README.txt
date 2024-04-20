Material parameter influence on the expression of Solitary-Wave-Induced Surface Dilation
Eric Frizzell (efrizz@umd.edu) and Christine Hartzell
University of Maryland, Aerospace Engineering, 3178 Glenn L. Martin Hall
College Park, MD, 20740, United States
Manuscript submitted for publication April, 2024
https://github.com/efrizz-umd/SID_sensitivity

The text files in these folders are LIGGGHTS input scripts used in Frizzell & Hartzell, 2024 (submitted).

There are two folders, 'channel_prep'  which contains the scripts used for filling channels of varying particle size ('fill scripts') and 'piston_impacts' which contains scripts for initiating shocks within the prepared channels ('shock sripts'). The result of running a fill script is to produce a 'restart file', a binary file (usually named something like restart_TESTTYPE.static) of particle states for a channel that has been filled with particles of various material parameters and has been settled (ie, particle forces have been allowed to slowly dissipate under gravity until they are 'low enough' - see our first work at [1]). The restart_file is then  used as in input to the shock script, allowing multiple velocities of impact to be studied without preforming channel preparation for each test. The restart files we produced in this work are located at the link below.

### Restart files ###
Zenodo link TBD

### Channel preparation ###
The in.pour files fill a channel of the desired channel geometry. In this work we perform a scaling of the base case from [1] which is a 2 meter long channel filled 20 cm deep with particles (2 cm in width). The particles for that channel were R_0 = 1.25 mm, which means that channel dimensions in terms of particls is 160, 1600, 16 (respectively). When changing particle size, we scale the original channel dimensions and particle insertion velocities by R_new/R_0. Also included is the 'meshes' folder, which contains a planar STL file of dimensions 2 m x 2 cm that is needed for the LIGGGHTS insertion procedure.

These cases were run on our stand-alone lab server (44 core). After compiling LIGGGHTS, the files are run as:
mpirun -np NUM_PROC_HERE ./lmp_auto -echo both -i in.pour

### Piston Impacts ###
Although we have run tests for each parameter at several different values, we typically just include one 'in.shock' file per test type (ex: in.shock_friction). The user would need to make sure that the input file parameter of interest (in our example, the LIGGGHTS variable 'coefficientFriction' would be updated) is set to the value desired. There are two methods of using restart files with the input scripts. First, is to use a restart file that was prepared with the exact same material parameters as in the shock script. The second method involves changing a parameter from what is already represented in the restart file (ex: restart file was prepared with coefficientFriction = 0.6 but you now want to shock a channel which has particles of coefficientFriction = 1.2). In this case you will want to make sure that the few lines of code before the shocking occurs (where the piston velocity is set) are uncommented to allow the particles to 'relax'. In some cases, changing particle property requires quite a bit of settling time (like increasing elastic modulus by orders of magnitude), and in these cases we provide a separate 'in.settle' script in which you load the initial restart file, change the material properties to those desired in the settle script, and then the result of running the script is a new restart file with the desired material parameters (this process may be subject to some manual tuning and you may need to increase settling time to achieve 'low enough' initial condition forces). We do include separate shock scripts for each R (radius/size) and E (elastic modulus) case as the needed data output rates for each case were heuristically chosen.

These cases were run on the UMD HPC cluster Zaratan (128 cores/node) and the included 'job_shock.sub' is an example of running the shock scripts.

### References ###
1 - Frizzell, E.S., Hartzell, C.M. Simulation of lateral impulse induced inertial dilation at the surface of a vacuum-exposed granular assembly. Granular Matter 25, 75 (2023). https://doi.org/10.1007/s10035-023-01363-6

LIGGGHTS: https://www.cfdem.com/liggghtsr-open-source-discrete-element-method-particle-simulation-code
