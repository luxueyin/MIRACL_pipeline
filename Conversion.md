# Conversion from tiff to nifti

Convert raw data tiff files to nifti for analyses or visualation. Additionally, this can also be used prior to running Registration to produce a nifti downsample nii file.



Usage: 
```miracl conv tiff_nii -f [Tiff folder]```

Required arguments:
```
      -f dir, --folder dir  Input CLARITY TIFF folder/dir

    optional arguments:
      -w,  --work_dir       Output directory (default: working directory)
      -d , --down           Down-sample ratio (default: 5)
      -cn , --channum       Chan # for extracting single channel from multiple channel data (default: 0)
      -cp , --chanprefix    Chan prefix (string before channel number in file name). ex: C00
      -ch , --channame      Output chan name (default: eyfp)
      -o , --outnii         Output nii name (script will append downsample ratio & channel info to given name)
      -vx , --resx          Original resolution in x-y plane in um (default: 5)
      -vz , --resz          Original thickness (z-axis resolution / spacing between slices) in um (default: 5)
      -c  [ ...], --center  [ ...]
                            Nii center (default: 0,0,0 ) corresponding to Allen atlas nii template
      -dz , --downzdim      Down-sample in z dimension, binary argument, (default: 1) => yes
      -pd , --prevdown      Previous down-sample ratio, if already downs-sampled
      -pct, --percentile_thr Percentile value for thresholding extreme values (default: 0)
      -h, --help            Show this help message and exit
```

**Example:**
To convert tiff raw data into a nifti file. First, we create an output directory (next time try if we can simply enter the command and the output directory can be created automatically):
```mkdir conv_Ex_561_Em_600_stitched```
Next run the command to convert tiff files from directory **Ex_561_Em_600_stitched** and place the newly created nifti file into the output directory **conv_Ex_561_Em_600_stitched**:
```miracl conv tiff_nii -f Ex_561_Em_600_stitched -w conv_Ex_561_Em_600_stitched```

