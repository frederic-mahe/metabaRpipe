#metabaRpipe:


# Configure a dedicated conda environment:


### Install conda
<https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html>
### Create a dedicated conda environment:
	$ conda create -n metabaRpipe -y
### Activate conda environment:
	$ conda activate metabaRpipe
### install R and atropos:
	(metabaRpipe)$ conda install -c bioconda atropos -y
	(metabaRpipe)$ conda install -c conda-forge r-base -y
	(metabaRpipe)$ conda install -c conda-forge r-devtools -y
	(metabaRpipe)$ conda install -c conda-forge r-optparse -y
	
### start R from the terminal an install R packages:
	(metabaRpipe)$ R
	
	devtools::install_github('tidyverse/tidyverse')
	devtools::install_github("KlausVigo/phangorn")
	devtools::install_github("benjjneb/dada2")

	if (!requireNamespace("BiocManager", quietly = TRUE))
	install.packages("BiocManager")
	BiocManager::install("ShortRead");BiocManager::install("DECIPHER");BiocManager::install("phyloseq")

	quit(save = "no")
	
## Clone the repository:

This step is required to get the Rscripts locally.

Change the directory where you would like to clone the repository.

	$ MY_DIR=/path_to_mydir/whereIwant/metabaRpipe/to/be/installed/
	$ cd ${MY_DIR}

Use ``git clone`` to clone on your computer the repository including the functions and test data.

	$ git clone https://github.com/fconstancias/metabaRpipe.git

## Running the pipeline from the terminal:

Everything is now ready to analyse your raw data.
There are several options: working within R or the terminal using Rscripts enabling you to run the pipeline and generate a phyloseq object from raw sequencing data using one command.


First, activate the dedicated conda environment:

	$ conda activate metabaRpipe


Then, use ``Rscript`` to run the pipeline and specify some parameters e.g.: *databases* 

For instance, using the `test-data` available on the repository:

```bash
(metabaRpipe)$ Rscript ${MY_DIR}metabaRpipe/Rscripts/dada2_metabarcoding_pipeline.Rscript \
-i metabaRpipe/test-data/ \
--preset V3V4 \
--db ~/db/DADA2/silva_nr99_v138_train_set.fa.gz \
--db_species ~/db/DADA2/silva_species_assignment_v138.fa.gz \
--metadata metabaRpipe/metadata.xlsx \
--save_out test_pipe_Rscript.RDS \
-f ${MY_DIR}metabaRpipe/Rscripts/functions.R > mylogs.txt 2>&1
```		
The ``> mylogs.txt 2>&1`` trick will redirect what is printed on the screen to a file including potential errors and also parameters that you used.


## Exploring the outputs:

By default the script generate the outputs under a ```dada2``` directory. Below the subfolders and outputs generated by the command:

