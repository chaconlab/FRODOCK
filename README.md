# FRODOCK

FRODOCK (Fast Rotational DOCKing) generates very efficiently many potential predictions of how  two proteins could interact from their 3D coordinates. 

## USAGE
The docking process will be carried out in four consecutive steps:

1. Check PDB input files
2. Potential maps generation
3. Perform the exhaustive docking search
4. Clustering and visualization of predictions.

### STEP 1. Check PDB input files

The input coordinates of both ligand and receptor proteins should conform to PDB format. Be aware of missing atoms, alternative conformations, bad place atoms and a long etc. that can eventually jeopardize your results. Please, use your favorite PDB checker to anticipate and fix any PDB error!

### STEP 2. Potential maps generation

All necessary potential maps must be pre-computed using FRODOCKGRID. Although vdw and electrostatics maps could be computed on the fly during the docking search, it is recommendable to create the maps beforehand in order to visualize them and check that they are consistent with the original structure. Here are the commands to generate the vdw, electrostatic, desolvation maps (receptor and ligand) in the illustrative example HyHel-5/lysozyme docking case.
```
> ../bin/frodockgrid 3hfl_fv.pdb -o 3hfl_fv_W.ccp4
> ../bin/frodockgrid 3hfl_fv.pdb -o 3hfl_fv_E.ccp4 -m 1
> ../bin/frodockgrid 3hfl_fv.pdb -o 3hfl_fv_DS.ccp4 -m 3
> ../bin/frodockgrid 3hfl_ly2.pdb -o 3hfl_ly2_DS.ccp4 -m 3
```
The receptor (3hfl_fv.pdb) and the ligand (3hfl_ly_ref.pdb) coordinates were extracted from 3hfl pdb entry of the Protein Data Bank. The ligand molecule was rotated randomly (3hfl_ly2.pdb, red on the left)to avoid pre-aligment situations.

<table border="0" cellspacing="2" cellpadding="0" align="center">
<tbody>
<tr>
<td><img title="Initial Structures" src="assets/initial_low_web.jpg" width="243" height="144" border="0" /></td>
<td align="center"> &rarr; FRODOCK &rarr;</td>
<td><img title="Experimental Solution" src="assets/dock_low_web.jpg" width="239" height="142" border="0" /></td>
</tr>
</tbody>
</table>

You can check the appearance of the generated receptor potential maps with a viewer that supports ccp4 or situs. As an example, next it is shown from left to right and from top to bottom: the receptor structure, van der Waals, electrostatic, and desolvation maps. All of them were represented in the same pose using blue color for negative and red for positive potentials. For better display arbitrary visualization thresholds were used.</p>
<table border="0" cellspacing="0" cellpadding="0" align="center">
<tbody>
<tr>
<td><img title="Receptor" src="assets/rec_low_web.jpg" border="0" /></td>
<td><img title="Receptor vdW map" src="assets/rec_vdw_low_web.jpg" border="0" /></td>
<td><img title="Receptor Electrostatic map" src="assets/rec_elecr_low_web.jpg" border="0" /></td>
<td><img title="Receptor Desolvation map" src="assets/rec_desol_low_web.jpg" border="0" /></td>
</tr>
<tr>
<td><img title="Ligand" src="assets/lig_low_web.jpg" border="0" /></td>
<td></td>
<td></td>
<td><img title="Ligand Desolvation map" src="assets/lig_desol_low_web.jpg" border="0" /></td>
</tr>  
</tbody>
</table>

### STEP 3. Perform the exhaustive docking search

