# scRNAseq_analysis
Gitpod environment with tools to perform downstream analyses and comparisons of scRNAseq count data generated from Cell Ranger and STARsolo.

---

### Initiating the GitPod environment

To start a gitpod that contains tools to parse a [h5 file](https://www.10xgenomics.com/support/software/cell-ranger/latest/analysis/outputs/cr-outputs-h5-matrices) generated from [10X Cell Ranger](https://www.10xgenomics.com/support/software/cell-ranger/latest) and [10X Cell Ranger ARC](https://www.10xgenomics.com/support/software/cell-ranger-arc/latest) and tools to perform downstream analyses and comparisons of scRNAseq count data generated from Cell Ranger and [STARsolo](https://github.com/alexdobin/STAR/blob/master/docs/STARsolo.md), add `https://gitpod.io#` to the start of the web address for this repository:
[https://gitpod.io/#https://github.com/asaravia-butler/scRNAseq_analysis](https://gitpod.io/#https://github.com/asaravia-butler/scRNAseq_analysis)

<br>

### Verify that all conda environments have been installed

In your active gitpod terminal, run the following command:

```bash
conda env list
```

Your output should look as follows:  
*Note: It could take several minutes for the conda environments to install, so keep checking periodically until they are all visible.*

```bash
gitpod /workspace/scRNAseq_analysis (main) $ conda env list
# conda environments:
#
base                     /home/gitpod/miniconda3
h5py_env                 /home/gitpod/miniconda3/envs/h5py_env
single_cell_analysis_06-2024     /home/gitpod/miniconda3/envs/single_cell_analysis_06-2024
```

<br>

### Set up jupyter environment to parse an H5 file

1. Download example h5 file and respective metadata from [OSD-352](https://osdr.nasa.gov/bio/repo/data/studies/OSD-352), which was generated using the [10X multiome kit](https://www.10xgenomics.com/products/single-cell-multiome-atac-plus-gene-expression) and processed using [Cell Ranger ARC 2.0.0](https://www.10xgenomics.com/support/software/cell-ranger-arc/latest):

    ```bash
    ## Download h5 file then rename ##
    wget https://osdr.nasa.gov/geode-py/ws/studies/OSD-352/download?file=GLDS-352_snRNA-Seq_filtered_feature_bc_matrix.h5&version=1
    mv 'download?file=GLDS-352_snRNA-Seq_filtered_feature_bc_matrix.h5' GLDS-352_snRNA-Seq_filtered_feature_bc_matrix.h5

    ## Download per barcode matrics metadata for each sample then rename ##
    wget https://osdr.nasa.gov/geode-py/ws/studies/OSD-352/download?file=GLDS-352_snATAC-Seq_CF1_per_barcode_metrics.csv&version=1
    mv 'download?file=GLDS-352_snATAC-Seq_CF1_per_barcode_metrics.csv' CF1_per_barcode_metrics.csv
    wget https://osdr.nasa.gov/geode-py/ws/studies/OSD-352/download?file=GLDS-352_snATAC-Seq_CF2_per_barcode_metrics.csv&version=1
    mv 'download?file=GLDS-352_snATAC-Seq_CF2_per_barcode_metrics.csv' CF2_per_barcode_metrics.csv
    wget https://osdr.nasa.gov/geode-py/ws/studies/OSD-352/download?file=GLDS-352_snATAC-Seq_CF7_per_barcode_metrics.csv&version=1
    mv 'download?file=GLDS-352_snATAC-Seq_CF7_per_barcode_metrics.csv' CF7_per_barcode_metrics.csv
    wget https://osdr.nasa.gov/geode-py/ws/studies/OSD-352/download?file=GLDS-352_snATAC-Seq_CG8_per_barcode_metrics.csv&version=1
    mv 'download?file=GLDS-352_snATAC-Seq_CG8_per_barcode_metrics.csv' CG8_per_barcode_metrics.csv
    wget https://osdr.nasa.gov/geode-py/ws/studies/OSD-352/download?file=GLDS-352_snATAC-Seq_CG9_per_barcode_metrics.csv&version=1
    mv 'download?file=GLDS-352_snATAC-Seq_CG9_per_barcode_metrics.csv' CG9_per_barcode_metrics.csv

    ## Modify the well ID to match those used for each sample ##
    sed 's/-1/-5/g' CF1_per_barcode_metrics.csv > CF1_per_barcode_metrics_wellID5.csv
    sed 's/-1/-1/g' CF2_per_barcode_metrics.csv > CF2_per_barcode_metrics_wellID1.csv
    sed 's/-1/-4/g' CG9_per_barcode_metrics.csv > CG9_per_barcode_metrics_wellID4.csv
    sed 's/-1/-3/g' CG8_per_barcode_metrics.csv > CG8_per_barcode_metrics_wellID3.csv
    sed 's/-1/-2/g' CF7_per_barcode_metrics.csv > CF7_per_barcode_metrics_wellID2.csv
    ```

2. Run the following command to activate the h5py environment:
    ```bash
    source activate h5py_env
    ```

4. Run the following command to start a jupyter lab environment:
    ```bash
    jupyter lab --ip=0.0.0.0 --allow-root
    ```

    A window will open to connect to a Jupyter Lab server, which will require a Token.
    You can find your token by looking at the terminal of your gitpod environment.  
    Example:  
    `http://asaraviabut-scrnaseqana-dmzy6mpvp3j:8888/lab?token=53b9bd94fc09e2f4e64a8f94fa81d1edfc1ee904330ba46b`  
    In the above example, the token is: `53b9bd94fc09e2f4e64a8f94fa81d1edfc1ee904330ba46b`

5. Double click on the `CellRanger_Extract_h5.ipynb` file to open a Jupyter Notebook with commands to extract the `barcodes.tsv`, `features.tsv`, and `matrix.mtx` files containing the single cell (or nuclei) gene expression data from each sample.  


#### Shut down the jupyter environment to parse an H5 file

1. Before sutting down, make sure you have downloaded any files you wish to save from the jupyter lab environment.
  
2. "X" out of the window containing the jupyter lab.

3. Return to the window containing the gitpod environment and click `ctrl+C` then type `y` to shut down the server.

4. Run the following command to deactivate the conda environment: `conda deactivate`

<br>

### Set up jupyter environment to perform downstream analyses and comparisons of scRNAseq count data generated from Cell Ranger and STARsolo

1. Download example Cell Ranger ARC and STARsolo GEX filtered gene output files from [OSD-352](https://osdr.nasa.gov/bio/repo/data/studies/OSD-352):

    ```bash
    ## Download and uncompress output data ##
    curl -LO https://figshare.com/ndownloader/files/47649481 && unzip 47649481 && rm 47649481
    ```

2. Run the following command to activate the single_cell_analysis_06-2024 environment:
    ```bash
    source activate single_cell_analysis_06-2024
    ```

4. Run the following command to start a jupyter lab environment:
    ```bash
    jupyter lab --ip=0.0.0.0 --allow-root
    ```

    A window will open to connect to a Jupyter Lab server, which will require a Token.
    You can find your token by looking at the terminal of your gitpod environment.  
    Example:  
    `http://asaraviabut-scrnaseqana-dmzy6mpvp3j:8888/lab?token=53b9bd94fc09e2f4e64a8f94fa81d1edfc1ee904330ba46b`  
    In the above example, the token is: `53b9bd94fc09e2f4e64a8f94fa81d1edfc1ee904330ba46b`


5. **To perform a comparison of Cell Ranger ARC and STARsolo output data:** Double click on the `CellRangerARC_vs_STARsolo-Gene_GLDS-352.ipynb` file to open a Jupyter Notebook with commands to compare the filtered gene output data from Cell Ranger ARC vs. STARsolo from each sample.  


#### Shut down the jupyter environment to analyze and compare scRNAseq output data 

1. Before sutting down, make sure you have downloaded any files you wish to save from the jupyter lab environment.
  
2. "X" out of the window containing the jupyter lab.

3. Return to the window containing the gitpod environment and click `ctrl+C` then type `y` to shut down the server.

4. Run the following command to deactivate the conda environment: `conda deactivate`

<br>