```bash
(metabaRpipe)$tree  dada2/
dada2/
├── 00_atropos_primer_removed
│   ├── 180914_M00842_0310_000000000-C3GBT
│   │   ├── R1F1-S66_primersout_R1_.fastq.gz
│   │   ├── R1F1-S66_primersout_R2_.fastq.gz
│   │   ├── R1F2-S300_primersout_R1_.fastq.gz
│   │   ├── R1F2-S300_primersout_R2_.fastq.gz
│   │   ├── R1F3-S90_primersout_R1_.fastq.gz
│   │   └── R1F3-S90_primersout_R2_.fastq.gz
│   └── 190719_M00842_0364_000000000-CKGHM
│       ├── Y2A15-2M-06-S78_primersout_R1_.fastq.gz
│       ├── Y2A15-2M-06-S78_primersout_R2_.fastq.gz
│       ├── Y2A15-2M-12-S77_primersout_R1_.fastq.gz
│       ├── Y2A15-2M-12-S77_primersout_R2_.fastq.gz
│       ├── Y3-R1F4-S136_primersout_R1_.fastq.gz
│       └── Y3-R1F4-S136_primersout_R2_.fastq.gz
├── 01_dada2_quality_profiles
│   ├── 180914_M00842_0310_000000000-C3GBT
│   │   ├── 180914_M00842_0310_000000000-C3GBT_forward.pdf
│   │   └── 180914_M00842_0310_000000000-C3GBT_reverse.pdf
│   └── 190719_M00842_0364_000000000-CKGHM
│       ├── 190719_M00842_0364_000000000-CKGHM_forward.pdf
│       └── 190719_M00842_0364_000000000-CKGHM_reverse.pdf
├── 02_dada2_filtered_denoised_merged
│   ├── 180914_M00842_0310_000000000-C3GBT
│   │   ├── 180914_M00842_0310_000000000-C3GBT.RData
│   │   ├── 180914_M00842_0310_000000000-C3GBT_seqtab.rds
│   │   ├── 180914_M00842_0310_000000000-C3GBT_track_analysis.tsv
│   │   ├── errors_180914_M00842_0310_000000000-C3GBT_fwd.pdf
│   │   ├── errors_180914_M00842_0310_000000000-C3GBT_rev.pdf
│   │   └── seq_distrib_180914_M00842_0310_000000000-C3GBT.pdf
│   └── 190719_M00842_0364_000000000-CKGHM
│       ├── 190719_M00842_0364_000000000-CKGHM.RData
│       ├── 190719_M00842_0364_000000000-CKGHM_seqtab.rds
│       ├── 190719_M00842_0364_000000000-CKGHM_track_analysis.tsv
│       ├── errors_190719_M00842_0364_000000000-CKGHM_fwd.pdf
│       ├── errors_190719_M00842_0364_000000000-CKGHM_rev.pdf
│       └── seq_distrib_190719_M00842_0364_000000000-CKGHM.pdf
├── 03_dada2_merged_runs_chimera_removed
│   ├── no-chim-seqtab.fasta
│   ├── no-chim-seqtab.rds
│   ├── physeq.rds
│   ├── seqtab_distrib.pdf
│   └── track_analysis.tsv
├── 04_dada2_taxonomy
│   ├── silva_nr99_v138_train_set_assignation.rds
│   └── silva_nr99_v138_train_set_table.tsv
└── phyloseq.RDS
```	


A `phyloseq` object has been generated and is waiting for you with the provided  metadata:

```R
librayry(phyloseq)
readRDS("dada2/phyloseq.RDS")
phyloseq-class experiment-level object
otu_table()   OTU Table:         [ 322 taxa and 6 samples ]
sample_data() Sample Data:       [ 6 samples by 18 sample variables ]
tax_table()   Taxonomy Table:    [ 322 taxa by 7 taxonomic ranks ]
refseq()      DNAStringSet:      [ 322 reference sequences ]
```


In addition, since `--save_out test_pipe_Rscript.RDS ` is specified all the outputs are also saved within this `R` object.

```R
readRDS("test_pipe_Rscript.RDS") -> out

ls(out)
[1] "filtering_denoising" "merging"             "physeq"             
[4] "qplot"               "taxo" 
```


## Raw data structure:

Please pay attention to structure of the raw sequencing data:

Fwd and Rev reads (*_R1_* and *_R2_*, respectively) are placed in run specific directory since error learning and ASV inference has to be perform on a run basis. If you are only analyzing one sequencing run, simply add only one subdirectory.


