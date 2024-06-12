# InfoGenomeR
- InfoGenomeR is the Integrative Framework for Genome Reconstruction that uses a breakpoint graph to model the connectivity among genomic segments at the genome-wide scale. InfoGenomeR integrates cancer purity and ploidy, total CNAs, allele-specific CNAs, and haplotype information to identify the optimal breakpoint graph representing cancer genomes.

<p align="center">
    <img width="1500" src="https://github.com/YeonghunL/InfoGenomeR/blob/master/doc/overview.png">
  </a>
</p>

# Snakemake install
- The InfoGenomeR workflow is run by the snakemake.
- Environments are described in workflow/envs.
- Rules are described in workflow/Snakefile.
```
wget https://github.com/conda-forge/miniforge/releases/download/24.1.2-0/Miniforge3-Linux-x86_64.sh
bash Miniforge3-Linux-x86_64.sh
mamba create -c conda-forge -c bioconda -n snakemake snakemake
conda config --set channel_priority strict
conda activate snakemake
```

# Conda environment setting
```
snakemake --core all --use-conda InfoGenomeR_env
```
# Dataset download
```
snakemake --cores all --use-conda InfoGenomeR_download
```

# InfoGenomeR workflow
Take a low coverage example (~50G)
```
snakemake --core all --use-conda InfoGenomeR_example_download
```
## Set a workspace
```
# go to the InfoGenomeR base directory
cd InfoHiC

# make a workspace directory
workspace_dir=InfoGenomeR_workspace1
mkdir -p ${workspace_dir}

# link the reference in the workspace directory
ln -s ${PWD}/humandb/ref ${workspace_dir}/ref
```
## Starting from fastq
Take the low coverage example in examples/fastq
### Inputs should be located in the workspace
  - fastq/normal1.fq.gz (optional for somatic)
  - fastq/normal2.fq.gz (optional for somatic)
  - fastq/tumor1.fq.gz
  - fastq/tumor2.fq.gz
```
ln -s ${PWD}/examples/fastq ${workspace_dir}/fastq
```
Then, go to [InfoGenomeR run](#infogenomer-run)
## Starting from bam
### Inputs should be located in the workspace 
Here, the bam folder would be yours, which should be named as below.
  - bam/tumor_sorted.bam
  - bam/normal_sorted.bam
```
ln -s bam ${workspace_dir}/bam
```
Then, go to [InfoGenomeR run](#infogenomer-run)
## InfoGenomeR run
Select either somatic or total mode
### Somatic run 
```
# Run the InfoGenomeR workflow. The example is triploidy
snakemake --core all --use-conda ${workspace_dir}/InfoGenomeR_output --config mode=somatic min_ploidy=2.5 max_ploidy=3.5 
```
### Total run
```
snakemake --core all --use-conda ${workspace_dir}/InfoGenomeR_output --config mode=total min_ploidy=2.5 max_ploidy=3.5 
```

