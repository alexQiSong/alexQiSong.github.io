---
title: "Using biocondutor R package in Python"
categories:
  - Programming
tags:
  - R
  - Python
  - rpy2
---
Oftentimes, I have to use some R packages within my Python pipeline because there are no alternatives in Python. Calling suprocess module and executing R script in Shell is a nice solution but this can be done more elegantly. A simple Google search led me to the introduction page of [rpy2](https://rpy2.readthedocs.io/en/version_2.8.x/).

Calling R functions directly inside Python may sound like some fancy tricks but rpy2 just made it as simple as possible! One cool feature is that you can even use Pandas DataFrame to directly interact with R functions. As I am heavily dependent on Pandas DataFrame to perform routine analysis, this nice feature does make a lot sense for me. 

For my case, I need to use bioconductor package 'ChIPseeker' in my Python pipeline. After walking through the doc, I finally ended up writing the code below to run ChIPseeker in my Python script. This script uses ChIPseeker to identify nearest genes for the given genomic regions which are stored in a Pandas DataFrame named `peakTab`. `gffFileName` is the GFF format genome annotation passed from some other functions. importr returns an instance of R library which has its own namespace hosting all the functions from that library. So you can effortlessly call any R function from any R package using `name.function()`. `name` is the alias for the R package name and `function` is the function name. 

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

'''
Enable interaction between Pandas DataFrame and R functions
You can use it just like an R data frame.
'''
pandas2ri.activate() 

# Create txdb object from GFF file
txdb = GF.makeTxDbFromGFF(gffFileName)

'''
Generate GRranges object from genomic regions stored in peakTab
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

# Find nearst genes using annotatePeak() in ChIPseeker package
annoPeakTab = pandas2ri.ri2py(base.as_data_frame(meth.slot(CSK.annotatePeak(peaks,TxDb = txdb),"anno")))

# Filter genes by distance. Only genes within 500 bps upstream and 3000 bps downstream are kept
index = (annoPeakTab['distanceToTSS'] > -3000) & (annoPeakTab['distanceToTSS'] <= 500)
re = annoPeakTab.loc[index,('TFID','geneId')]
```
One thing I am still struggling with is how to suppress the standard output from R function. The standard output generated from R function looks ugly in Python, I will think of a way to get rid of it.   
