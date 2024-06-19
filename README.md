# FRODOCK
FRODOCK (Fast Rotational DOCKing) generates very efficiently many potential predictions of how  two proteins could interact. Here we give a brief overview of the necessary commands to run FRODOCK, but we strongly encourage to follow the tutorials. Right now, there are four different executables: </p>
<ul>
<li><a href="#Frodockgrid">frodockgrid</a> to pre-calculate the protein potential maps<a href="#Frodockgrid">.</a></li>
<li><a href="#Frodock">frodock</a> to perform the protein protein docking 6D exhaustive search.</li>
<li><a href="#Frodockcluster">frodockcluster</a>  for clustering frodock's raw predictions.</li>
<li><a href="#Frodockview">frodockview </a>for displaying solutions and creating pdb solutions.</li>
</ul>
## FRODOCKGRID - precomputing potential maps

To create the potential maps that will be used for scoring during the docking process, enter the following command at the prompt:
<div class="box-content">frodockgrid &lt;pdb&gt;   -m &lt;int&gt;</div>
<p>where:</p>
<table style="width: 550px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="89" /> <col width="453" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>pdb</p>
</td>
<td width="453">
<p>The initial model structure to be fitted (PDB format)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−m &lt;int&gt;</p>
</td>
<td width="453">
<p>Type map to generate:</p>
<ul>
<li>
<p><i>0: </i>[default] van der Waals potential map.</p>
</li>
<li>
<p><i>1: </i>Electrostatic potential map.</p>
</li>
<li>
<p><i>2: </i>Charge projection map.</p>
</li>
<li>
<p><i>3: </i>Desolvation potential + PDB file with ASA in Occupancy column + ASA projection map.</p>
</li>
</ul>
</td>
</tr>
</tbody>
</table>
<p>Basic Options</p>
<hr size="1" />
<p>In this section, the basic options to customize the potential maps are detailed. Just add them after the minimum command shown above.</p>
<table style="width: 550px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="89" /> <col width="453" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−o &lt;string&gt;</p>
</td>
<td width="453">
<p>Volume output filename (default: frodockgrid.ccp4).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−−gs &lt;float&gt;</p>
</td>
<td width="453">
<p>Volume output grid size in amstrongs (default: 1.0).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−t &lt;char&gt;</p>
</td>
<td width="453">
<p>Type of interaction (used only with -m 1):<br /><br /> E: Enzyme/Inhibitor or Enzyme/substrate.<br /><br /> A: Antobody/Antigen.<br /><br /> O: Others(Default).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−−wr &lt;float&gt;</p>
</td>
<td width="453">
<p>Water probe radius in amstrongs. To be used only when -m={0,3} (default: 1.4).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−−sr &lt;float&gt;</p>
</td>
<td width="453">
<p>Desolvation probe radius in amstrongs. To be used only when -m={0,3} (default: 1.95).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−h</p>
</td>
<td width="453">
<p>Displays usage information and exits.</p>
</td>
</tr>
</tbody>
</table>
<p>Advanced options</p>
<hr size="1" />
<p>Only for expert users!</p>
<table style="width: 600px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="104" /> <col width="488" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="104">
<p>−−hyd</p>
</td>
<td width="488">
<p>Consider hydrogen in the creation of the potential map.</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="104">
<p>−−fp &lt;int&gt;</p>
</td>
<td width="488">
<p>Forcefield parameters:</p>
<ul>
<li>
<p><i>0: </i>[default] CHARMM</p>
</li>
<li>
<p>1: ICM</p>
</li>
<li>
<p>2: EEF1</p>
</li>
<li>
<p>3: Sybil</p>
</li>
</ul>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="104">
<p>−−an &lt;int&gt;</p>
</td>
<td width="488">
<p>Atom name convention:</p>
<ul>
<li>
<p><i>0: </i>[default] IUPAC</p>
</li>
<li>
<p><i>1: </i>PDB</p>
</li>
</ul>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="104">
<p>−−rm &lt;float&gt;</p>
</td>
<td width="488">
<p>Van der Walls radius modification (default: 1. DEPRECATED)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="104">
<p>−p</p>
</td>
<td width="488">
<p>Use forces in fact column of the PDB file</p>
</td>
</tr>
</tbody>
</table>
<p> </p>
<p><b>FRODOCK - Docking process</b></p>
<hr />
<p>To perform a basic shape (van de Waals) docking enter the following command at the prompt:</p>
<div class="box-content">frodock &lt;recept_pdb&gt; &lt;ligand_pdb&gt; --E 0</div>
<p>Alternatively, by removing the --E flag the electrostaics can be considered. You could include the vdW and electrostatics precalculated maps using -w and -e &lt;string&gt; options, respectively. These simple docking alternatives can be useful as a first aproximation.The recommended option is to include vdw, electrostatic and desolvation terms, i.e.:</p>
<div class="box-content">frodock rec_ASA.pdb lig_ASA.pdb −d lig_sol.cpp4,rec_sol.cpp4</div>
<p>When using desolvation it is madatory ligand and receptor pdb input files with the atom ASA values in the Occupancy column. These files are standard output of frodockgrid, please do not forget to use them instead of the original ones. Exploiting pre-calculations to gain efficiency, an equivalent command line will be:</p>
<div class="box-content">frodock rec_ASA.pdb lig_ASA.pdb  −w rec_vdw.cpp4 −e rec_ele.cpp4 −d lig_solv.cpp4,rec_solv.cpp4</div>
<p><br />where:</p>
<table style="width: 657px; height: 296px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="36" /> <col width="220" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="14%">
<p>&lt;recept_pdb&gt;</p>
</td>
<td width="86%">
<p>The receptor structure PDB.</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="14%">
<p>&lt;ligand_pdb&gt;</p>
</td>
<td width="86%">
<p>The ligand structure PDB (the smallest one recommended).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="14%">
<p>−w &lt;string&gt;</p>
</td>
<td width="86%">
<p>vdW receptor potential file (default: Created from receptor pdb file).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="14%">
<p>−e &lt;string&gt;</p>
</td>
<td width="86%">
<p>Electrostatic receptor map</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="14%">
<p>−−E &lt;float&gt;</p>
</td>
<td width="86%">
<p>Electrostatic term weight (default: 0.3)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="14%">
<p>−d &lt;string&gt;</p>
</td>
<td width="86%">
<p>Desolvation potential maps for receptor and ligand (syntax: -d receptor_map.ccp4,ligand_map.ccp4).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="14%">
<p>−−D &lt;float&gt;</p>
</td>
<td width="86%">
<p>Desolvation term weight (default: 0.5)</p>
</td>
</tr>
</tbody>
</table>
<p>Basic Options</p>
<hr size="1" />
<p>In this section, the basic options to customize the docking process are detailed.</p>
<table style="width: 550px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="139" /> <col width="403" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="139">
<p>−o &lt;string&gt;</p>
</td>
<td width="403">
<p>Solutions output filename (default: frodock_output.dat).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="139">
<p>−−bw &lt;int&gt;</p>
</td>
<td width="403">
<p>Bandwidth in spherical harmonic representation. Define rotational stepsize (default: 32. Rotational stepsize ~6°)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="139">
<p>−−lmax &lt;float&gt;</p>
</td>
<td width="403">
<p>External Mask reduction ratio. (default: 0.23)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="139">
<p>−−lmin &lt;float&gt;</p>
</td>
<td width="403">
<p>Internal Mask reduction ratio. (default: 0.43)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="139">
<p>−−th &lt;float&gt;</p>
</td>
<td width="403">
<p>Electrostatic map threshold. (default: 10.0)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="139">
<p>−−lw &lt;float&gt;</p>
</td>
<td width="403">
<p>Width between spherical layers in amstrongs. (default: 1.0)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="139">
<p>−−st &lt;float&gt;</p>
</td>
<td width="403">
<p>Translational search stepsize in amstrongs. (default: 2.0)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="139">
<p>−h</p>
</td>
<td width="403">
<p>Displays usage information and exits.</p>
</td>
</tr>
</tbody>
</table>
<p>Advanced options</p>
<hr size="1" />
<p>Only for expert users!</p>
<table style="width: 589px; height: 531px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="114" /> <col width="428" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−W &lt;float&gt;</p>
</td>
<td width="428">
<p>Van der Waals term weight (default: 1.0)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−−np &lt;int&gt;</p>
</td>
<td width="428">
<p>Number of solutions stored per traslational position. (default: 4)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−−rd &lt;float&gt;</p>
</td>
<td width="428">
<p>Minimal rotational distance allowed between close solutions in degrees. (default: 12.0)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−−nt &lt;int&gt;</p>
</td>
<td width="428">
<p>Number of solutions stored in the search. (default: unlimited)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−−td &lt;float&gt;</p>
</td>
<td width="428">
<p>Maximal translational distance to consider close solutions in grid units. (default: 0)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−a&lt;string&gt;</p>
</td>
<td width="428">
<p>Receptor and ligand ASA maps (default: created from receptor and ligand pdb files if Desolvation potential used. Syntax: -a asa_receptor,asa_ligand).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−−around &lt;string&gt;</p>
</td>
<td width="428">
<p>translational search restricted to a region around a point defined in "x,y,z" format.</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−−nover</p>
</td>
<td width="428">
<p>Show no messages.</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="114">
<p>−−fp &lt;int&gt;</p>
</td>
<td width="428">
<p>Forcefield terms employed:</p>
<p>0: Rosseta (default)</p>
<p>1: ICM</p>
<p>2: EEF1</p>
<p>3: Sybil</p>
</td>
</tr>
</tbody>
</table>
<p> </p>
<p><b>FRODOCKCLUSTER - Sort and cluster frodock output file</b></p>
<hr />
<p>To sort and cluster the docking solutions, enter the following command at the prompt:</p>
<div class="box-content">frodockcluster &lt;data&gt; &lt;pdb</div>
<p>where:</p>
<table style="width: 550px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="82" /> <col width="460" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="82">
<p>&lt;data&gt;</p>
</td>
<td width="460">
<p>Frodock solutions data filename. (required)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="82">
<p>&lt;pdb&gt;</p>
</td>
<td width="460">
<p>Ligand pdb filename. (required)</p>
</td>
</tr>
</tbody>
</table>
<p> </p>
<table style="width: 550px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="109" /> <col width="433" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="109">
<p>−o &lt;string&gt;</p>
</td>
<td width="433">
<p>Clustered solution output Frodock filename (default: frodockcluster.dat)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="109">
<p>−d &lt;float&gt;</p>
</td>
<td width="433">
<p>RMSD distance between clusters (default: 5.0).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="109">
<p>−−nc &lt;int&gt;</p>
</td>
<td width="433">
<p>Maximum number of clusters to generate (default: unlimited).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="109">
<p>−h</p>
</td>
<td width="433">
<p>Displays usage information and exits.</p>
</td>
</tr>
</tbody>
</table>
<p>Only for real expert users!</p>
<table style="width: 588px; height: 183px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="89" /> <col width="453" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−−ns &lt;int&gt;</p>
</td>
<td width="453">
<p>Limit the number of solutions employed (default all the solutions are used)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−e</p>
</td>
<td width="453">
<p>Input solution file is a evaluated Frodock file.</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−−ns</p>
</td>
<td width="453">
<p>The solutions in input file are sorted by correlation (Skip sort operation).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−c &lt;float&gt;</p>
</td>
<td width="453">
<p>Ratio of maximal correlation used to define correlation interval in clusters (default: 1.0).</p>
</td>
</tr>
</tbody>
</table>
<p> </p>
<p><b>FRODOCKVIEW - Show and create pdb solutions</b></p>
<hr />
<p>To show docking solutions and create resulting PDBs, enter the following command at the prompt:</p>
<div class="box-content">frodockview &lt;input&gt;</div>
<p>where:</p>
<table style="width: 550px;" border="0" cellspacing="0" cellpadding="2"><colgroup> <col width="89" /> <col width="453" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>&lt;input&gt;</p>
</td>
<td width="453">
<p>Frodock solutions input filename (required)</p>
</td>
</tr>
</tbody>
</table>
<p>Other options</p>
<table style="width: 500px; height: 309px;" border="2" cellspacing="1" cellpadding="2"><colgroup> <col width="89" /> <col width="453" /> </colgroup>
<tbody>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−r &lt;string&gt;</p>
</td>
<td width="453">
<p>Range of solutions showed (default: all solutions).</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−p &lt;string&gt;</p>
</td>
<td width="453">
<p>Generate pdb files applying solutions coordinates showed over Ligand pdb file (default: No files created</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−−size</p>
</td>
<td width="453">
<p>Show the total number of solution in file.</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−e</p>
</td>
<td width="453">
<p>Input file is a Frodock evaluated file.</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−d</p>
</td>
<td width="453">
<p>Disable solution files creation</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−f &lt;string&gt;</p>
</td>
<td width="453">
<p>Reference PDB file for RMSD evaluation of solutions (Requires -p option)</p>
</td>
</tr>
<tr>
<td bgcolor="#f0f0f0" width="89">
<p>−h</p>
</td>
<td width="453">
<p>Displays usage information and exits.</p>
</td>
</tr>
</tbody>
</table>