```bash
(metabaRpipe)$ tree metabaRpipe/test-data/
metabaRpipe/test-data/
├── 180914_M00842_0310_000000000-C3GBT
│   ├── R1F1-S66_L001_R1_001.fastq.gz
│   ├── R1F1-S66_L001_R2_001.fastq.gz
│   ├── R1F2-S300_L001_R1_001.fastq.gz
│   ├── R1F2-S300_L001_R2_001.fastq.gz
│   ├── R1F3-S90_L001_R1_001.fastq.gz
│   └── R1F3-S90_L001_R2_001.fastq.gz
├── 190719_M00842_0364_000000000-CKGHM
│   ├── Y2A15-2M-06-S78_L001_R1_001.fastq.gz
│   ├── Y2A15-2M-06-S78_L001_R2_001.fastq.gz
│   ├── Y2A15-2M-12-S77_L001_R1_001.fastq.gz
│   ├── Y2A15-2M-12-S77_L001_R2_001.fastq.gz
│   ├── Y3-R1F4-S136_L001_R1_001.fastq.gz
│   └── Y3-R1F4-S136_L001_R2_001.fastq.gz
└── metadata.xlsx
```

By default, sample names are retrieved from files names after the first ```_```, Fwd and Rev fastq files should be recognised by ```*_R1_*``` and ```*_R2_*```, respectively and a ```sample_name``` column in the metadata```.xlsx``` file links the sample names retrieved from the files together with the metadata.


## Definying your own presets:


The ```--preset``` option is the most important parameter here since it will be used to remove primers used for PCR amplification - as recommended by dada2 author and also adjust the length filtering and trimming parameters. For instance, ```V3V4``` is related to the following primers, dada2 options.

```
(metabaRpipe)$ open ${MY_DIR}metabaRpipe/Rscripts/functions.R 
...
  if(V == "V3V4"){
    
    PRIMER_F = "CCTAYGGGRBGCASCAG"
    PRIMER_R = "GGACTACNNGGGTATCTAAT"
    trim_length = c(240,600)
    trunclen =  c(260,250)
    maxee = c(4,5)
    minLen = 160
    minover = 10
    }
...
```

The sequences ```CCTAYGGGRBGCASCAG``` and ```GGACTACNNGGGTATCTAAT``` will be searched by ```atropos``` as a Fwd and Rev primers, respectively and removed. Only reads containing the expected primers will be kept.

Fwd and Rev reads will by truncated after ```260``` and ```250``` nucleotide positions, reads shorter then `160` nucleotides will be removed as well as the Fwd with a maximum expected error more then `4` and Rev of `5`. `10` nucleotides will be used to merged denoised Fwd and Rev reads and only ASV `>240 length <600` will be kept.

You can modify the `${MY_DIR}metabaRpipe/Rscripts/functions.R` script by adding any `preset` of your choice adding another if statement and using the same variable names. Then  `--preset mypreset` can be used to call the parameters you defined in `${MY_DIR}metabaRpipe/Rscripts/functions.R` when running the script ```(metabaRpipe)$ Rscript ${MY_DIR}metabaRpipe/Rscripts/dada2_metabarcoding_pipeline.Rscript ```. This allows to automatise the process and run the script from your terminal or an HPC cluster.


## Getting some help:


All the options can be access using `--help`.

