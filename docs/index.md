# Tutorial for aims-chembond (v0.0.1)

## About

aims-chembond is a post-processing tool to perform crystal orbital overlap population (COOP) analysis using FHI-aims.

The following projections are available:

* atom-projected DOS (Partial DOS)
* atom pair COOP
* orbital pair COOP
* atom-orbital pair COOP

## Compiling FHI-aims
Add `set(USE_COOP ON CACHE STRING "")` to initial_cache.cmake, and compile following the same procedure as in the original FHI-aims.
This should create the `aims.chembond.0.0.1.x` executable file of aims-chembond.

##  Using aims-chembond
Using this functionality requires a two-step process. First, perform electronic structure calculations with FHI-aims, then use the results as input for aims-chembond to perform the chemical bonding analysis.

!!! warning "Warning"
    When using aims-chembond for COOP calculations in crystalline systems, care should be taken when interpreting COOP from primitive cell calculations. It is generally safer to perform calculations using a supercell. This is because when calculating COOP, the unit cell is expanded to a supercell, and the influence of equivalent atoms in adjacent unit cells is included in the results. 

### Running FHI-aims
In the `control.in` file, please add the following output specification:

```
output coop_set
```
This write command activates the generation of two output files:

- eigenvec.out - Contains eigenvector information
- coop_set.out - Contains the remaining information necessary for COOP analysis

These files will serve as inputs for the subsequent chemical bonding analysis step with aims-chembond.

### Running aims-chembond
After performing the FHI-aims calculation, you will perform chemical bonding analysis using the `aims.chembond.0.0.1.x`. To use aims-chembond, you need three input files: eigenvec.out, coop_set.out, and control_chembond.in.

aims-chembond provides functionality to calculate both PDOS (Partial Density of States) and COOP (Crystal Orbital Overlap Population). To calculate PDOS, include the following content in your control_chembond.in file:

```
output pdos -20 20 401 0.3
atom 1
atom 2
```
Here, `output pdos -20 20 401 0.3` defines the energy window parameters for the PDOS spectra: the lower bound, the upper bound, the number of points, and the Gaussian broadening factor. The `atom` tag specifies the atom index for which to calculate the PDOS.

Similarly, to calculate COOP, include or append the following content to the control_chembond.in file:
```
output coop -20 20 401 0.3
atom_pair 1 2
orbital_pair 3 18
atom_orb_pair 1 3s 2 3p
```
In the example above, `atom_pair 1 2` calculates the COOP between atom 1 and atom 2. `orbital_pair 3 18` calculates the COOP between the 3rd orbital and the 18th orbital. `atom_orb_pair 1 3s 2 3p` calculates the COOP between the 3s orbital component of atom 1 and the 3p orbital component of atom 2.

aims-chembond is parallelized across k-points. You can execute the calculation using MPI with a command like: `mpiexec -n 6 aims.chembond.0.0.1.x`.

!!! info "Information"
    PDOS and COOP analyses are highly dependent on the choice of basis functions. While the "light" basis set offers a good starting point, you should carefully validate your results to ensure their accuracy and reliable interpretation.


## Citing

If you find aims-chembond helpful, please consider citing:

```
Izumi Takahara, Kiyou Shibata, and Teruyasu Mizoguchi
"Crystal orbital overlap population based on all-electron ab initio simulation with numeric atom-centered orbitals and its application to chemical-bonding analysis in Li-intercalated layered materials"
Modelling Simul. Mater. Sci. Eng. 32 055028 (2024).
https://doi.org/10.1088/1361-651X/ad4c82
```