# multivariantviewer

A JBrowse plugin that adds some custom glyphs for variants on a "multi VCF" file (VCF with multiple samples)

## Options


### Tracktype
 
* MultiVariantViewer/View/Track/Grid - displays all genotypes for features
* MultiVariantViewer/View/Track/LDTrack - displays ld triangle on the genome browser
* MultiVariantViewer/View/Track/Matrix - displays all genotypes for features in matrix form

### Style options

* style->height - Pixel height for each sample. Default: 5
* style->offset - Pixel offset between each sample. Default: 0
* style->matrixColor - A color or a callback that returns colors for both the grid type and the matrix popup. Default: cyan is heterozygous non-ref, grey homozygous ref, blue is homozygous non-ref. Can be customized by a callback with the a function signature `function(feature, type, genotype)` where `type` is either 'ref' or 'alt' and `genotype` is the actual genotype as 0|0 or 0|1 or similar and `feature` contains all info about a particular variant

### Subtrack label options

* showLabels - Display subtrack labels (boolean)
* showTooltips - Display mouseover tooltips with subtrack name and description (boolean)
* labelFont - Specify subtrack label font CSS e.g. "6px sans-serif"
* labelWidth - Specify a specific width for all subtrack labels. Default autosizes to each sublabel's length, which can look ugly
* sublabels - An array of structures like {"name": "sample1", "color": "red", "description": "Optional description or sample displayed on mouseover", "population": "CEU"}
* sortByPopulation - Sorts genotypes by population

The sublabels are optional and default to just showing the sample names if not specified

### LD viewing options

* ldviewer - URL for the linkage_server service. Default http://localhost:4730/
* maf - Set minor allele frequency cutoff. Default 0.01

## Example configuration

In tracks.conf format

    [tracks.variant]
    urlTemplate=file.vcf.gz
    storeClass=JBrowse/Store/SeqFeature/VCFTabix
    type=MultiVariantViewer/View/Track/Grid
    showLabels=true
    labelFont=4px sans-serif
    style.height=10
    sublabels+=json:{"name": "sample1", "color": "blue", "description": "mouseover description"}
    sublabels+=json:{"name": "sample2", "color": "red", "description": "mouseover description"}

In trackList.json format

    {
        "type": "MultiVariantViewer/View/Track/Grid",
        "urlTemplate": "variants.vcf.gz",
        "label": "Variant track",
        "storeClass": "JBrowse/Store/SeqFeature/VCFTabix",
        "style": {
            "height": 1,
            "color": "function(feat, gt) { return gt == 'ref'? 'blue': 'orange'; }"
        }
    }

## Sample

See test subdirectory for example, you can use http://localhost/jbrowse/?data=plugins/MultiVariantViewer/test/data to see the sample data



## Screenshot

![](img/example.png)

Shows 1000genomes VCF data, tooltips on sidebar

![](img/matrix.png)

Displays matrix of variants


![](img/ld.png)

Shows LD


## Install plugin

Clone the repo to your plugins directory and name it MultiVariantViewer

    git clone https://github.com/cmdcolin/multivariantviewer MultiVariantViewer

Then add it to your jbrowse config

    "plugins": ["MultiVariantViewer"]
    
See http://gmod.org/wiki/JBrowse_FAQ#How_do_I_install_a_plugin for more details

## linkage_server

Optionally, LD can be calculated from the VCF files on the server side using plink and rendered as a classic block view.

### Pre-requisites linkage_server

* tabix
* plink2 aka plink 1.9

### Install linkage_server

    npm install

### Run linkage_server

    node linkage_server/index.js

You might also find it useful to use a node.js taskrunner like forever or pm2