```(metabaRpipe)$ Rscript ${MY_DIR}metabaRpipe/Rscripts/dada2_metabarcoding_pipeline.Rscript --help

Options:
	-i CHARACTER, --input_directory=CHARACTER
		Path of the input directory containing raw _R1_ and _R2_ raw reads in their respective run sub-directories 

              e.g., -i raw [contains raw/run1 and raw/run2]

              N.B.: sample name is extracted from .fastq.gz samples  before the first '_' e.g., XXX-XX 

              sample names must be unambigious and unique e.g., sample-1 = sample-11, sample-01 != sample-10

	--raw_file_pattern=CHARACTER
		Patterns for R1 and R2 for raw files

	-a CHARACTER, --atropos_binary=CHARACTER
		Path of atropos program [used for primer removal]

	--cut_dir=CHARACTER
		Directory for atropos PCR primer removed reads

	--cut_pattern=CHARACTER
		Patterns for R1 and R2 of atropos files

	--filt_dir=CHARACTER
		Directory for filtered reads

	--merged_run_dir=CHARACTER
		Directory for run merging, chimera removal

	--taxa_dir=CHARACTER
		Directory for taxonomy step

	--sep=CHARACTER
		regex pattern to identify sample names  [default: after first _]

	--preset=CHARACTER
		Will use default primers and parameters as defined in the run_dada2_pipeline() [primers, trunc, maxee, overlap, expected length, ...]

	--PRIMER_F=CHARACTER
		Sequence of the gene specific Fwd primer to be removed with atropos [if using -V V4 or V3V4, this parameter is already set]

	--PRIMER_R=CHARACTER
		Sequence of the gene specific Rev primer to be removed with atropos [if using -V V4 or V3V4, this parameter is already set]

	--tax_threshold=CHARACTER
		Threshold for taxonomic assignments [the minimum bootstrap confidence for assigning a taxonomic level.

	--nbases=CHARACTER
		Number of bases for error learning step

	--pool=CHARACTER
		Pooling strategy

	--minover=CHARACTER
		Minimum overlap for merginf R1 and R2 reads [if using -V V4 or V3V4, this parameter is already set]

	--trunclen=CHARACTER
		Nucleotide position to truncate the Fwd and Rev reads at [if using -V V4 or V3V4, this parameter is already set]

	--trim_length=CHARACTER
		ASV of length outside the range will be discarded [i.e., insilco size exclusion of ASV - if using -V V3 or V3V4, this parameter is already set]

	--maxee=CHARACTER
		Maximum expected error for Fwd and Rev reads [if using -V V4 or V3V4, this parameter is already set]

	--minLen=CHARACTER
		Minimul read length [if using -V V3 or V3V4, this parameter is already set]

	--chimera_method=CHARACTER
		 Chimera removal strategy

	--collapseNoMis=CHARACTER
		 Perform 100% similarity ASV clustering?

	--tryRC=CHARACTER
		Perform taxonomic assignments also on reversed complemented ASV sequence?

	--metadata=CHARACTER
		Path to excel document containing metadata [Sample identifier column should be sample_name]

	--db=CHARACTER
		Path to the taxonomic database

	--db_species=CHARACTER
		Path to the speies-level taxonomic database [only for --tax_metod  dada]

	--run_phylo=CHARACTER
		Compute phylogenetic tree from the ASV sequence ?

	--merge_phyloseq_export=CHARACTER
		Path fof the phyloseq object

	--output_phyloseq_phylo=CHARACTER
		Path fof the phyloseq object

	--save_out=CHARACTER
		Path fof the R object containing all output from the pipeline

	--export=CHARACTER
		Export pdf, table and intermediate RDS files?

	--remove_input_fastq=CHARACTER
		Remove intermediate fastq.gz files

	-T NUMERIC, --slots=NUMERIC
		Number of threads to perform the analyses

	--seed_value=NUMERIC
		Seed value for random number generator

	-f CHARACTER, --fun_dir=CHARACTER
		Directory containing the R functions

	-h, --help
		Show this help message and exit

```









### for additional functionalities (i.e., post ASV clustering using vsearch/lulu & picrust2 functional potential estimation - please install the following tools:
### R packages - similarly as done before:

	devtools::install_github("tobiasgf/lulu");devtools::install_github("ycphs/openxlsx");devtools::install_github("mikemc/speedyseq")
### vsearch in the dedicated conda environment:
	(metabaRpipe)$ conda install -c bioconda vsearch -y
	
### picrust2 in the dedicated conda environment:
	(metabaRpipe)$ conda install picrust2 -y


## Clone the repository:

This step is required to get the Rscripts locally.

Change the directory where you would like to clone the repository.

	$ MY_DIR=/path_to_mydir/whereIwant/metabaRpipe/to/be/installed/
	$ cd ${MY_DIR}

Use ``git clone`` to clone on your computer the repository including the functions and test data.

	$ git clone https://github.com/fconstancias/metabaRpipe.git



## Run the pipeline:

activate the dedicated conda environment:

	$ conda activate metabaRpipe


Use ``Rscript`` to run the pipeline and specify some necessary parameters e.g.: *databases* 

