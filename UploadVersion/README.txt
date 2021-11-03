## Mitochondrial nanontunnel segmentation workflow
# Authors: Grey P. Madison, Dillon J. Chung 
# Affiliation: Lab of Cardiac Energetics, NHLBI, NIH
# Date: 2020.01.15

#Description:
This workflow isolates mitochondrial nanotunnels from segmented mitochondrial volumes and provides geometric data of the resulting structures.

This protocol requires a local install of MeshLab (https://www.meshlab.net). 

The workflow first converts mitochondrial volumes from the STL format to the OFF format (using MeshLab functions; input folder: "[YourFilePathHere]/nanotunnel_detection/20200724Version/STLs"). Next the workflow uses a triangulated surface mesh approximation to simplify the volume for subsequent analysis. This is followed by iterative repairs of errors in the volumes that cause failure of the submesh segmentation program (e.g., holes and folded faces; using MeshLab functions). Finally, the workflow conducts submesh segmentation and provides geometric information on all submeshes as an output (output folder: "[YourFilePathHere]/nanotunnel_detection/20200724Version/ProgramOutput").

Most of the workflow can be completed using the ProcessSTLsFINAL.bat. The user needs to complete filepath setup ("[YourFilePathHere]" in "ProcessSTLsFINAL.bat") and manual checking of the results.

#Notes:
We limited our analysis to batches of 200 mitochondrial volumes. Adjust according to your computer's processing power.

There are some parameters that can be adjusted in the .bat as outlined below:

"[YourFilePathHere]/nanotunnel_detection/20200724Version/Program/TriangulatedSurfaceMeshApproximation2.exe" "[YourFilePathHere]/nanotunnel_detection/20200724Version/OFFs/" "[YourFilePathHere]/nanotunnel_detection/20200724Version/OFFs_simplified" 10 2500 3
	- 10: The maximum number of volumes processed in parallel, 
	  2500: Value passed to the "max_number_of_proxies" parameter of the GCAL 5.2 surface mesh approximation function (i.e. number of proxy seeds used to approximate a new surface mesh), 
	  3: Value passed to the "number_of_iterations" parameter of the GCAL 5.2 surface mesh approximation function (i.e. number of Lloyd's algorithm iterations run on the proxy seeds)
	  *For a detailed explanation of the Triangulated Surface Mesh Approximation function see https://doc.cgal.org/latest/Surface_mesh_approximation/index.html

"[YourFilePathHere]/nanotunnel_detection/20200724Version/Program/20200804NanotunnelSegmenter.exe" "[YourFilePathHere]/nanotunnel_detection/20200724Version/OFFs_simplified_13/" "[YourFilePathHere]/nanotunnel_detection/20200724Version/ProgramOutput" 8
	- 8: The maximum number of files processed in parallel

The workflow outputs a summary CSV and submesh segmentation for every mitochondrial volume. These results can be viewed in MeshLab and should be compared with mitochondrial volumes in the original dataset (to check for localization and connectivity).

The only inputs for this workflow are mitochondrial volumes in the STL format. Each volume must have a unique filename.

Prior to running the .bat, ensure that that the following file folders are empty:

"[YourFilePathHere]/nanotunnel_detection/20200724Version/OFFs_simplified"
"[YourFilePathHere]/nanotunnel_detection/20200724Version/OFFs_simplified_#"
	- # = 1 to 13
	- Note: these file folders are kept separate in the event that the repair process fails on a given volume preventing an attempt to run the nanotunnel segmenter on a 'corrupt' volume.
"[YourFilePathHere]/nanotunnel_detection/20200724Version/ProgramOutput"

