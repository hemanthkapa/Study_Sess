## Abstract: 

TARDIS is a tool that allows quick and accurate segmentations of macromolecular structures. Reduces the manual effort which usually takes months to go through the EM data. TARDIS uses novel geometric transformer architecture to provide accurate instance segmentations. 

## Introduction: 
- Advancements in the EM technology, from improved microscope optics to sample prep techniques to data collection annotations have sped up the acquisition of quality data. 
- Despite all the advancements tomogram analysis is a still a major challenge, especially when it comes to labelling biomolecules. 
- Unlike single particle analysis labelling big structures is relatively harder due to their complex shapes. Even with new methods for annotation the process is still slow and the workflow is not fully automated and still needs manual effort. 
- Many tools have been created recently for labelling large biomolecules but often these tools are slow and may not be very accurate. Instance Segmentation(separating individual objects) is still a big problem (closely packed filaments). 
- Tools like Fiji, Napari, IMOD are accurate but needs a lot of manual annotation. Other tools like TomoSegMemTC, Amira are specialized for certain structures but needs high quality images and lots of adjustments. Newer tools like Dragonfly add some automation but still need manual labelling, fine tuning or doesnt work for all data and tasks. 
- Researchers often have to use several tools and spend a lot of time customizing them for each dataset.

## Tardis: 
- Transformer based Rapid Dimensionless Instance Segmentation.
- Tardis can do both semantic segmentation and instance segmentation an dit first tool to offer highly accurate instance segmentation
- It uses pre-trained network for labelling membrane and microtubules. Trained in the biggest dataset of its kind so far. 
- It is flexible so it can be expanded to label new types of molecules by adding segmentation networks. Uses Dimension instance Segmentation transformer that works well for identifying individual objects from the semantic maps. 
- Impoved accuracy by 42% for microtubules and 55 % for membranes compared to other tools. 2x faster than other tools for membranes and 5x faster for microtubules. 

## Results: 

- Specialized segmentation models for sub-cellular features. 
	- Microtubules in 3D electron tomograms (cryo and plastic sections)
	- Microtubules in 2D fluorescence images
	- Membranes in 3D cryo-electron tomograms
	- Membranes in 2D cryo-electron micrographs
	- Actin filaments in 3D cryo-electron tomograms
- There are two different DIST models: one for liner objects (like filaments) and one for surfaces (like membranes in 3D)
### High Accuracy Semantic Segmentation with FNet

![[2024.12.19.629196v1.full.pdf#page=47&rect=46,349,557,744|2024.12.19.629196v1.full, p.47]]

- Single Encoder, Dual Decoders: FNet uses one UBet-style encoder to extract features from the input image. It then splits into two concurrent decoders: 
	- One decoder uses conventional skip connections 
	- The other uses fully connected skip connections which likely allow for more global context for the segmentation
- The model generated probability maps which are then converted to binary masks. These are then processed into point clouds for downstream analysis. 

### Geometry aware instance segmentation with DIST
- Point clouds are creatd from voxel-level probability maps using skeletonization, which extracts skeletion of segmented structures. 
- DIST model predicts graph edges between points in the point cloud to define individual object instances. 
- The model works with point clouds of any dimentinality uses an SO(n) equivariant representaion. Geometry aware operations are integrated with transformer network
### TARDIS accurately segments microtubule and membrane instances in tomograms and micrographs

- The authors benchmarked TARDIS against existing tools: Amira, MemBrain-seg V2, and TomoSegMemTV, focusing on microtubule and membrane segmentation across datasets with different quality and resolution.
- **Semantic segmentation** was evaluated using:
	- F1 score
	- Precision
	- Recall
	- Average precision (AP)
	- Precision at 90% recall (P90)
- **Instance segmentation** was evaluated using the mean coverage (mCov) score, which measures the overlap between ground truth and predicted point clouds.
- All metrics range from 0 to 1, with higher values indicating better performance.
### Segmenting TMV in cryo-ET without TARDIS re-training
- Tardis was tested on Tobacco Mosaic Virus (TMV) segmentation in cryo-ET datasets without any model retraining. 
- The pre trained FNet (for 3D microtubles) and DIST-linear models were used as is
- Despite structural differences between microtubules and TMV, TARDIS successfully segmented TMV filaments, showing strong generalization and minimal need for manual intervention. 
### TARDIS can segment actin filament trained in limited scenarios
- TARDIS was evaluated for acting filament segmentation using only three annotated tomograms. Both Tardis FNet and UNet were trained on this limited data for comparison. 
- TARDIS outperformed UNet on all metrics achieving a higher MCov score. 

### Tardis uncovers variability in wild-type versus phosphomimetic mutant U2OS cells 
- During cell division, microtubules connect chromosomes to the mitotic spindle via the kinetochore, where the NDC80 complex is the main mictrotubule-binding interface. 
- Phosphorylation of the Hec1 subunit's N-terminal tail in NDC80s reduced its affinity for microtubules, increasing detachment rates; dephosphorylation stabilizes attatchments. 
- Previous studies showed KMT (kinetochore microtubule) lifetime decreases with increased phosphorylation but the ultrastructural consequences were unclear. 
- Amira is too slow for large-scale studies. TARDIS segmented 14,452 (WT), 8804 (9D) and 10,336(9A) microtubules per cell. Manually identified KMTs: 1,228 (WT), 61 (9D), 251 (9A).
- Average KMTs per kinetochore: 10.2 (WT), 1.58 (9D), 6.5 (9A).
- Microtubule and KMT length distributions were quantified. Results align with previous findings: phosphorylation (9D) leads to fewer, less stable KMTs; dephosphorylation (9A) stabilizes attachments.
- Only one cell per condition was analyzed; more cells are needed for statistical robustness. TARDIS enables high-throughput segmentation, reducing analysis time from months to hours. 


![[2024.12.19.629196v1.full.pdf#page=34&rect=62,172,549,693|2024.12.19.629196v1.full, p.34]]

[[Semantic Segmenation]]: Assigns a class for different things. For example person, tree, grees. But we cannot differentiate between instances from same class. 

[[Instance Segmentation]]: Closely related to object detection. Output is a mask containing the object instead of a bounding box. Unlike semantic segmentation we do not label every pixel in the image. 

[[Panoptic segmentation]]: Combination of semantic and instance segmentations. 