- ``dada`` method: <https://benjjneb.github.io/dada2/training.html>


```bash
(metabaRpipe)$ Rscript scripts/dada2_metabarcoding_pipeline.Rscript \
-i test-data/ \
--preset V3V4 \
-T 8 \
--db ~/db/DADA2/silva_nr99_v138_train_set.fa.gz \
--db_species ~/db/DADA2/silva_species_assignment_v138.fa.gz \
--metadata test-data/metadata.xlsx \
--run_phylo FALSE \
--save_out test_pipe_Rscript.RDS \
-f https://raw.githubusercontent.com/fconstancias/metabaRpipe/Rscripts/functions.R > mylogs.txt 2>&1

```
		
The ``> mylogs.txt 2>&1`` trick will redirect what is printed on the screen to a file including potential errors and also parameters that you used.

## Add phylogenetic tree to a phyloseq object:

activate the dedicated conda environment:

	$ conda activate metabaRpipe

By default, based on <https://f1000research.com/articles/5-1492>. It might take quite some time depending on your configuration and the overall richness of your phyloseq object, that's why it is not computed by default running the pipeline.

	(metabaRpipe)$ Rscript scripts/add_phylogeny_to_phyloseq.R \
		-p dada2/physeq.RDS \
		-o dada2/physeq_phylo 


## Add/replace taxonomical information fron a phyloseq object:
### using dada2 assigntaxa/ assignspecies:

#### using Rscript
```bash
(metabaRpipe)$ Rscript scripts/run_phyloseq_dada2_tax.Rscript \
--phyloseq_path rscript-output/03_dada2_merged_runs_chimera_removed/physeq.rds \
--tax_threshold 60 \
--output rscript-output/04_dada2_taxonomy \
--db ~/db/DADA2/silva_nr99_v138_train_set.fa.gz \
--db_species ~/db/DADA2/silva_species_assignment_v138.fa.gz \
--reverse_comp TRUE \
-T 4 \
-f scripts/functions_export_simplified.R
```
#### within R
```r
source("https://raw.githubusercontent.com/fconstancias/metabaRpipe/Rscripts/functions.R")

readRDS("/Users/physeq.RDS") %>%
  phyloseq_dada2_tax(physeq = ., 
                     threshold = 60, # 60 (very high),  50 (high), PB = 10
                     db ="~/db/DADA2/silva_nr99_v138_train_set.fa.gz"
                     db_species ="~/db/DADA2/silva_species_assignment_v138.fa.gz"
                     nthreads = 2,
                     tryRC = TRUE,
                     return = TRUE,
                     full_return = FALSE) -> physeq_new_tax

```
                     
### using DECIPHER IDtaxa::

You can also add or replace taxonomical information of your phyloseq object using DECIPHER IDtaxa function.


```bash
Rscript scripts/run_phyloseq_DECIPHER_tax.Rscript \
--phyloseq_path rscript-output/03_dada2_merged_runs_chimera_removed/physeq.rds \
--export rscript-output/04_dada2_taxonomy \
--reverse_comp TRUE \
--db ~/db/DADA2/SILVA_SSU_r132_March2018.RData \
--tax_threshold 50 \
-T 8 \
-f scripts/functions_export_simplified.R
```
#### within R

```r
source("https://raw.githubusercontent.com/fconstancias/metabaRpipe/Rscripts/functions.R")


readRDS("/Users/physeq.RDS") %>%
  phyloseq_DECIPHER_tax(physeq = ., 
                        threshold = 60, # 60 (very high),  50 (high), PB = 10
                        db="~/db/DADA2/SILVA_SSU_r132_March2018.RData" 
                        nthreads = 6,
                        tryRC = TRUE,
                        return = TRUE) -> physeq_new_tax
```

## Add/update metadata a phyloseq object:


