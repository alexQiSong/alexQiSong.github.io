---
permalink: /using-r-with-python/
title: "Using R with Python"
layout: single
author_profile: true
---
Oftentimes, I have to use some R packages within my Python pipeline because there are no Python alternatives. Calling suprocess module and execute R script in Shell is a nice solution but I thought this can be done more elegantly. A simple Google search led me to the introduction page of [rpy2](https://rpy2.readthedocs.io/en/version_2.8.x/).

Calling R functions directly inside Python may sound like some fancy tricks but rpy2 just made it as simple as possible! One cool feature is that you can even use Pandas DataFrame to directly interact with R functions. As I am heavily dependent on Pandas DataFrame to perform routine analysis, this nice feature does make a lot sense for me :-)!. 

For my case, I need to use bioconductor package 'ChIPseeker' in my Python pipeline. I finally ended up writing the following code to run ChIPseeker in my Python script. This script uses ChIPseeker to identify the nearest genes for the given genomic regions which are stored in a Pandas DataFrame `peakTab`. `gffFileName` is the GFF format genome annotation passed from some other functions.

```python
from rpy2.robjects import pandas2ri
from pandas import read_csv,DataFrame
from rpy2.robjects.packages import importr # Loading R library, equivalent to library() in R

# import R libraries
base = importr('base')
GF = importr('GenomicFeatures')
GR = importr('GenomicRanges')
BG = importr("BiocGenerics")
IR = importr('IRanges')
S4V = importr('S4Vectors')
CSK = importr('ChIPseeker')
meth = importr('methods')

# Enable interaction between Pandas DataFrame and R functions
pandas2ri.activate() 

# Create txdb object from GFF file
txdb = GF.makeTxDbFromGFF(gffFileName)

'''
Find the nearest genes for all genomic regions stored in peakTab.
chrStart is the start position and chrEnd is the end position.
Note that peakTab is a Pandas DataFrame but you can directly use
this in R function GR.GRanges
'''
peaks = GR.GRanges(
      seqnames = S4V.Rle(peakTab['chr']),
      ranges = IR.IRanges(peakTab['chrStart'],peakTab['chrEnd']),
      strand = S4V.Rle(BG.strand(base.gsub('\\.','*',peakTab['strand']))),
      #weight = peakTab['weight'],
      TFID = peakTab['TFID']
    )
    
annoPeakTab = pandas2ri.ri2py(base.as_data_frame(meth.slot(CSK.annotatePeak(peaks,TxDb = txdb),"anno")))
index = (annoPeakTab['distanceToTSS'] > -3000) & (annoPeakTab['distanceToTSS'] <= 500)
re = annoPeakTab.loc[index,('TFID','geneId')]
```
One thing I still could not figure out is how to suppress the standard output from R function. The standard output generated from R function seems ugly in Python, I will think of a way to get rid of it.   
