
# Preparation
To perform PRSice, you will need
## Input
- **PRSice.R file:** A wrapper for the PRSice binary and for plotting
- **PRSice executable file:** Perform all analysis except plotting
- **Base data set:** GWAS summary results, which the PRS is based on
- **Target data set:** Raw genotype data of **target phenotype**.
Can be in the form of  [PLINK binary](https://www.cog-genomics.org/plink2/formats#bed) or [BGEN](http://www.well.ox.ac.uk/~gav/bgen_format/)

!!! Note

    You should first perform quality control on your genotype file before passing it to PRSice.
    You can do that using PLINK. A good starting point is (assume **_($target)_** is the prefix of your target binary file)

``` bash
plink --bfile ($target) \
    --maf 0.05 \
    --mind 0.1 \
    --geno 0.1 \
    --hwe 1e-6 \
    --make-just-bim \
    --make-just-fam \
    --out ($target).qc
```

Then you can add `--keep ($target).qc.fam --extract ($target).qc.bim` to PRSice command to filter out
the samples and SNPs




# Running PRSice
In most case, you can simply run PRSice using the following command, assuming your
PRSice executable is located in `($HOME)/PRSice/` and you are working in `($HOME)/PRSice`

!!! Note
    For window users, please use **Rscript.exe** instead of **Rscript**

## Binary Traits
For binary traits, you can use the following command
``` bash hl_lines="6 7"
Rscript PRSice.R --dir . \
    --prsice ./PRSice \
    --base TOY_BASE_GWAS.assoc \
    --target TOY_TARGET_DATA \
    --thread 1 \
    --stat OR \
    --binary-target T

```

## Quantitative Traits
For quantitative traits, you can use
``` bash hl_lines="6 7"
Rscript PRSice.R --dir . \
    --prsice ./PRSice \
    --base TOY_BASE_GWAS.assoc \
    --target TOY_TARGET_DATA \
    --thread 1 \
    --stat BETA \
    --binary-target F
```

!!! Note
    The default of PRSice is based on the header of your base file:
    1. When *BETA* (case insensitive) is found in the header and `--stat` was not provided, `--beta` will be added to your command, and if `--binary-target` was not provided, `--binary-target F` will be added to your command
    2. When *OR* (case insensitive) is found in the header and `--binary-target` was not provided, `--binary-target T` will be added to your command

    You can also disable this behavior by using the `--no-default` option

    Most importantly, PRSice will detail all effective option in its log file where you can simply copy and paste it to get the same output
