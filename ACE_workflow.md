## Data analyses (Segementation and Registration Combined)

AI-based Cartography of Ensembles (ACE) pipeline highlights:

1. Cutting-edge vision transformer and CNN-based DL architectures trained on very large LSFM datasets (refer to example section) to map brain-wide local/laminar neuronal activity.
2. Optimized cluster-wise statistical analysis with a threshold-free enhancement approach to chart subpopulation-specific effects at the laminar and local level, without restricting the analysis to atlas-defined regions (refer to example section).
3. Modules for providing DL model uncertainty estimates and fine-tuning.
4. Interface with MIRACL registration.
5. Ability to map the connectivity between clusters of activations.

To get more information about the workflow and its required arguments use the following command:
```miracl flow ace -h```

The following information will be printed to the terminal:
```
usage: miracl flow ace
     [-s SINGLE_TIFF_DIR]
     [-c CONTROL_BASE_DIR CONTROL_TIFF_DIR_EXAMPLE]
     [-t TREATED_BASE_DIR TREATED_TIFF_DIR_EXAMPLE]
     -sao SA_OUTPUT_FOLDER
     -sam {unet,unetr,ensemble}
     -sar X-res Y-res Z-res
     [-sag SA_GPU_INDEX]
     [-ctnd CTN_DOWN]
     [-rcao RCA_ORIENT_CODE]
     [-rcav {10,25,50}]
     [-rvad RVA_DOWNSAMPLE]
     [-rwcv {10,25,50}]
     [--rerun-registration TRUE/FALSE]
     [--rerun-segmentation TRUE/FALSE]
     [--rerun-instance-segmentation TRUE/FALSE]
     [--rerun-conversion TRUE/FALSE]
     [--no-instance-segmentation]
     [--no-validate-clusters]

   1) Segments images with ACE
   2) Convert raw tif/tiff files to nifti for registration
   3) Registers CLARITY data (down-sampled images) to Allen Reference mouse brain atlas
   4) Voxelizes high-resolution segmentation results into density maps with Allen atlas resolution
   5) Warps voxelied segmentation maps from native space to Allen atlas
   6) Generates group-wise heatmaps of cell density using the average of voxelized and warped segmentation maps in each group
   7) Computes group-level statistics/correlation using cluster-wise analysis on voxelized and warped segmentation maps

single or multi method arguments:
   user is required to pass either single or multi method arguments

   -s SINGLE_TIFF_DIR, --single SINGLE_TIFF_DIR
                           path to single raw tif/tiff data folder
   -c CONTROL_BASE_DIR CONTROL_TIFF_DIR_EXAMPLE, --control CONTROL_BASE_DIR CONTROL_TIFF_DIR_EXAMPLE
                           FIRST: path to base control directory. SECOND: example
                           path to control subject tiff directory
   -t TREATED_BASE_DIR TREATED_TIFF_DIR_EXAMPLE, --treated TREATED_BASE_DIR TREATED_TIFF_DIR_EXAMPLE
                           FIRST: path to base treated directory. SECOND: example
                           path to treated subject tiff directory

required arguments:
   (set the single or multi method arguments first)

   -sao SA_OUTPUT_FOLDER, --sa_output_folder SA_OUTPUT_FOLDER
                           path to output file folder
   -sam {unet,unetr,ensemble}, --sa_model_type {unet,unetr,ensemble}
                           model architecture
   -sar X-res Y-res Z-res, --sa_resolution X-res Y-res Z-res
                           voxel size (type: float)

useful/important arguments:
   -sag SA_GPU_INDEX, --sa_gpu_index SA_GPU_INDEX
                           index of the GPU to use (type: int; default: 0)
   -ctnd CTN_DOWN, --ctn_down CTN_DOWN
                           Down-sample ratio for conversion (default: 5)
   -rcao RCA_ORIENT_CODE, --rca_orient_code RCA_ORIENT_CODE
                           to orient nifti from original orientation to
                           'standard/Allen' orientation, (default: ALS)
   -rcav {10,25,50}, --rca_voxel_size {10,25,50}
                           labels voxel size/Resolution in um (default: 10)
   -rvad RVA_DOWNSAMPLE, --rva_downsample RVA_DOWNSAMPLE
                           downsample ratio for voxelization, recommended: 5 <=
                           ratio <= 10 (default: 10)
   -rwcv {10,25,50}, --rwc_voxel_size {10,25,50}
                           voxel size/Resolution in um for warping (default: 25)
   --rerun-registration TRUE/FALSE
                           whether to rerun registration step of flow; TRUE =>
                           Force re-run (default: false)
   --rerun-segmentation TRUE/FALSE
                           whether to rerun segmentation step of flow; TRUE =>
                           Force re-run (default: false)
   --rerun-instance-segmentation TRUE/FALSE
                           whether to rerun instance segmentation step of flow;
                           TRUE => Force re-run (default: false)
   --rerun-conversion TRUE/FALSE
                           whether to rerun conversion step of flow; TRUE =>
                           Force re-run (default: false)
   --no-instance-segmentation
                           Do not run instance segmentation (default: False).
                           Instance seg is used to identify and label neurons in
                           the image. It is useful for counting and downstream
                           tasks.
   --no-validate-clusters
                           Do not validate clusters (default: False). Validate
                           clusters is used to get native space statistics for
                           each subject based on the ouput of ACE TFCE stats.
   -rcaso, --rca_sep_orient_code
                           set flag if orientations differ between individual
                           subjects (default i.e. flag not set: False). This
                           requires an 'orientation.txt' file at the root of the
                           RAW Tiff data folder (i.e. in the folder where all RAW
                           Tiff images are stored) for EACH subject containing
                           only the respective orientation of the subject which
                           must be exactly 3 uppercase letters (e.g. 'ALS' or
                           'PLI'). This will override '--rca_orient_code' so make
                           sure that the 'orientation.txt' file exists in each
                           subject's RAW Tiff folder
  -rcaao, --rca_autodetect_sep_orient_code
                           same as '--rca_sep_orient_code' except that it only
                           expects an 'orientation.txt' file for subjects that
                           have a different orientation from the one provided to
                           '--rca_orient_code' (default i.e. flag not set:
                           False). This will use the value provided to '--
                           rca_orient_code' by default unless an
                           'orientation.txt' file is detected in the RAW Tiff
                           folder of a subject, in which case the value from the
                           'orientation.txt' file will be used. This is useful if
                           you have many subjects with the same orientation and
                           only a few exceptions

--------------------------------------------------

Use -hv or --help_verbose flag for more verbose help
```
Minimum requirement to run the ACE workflow








