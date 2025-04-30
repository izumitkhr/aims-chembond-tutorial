# Tutorial for aims_chembond 0.0.1

## About

aims_chembond is a post-processing tool to perform crystal orbital overlap population (COOP) analysis using FHI-aims.

The following projections are available:

* atom-projected DOS
* atom pair COOP
* orbital pair COOP
* atom-orbital pair COOP

##  Using

In `control.in`, specify the following:

```
output coop_set
```

This write command enables writing the following file:

* `eigenvec.out` 
* `coop_set.out`

In order to to perform a COOP analysis, you need to create an input file `control_chembond.in` in the same folder, e.g. similar to the following:

```
output pdos -20 20 401 0.3
atom 1
atom 2

output coop -20 20 401 0.3
atom_pair 1 2
orbital_pair 3 18
atom_orb_pair 1 3s 2 3p
```

`output coop -20 20 401 0.3` defines the lower and the upper bound of the printed energy window, the number of points and the gaussian broadening.

Now, run the executable `aims.chembond.0.0.1.x`. 

!!! warning "Warning"
When using this code for COOP calculations in crystalline systems, care should be taken when interpreting COOP from primitive cell calculations. It is generally safer to perform calculations using a supercell. This is because when calculating COOP, the unit cell is expanded to a supercell, and the influence of equivalent atoms in adjacent unit cells is included in the results. 

## Citing

If you find this code helpful, please consider citing:

```
Izumi Takahara, Kiyou Shibata, and Teruyasu Mizoguchi
"Crystal orbital overlap population based on all-electron ab initio simulation with numeric atom-centered orbitals and its application to chemical-bonding analysis in Li-intercalated layered materials"
Modelling Simul. Mater. Sci. Eng. 32 055028 (2024).
https://doi.org/10.1088/1361-651X/ad4c82
```