```r
source("https://raw.githubusercontent.com/fconstancias/metabaRpipe/Rscripts/functions.R")

ps_tax_phylo %>%
  physeq_add_metadata(physeq = .,
                      metadata = "test-data/metadata.xlsx" %>%
  readxl::read_xlsx(),
                      sample_column = "sample_name") -> ps_tax_phylo_meta

ps_tax_phylo_meta
```


## TODO:

- <s>add phylogenetic tree to a phyloseq object</s>
- change name
- add https://zenodo.org/account/settings/github/ -> DOI
- Faster method for phylogenetic reconstruction (e.g., MAFFT + FastTree)
- add possibility to skip primer removal: skipping run_atropos() or changing atropos parameter?
- <s>replace taxonomic assignments of a phyloseq object using alternative approach/ database</s>
- <s>cluster ASV using DECIPHER</s>
- <s>cluster ASV using vsearch lulu</s>
- <s>run picrust2 from a phyloseq object</s>

## Help:


activate the dedicated conda environment:

	$ conda activate metabarcodingRpipeline

print help:
	
	(metabaRpipe)$ Rscript scripts/dada2_metabarcoding_pipeline.R --help

	Usage: scripts/dada2_metabarcoding_pipeline.R [options]


	Options:
	
	-i CHARACTER, --input_directory=CHARACTER
		Path of the input directory containing raw _R1_ and _R2_ raw reads in their respective run sub-directories 

       e.g., -i raw [contains raw/run1 and raw/run2]

       N.B.: sample name is extracted from .fastq.gz samples  before the first '_' e.g., XXX-XX 

       sample names must be unambigious and unique e.g., sample-1 = sample-11, sample-01 != sample-10

	-a CHARACTER, --atropos_binary=CHARACTER
		Path of atropos program [used for primer removal]

	-o CHARACTER, --output_directory=CHARACTER
		Name of the output directory

	-V CHARACTER, --pipeline=CHARACTER
		V4 or V3V4 will use default primers and parameters as used in the FBT lab [primers, trunc, maxee, overlap, expected length, ...]

	-t CHARACTER, --tax_method=CHARACTER
		User can specify using dada2 (=dada) or DECIPHER for taxonomic assignments [default dada]

	--tax_threshold=NUMERIC
		Thershold for taxonomic assignments [if --tax_metod dada: he minimum bootstrap confidence for assigning a taxonomic level. if --tax_method DECIPHER: Numeric specifying the confidence at which to truncate the output taxonomic classifications. ]

	--metadata=CHARACTER
		Path to excel document containing metadata [Sample identifier column should be sample_name]

	--database=CHARACTER
		Path to the taxonomic database

	--database_for_species_assignments=CHARACTER
		Path to the speies-level taxonomic database [only for --tax_metod  dada]

	--phylo=CHARACTER
		Compute phylogenetic tree from the ASV sequence ?

	--PRIMER_F=CHARACTER
		Sequence of the gene specific Fwd primer to be removed with atropos [if using -V V4 or V3V4, this parameter is already set]

	--PRIMER_R=CHARACTER
		Sequence of the gene specific Rev primer to be removed with atropos [if using -V V4 or V3V4, this parameter is already set]

	--minover=NUMERIC
		Minimum overlap for merginf R1 and R2 reads [if using -V V4 or V3V4, this parameter is already set]

	--trunclen=NUMERIC
		Nucleotide position to truncate the Fwd and Rev reads at [if using -V V4 or V3V4, this parameter is already set]

	--trim_length=NUMERIC
		ASV of length outside the range will be discarded [i.e., insilco size exclusion of ASV - if using -V V3 or V3V4, this parameter is already set]

	--maxee=NUMERIC
		Maximum expected error for Fwd and Rev reads [if using -V V4 or V3V4, this parameter is already set]

	--minLen=NUMERIC
		Minimul read length [if using -V V3 or V3V4, this parameter is already set]

	-T NUMERIC, --slots=NUMERIC
		Number of threads to perform the analyses

	-h, --help
		Show this help message and exit