Once the potential maps are generated, you are ready to perform the docking. To this end, use the following command including all the potentials (van der Waals, Electrostatic and desolvation):
```
> ../bin/frodock 3hfl_fv_ASA.pdb 3hfl_ly2_ASA.pdb -w 3hfl_fv_W.ccp4 -e 3hfl_fv_E.ccp4 --th 10 -d 3hfl_fv_DS.ccp4,3hfl_ly2_DS.ccp4 -o dock.dat
Reading receptor PDB
        * Receptor accessibility read from receptor PDB. 
                  Checking ASA in Occupancy column...
                - Ok. Occupancy column seems to show ASA values
        * Creating Accessibility map from receptor PDB...
        * Accessibility map created from receptor PDB
        * OK

Receptor VDW map information:
        * Receptor VDW map read from file
        * Map origin: -33.141129 -39.668289 -12.076756
        * Map size: 89 81 75

Receptor electrostatic map information:
        * Receptor electrostatic map read from file
        * Map modification. Threshold applied: 10.000000
        * Map origin: -33.141129 -39.668289 -12.076756
        * Map size: 89 81 75

Receptor desolvation map information:
        * Receptor electrostatic map read from file
        * Map origin: -24.141129 -30.668287 -3.076756
        * Map size: 71 63 57
        * Extra potential receptor maps
        *Number of read potentials: 0

Reading ligand pdb
        * Ligand center: 229.958908 23.243317 49.012619
        * Ligand accesilibility read from Ligand PDB. 
                  Checking ASA in Occupancy column...
                - Ok. Occupancy column seems to show ASA values
        * Ligand desolvation map read from file

Spherical radius
        * Receptor vdw radius in amstrongs: 71.000000 (71 spherical layers)
        * Ligand radius in amstrongs: 28.025059 (29 spherical layers)
        * Ligand electrostatic radius in amstrongs: 28.025059 (29 spherical layers)
        * Ligand accesibility radius in amstrongs: 28.025059 (29 spherical layers)
        * Ligand desolvation radius in amstrongs: 33.025059 (34 spherical layers)
        * Maximum ligand desolvation radius in amstrongs: 33.025059 (34 spherical layers)

Precomputation of spherical representation of ligand

Precomputation Fixed time: 43.110000 s
Determination of translational points to explore
        * Mask created
        * Number of points to explore: 18996 

Translational search init...
        * Translational search finished
        * Number of solutions: 69464    
```       
        
dock.dat is a binary file that stores all the docking solutions (ligand orientations and positions). 
Note for advanced users: The van der Waals potential term cannot be disabled; if it is not given as input it will be automatically computed. To avoid the electrostatic potential −−E option must be set to 0.0.

### STEP 4. Clustering and visualization of predictions 

Once the docking process has been performed it is possible to sort and cluster the solutions by:
```
../bin/frodockcluster dock.dat 3hfl_ly2.pdb --nc 100 -o clust_dock.dat
Sort process...
Maximum correlation: 1299.208740
Clustering process...      
```
This way a new solution file containing only 100 solutions is created. Each of these solutions represents the best element of the first 100 clusters created. The RMSD default value used in the clusterization is 5Å.

To visualize the 10 first solutions:
```
> ../bin/frodockview clust_dock.dat -r 1-10</div>
1  86.00  158.32  201.22    19.36    6.83   55.42    1359.015259
2 244.84   55.47  123.03    13.36    2.83   51.42    1340.471680
3 194.98   99.53  265.77    -6.64   14.83    9.42    1331.359131
4 314.93   81.77  144.80     5.36    2.83   51.42    1314.301514
5 307.26  142.28   57.47     7.36    6.83   51.42    1297.874390
6 350.57   90.12  270.98    -0.64   20.83   15.42    1283.359741
7 243.61   82.73   26.85    11.36    2.83   57.42    1268.607544
8 312.64  125.93   92.45     5.36   10.83   49.42    1267.174683
9 273.09  101.75   35.90     3.36    4.83   55.42    1237.752808
10 149.5  105.01  199.2     -0.64   18.83   11.42    1225.663574
```
Each line has the format: rank Euler1 Euler2 Euler3 posX posY PosZ correlation, where rank indicates the position of the solution, {Euler1,Euler2,Euler3} are the Euler angle rotation (in ZYZ convention) and {posX ,posY ,PosZ} is the X,Y,Z localization respect the center of mass of the ligand pdb (only C-alpha are used to center). Finally, the correlation is the absolute energy score obtained.

To generate the 3D coordinates of the predicted solutions:
```
../bin/frodockview clust_dock.dat -r 1-5 -p 3hfl_ly2.pdb
1  86.00  158.32  201.22    19.36    6.83   55.42   1359.01 => 3hfl_ly2_1.pdb
2 244.84   55.47  123.03    13.36    2.83   51.42   1340.47 => 3hfl_ly2_2.pdb
3 194.98   99.53  265.77    -6.64   14.83    9.42    1331.35 => 3hfl_ly2_3.pdb
4 314.93   81.77  144.80     5.36    2.83   51.42    1314.30 => 3hfl_ly2_4.pdb
5 307.26  142.28   57.47     7.36    6.83   51.42    1297.87 => 3hfl_ly2_5.pdb 
```
In this case, the first prediction (3hfl_ly2_3.pdb) is shown to be very similar to the real solution (3hfl_ly2_ref.pdb).
<table border="0" cellspacing="0" cellpadding="0" align="center">
<tbody>
<tr>
<td align="center"><img title="Final docked structures" src="assest/rec_lig_pred_web.jpg" width="291" height="197" /></td>
</tr>
</tbody>
</table>


