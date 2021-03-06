IDEA
=============
<p>    Identification of differentially expressed genes is a major application of count data such as RNA-Seq. To this end, many tools  such as <a href="http://bioconductor.org/packages/release/bioc/html/DESeq.html/">DESeq</a> (<a href="http://www.ncbi.nlm.nih.gov/pubmed/20979621/">Anders and Huber 2010</a>) (updated as <a href="http://bioconductor.org/packages/release/bioc/html/DESeq2.html/">DESeq2</a> (<a href="http://www.ncbi.nlm.nih.gov/pubmed/25516281/">Love, Huber et al. 2014</a>)), <a href="http://bioconductor.org/packages/release/bioc/html/edgeR.html/">edgeR</a> (<a href="http://www.ncbi.nlm.nih.gov/pubmed/19910308/">Robinson, McCarthy et al. 2010</a>, <a href="http://www.ncbi.nlm.nih.gov/pubmed/24753412/">Zhou, Lindsay et al. 2014</a>), <a href="http://www.bioconductor.org/packages/release/bioc/html/NOISeq.html/">NOISeq</a> (<a href="http://www.ncbi.nlm.nih.gov/pubmed/21903743/">Tarazona, García-Alcalde et al. 2011</a>), <a href="http://cran.r-project.org/web/packages/PoissonSeq/index.html/">PoissonSeq</a> (<a href="http://www.ncbi.nlm.nih.gov/pubmed/22003245/">Li, Witten et al. 2011</a>), and <a href="http://www.inside-r.org/packages/cran/samr/docs/SAMseq/">SAMseq (samr)</a> (<a href="http://www.ncbi.nlm.nih.gov/pubmed/22127579/">Li and Tibshirani 2013</a>) have been developed. However, the majority of biologists who lack of programming skills are not able to make full use of these tools since these tools provide little interaction and visualization functions that can help users run the analysis and interpret results intuitively. Moreover, the results from different analysis tools could be inconsistent. Therefore, an interactive tool that implements and combines these differential expression analysis tools is particularly helpful to those researchers with little bioinformatics knowledge.</p>
<br/>
<p>    Here, we developed a web server called  "<strong>I</strong>nteractive <strong>D</strong>ifferential <strong>E</strong>xpression <strong>A</strong>nalyzer (<strong>IDEA</strong>)" for differential gene expression analysis of count data using <a href="http://www.r-project.org/">R</a> (R Core Team 2014), <a href="http://shiny.rstudio.com/">Shiny</a> (RStudio Inc. 2014) and JavaScript. With the help of this web server, users can quickly and easily access the most often used differential expression analysis tools. IDEA is highly interactive and will provide good visualization. Additionally, a Score based on robust rank aggregation (RRA) (<a href="http://www.ncbi.nlm.nih.gov/pubmed/22247279/">Kolde, Laur et al. 2012</a>) integrates the result rank lists from different tools to refine the set of differentially expressed features.</p>
[Click here to redirect to IDEA website.](http://idea.biocuckoo.org)<br />

**Screen shoot of IDEA**
<img src="inst/IDEA/www/img/demo0-v2.png">

Installation
-------------
To run IDEA functionally in local machine, you must make sure that all the dependencies were well installed.
To check the dependencies, one can simply type 
```
library("Packages for check")
```
To date, the current version(IDEA 1.0) is work well based on the following versions of dependencies<br/>
    shiny (>= 0.12.2),<br/>
    DESeq2 (>= 1.6.2),<br/>
    shinyBS (<= 0.25),<br/>
    shinysky (>= 0.1.2),<br/>
    edgeR (>= 3.2.4),<br/>
    NOISeq (>= 2.8.0),<br/>
    samr (>= 2.0),<br/>
    PoissonSeq (>= 1.1.2),<br/>
    ggplot2 (>= 1.0.1),<br/>
    FactoMineR (>= 1.27),<br/>
    VennDiagram (>= 1.6.5),<br/>
    rmarkdown (>= 0.2.53),<br/>
    RobustRankAggreg (>= 1.1)<br/>
<br/>

User should notice that , the latest **shinyBS**  is incompatible with IDEA; you should install the former version (0.25) before IDEA was imported. This is because current shinyBS removed the ```
progressBar
``` function used in IDEA. To install the certain version shinyBS, you can type
```
devtools::install_github( "ebailey78/shinyBS")
```
We also list the command to install the related packages:
```
cDep <- c("abind")
#dependencies from BIOCONDUCTOR
bcDep <- c("Biobase", "BiocGenerics", "S4Vectors", "IRanges", "GenomeInfoDb", "GenomicRanges")
#target from CRAN
cTgt <- c("PoissonSeq","FactoMineR","samr","ggplot2","VennDiagram","RobustRankAggreg","shiny","rmarkdown")
#target from BIOCONDUCTOR
bcTgt <- c("edgeR", "DESeq2")



###INSTALLED PACKAGES
#get installed list
inst <- packageStatus()$inst

#check and install DEPENDENCIES from CRAN
for(i in 1:length(cDep)){
  tag = which(inst$Package == cDep[i])
  if(length(tag)){
    remove.packages(cDep[i])
  }
  install.packages(cDep[i])
}
#check and install DEPENDENCIES from BIOCONDUCTOR
source("http://bioconductor.org/biocLite.R")
for(i in 1:length(bcDep)){
  tag = which(inst$Package == bcDep[i])
  if(length(tag))
    remove.packages(bcDep[i])
  biocLite(bcDep[i])
}

#check and install TARGET packages
for(i in 1:length(cTgt)){
  install.packages(cTgt[i])
}
for(i in 1:length(bcTgt)){

  biocLite(bcTgt[i])
}

devtools::install_github("shiny-incubator", "rstudio")
devtools::install_github( "ebailey78/shinyBS")
```
To install the latest development builds directly from GitHub, run this:(For some reason, we did not release the current version on CRAN or BIOCONDUCTOR site):

```
if (!require("devtools"))
  install.packages("devtools")
devtools::install_github("likelet/IDEA")
```
The above command can automaticly install the several dependencies which allows users skippin some installation steps; However, the shinyBS package would be latest version which will result in incompatible with IDEA. User can remove it and reinstall the right version to avoid this;
Alternately, if you have installed dependencies manually, you can then install IDEA pakcage alone with following command :
```
if (!require("devtools"))
  install.packages("devtools")
devtools::install_github("likelet/IDEA",dependencies = FALSE))
```

Run Instruction
-------------
**For computer with GUI**<br/>
Clone repository into local path and run as a Shiny App <br/>
example:  <br/>
Clone repository into local directory '/Document/IDEA' (so that ui.R locates as '/Document/IDEA/ui.R') <br/>
then run in R under work directory of '/Document' : <br/>
      ```
      runApp("IDEA")
		```
Also, after install IDEA pakcages with no error print,  users can simply input<br/>
```
runIDEA()
``` 
function to run application locally; Since there is only one function in this package, you can also print help massege in your R console;

**For server or computer without GUI(Run IDEA on a sever)**<br/>
Install shiny-server first and finish the configurations step refers to [http://rstudio.github.io/shiny-server/latest/](http://rstudio.github.io/shiny-server/latest/)<br/>
Clone this package and install the dependencies mentioned above in your R environment; <br/>
Copy _IDEA_ folder under _inst_ dir into /srv/shiny-sever/ (or orther app location you configured in shiny-sever)<br/>
Configure the server network/firewall options like open the shiny port(3838 default), internal IP access (please refer to shiny-server configuration step);<br/>

Input the server ip address with port number and enjoy your IDEA analysis trip :).<br/>

Developing Environment
-------------
R version 3.1.2
Packages:
Shiny (0.12.2), ShinySky(0.1.2), shinyIncubator (3.2.4),shinyBS(0.25) and their dependencies <br/>
DESeq2 (1.6.2), edgeR (3.2.4), SAMseq (2.0), PoissonSeq (1.1.2), NOISeq (2.8.0) and their dependencies <br/>
ggplot2, reshape, plyr, scale, RColorBrewer and other packages needed in plotting
Documentation
-------------
First stable version released.
To view user guide, please visit [http://idea.biocuckoo.org](http://idea.biocuckoo.org) and refers to **HELP** section
Developer Information
-------------
FAQ
-------------
During the analysis procedure,  you may encounter the following question due to software or enviroment bugs; We summarized those questions and give some solution for bug fixing:

1. "Error : unrecognized fields specified in html_dependency: attachment" while clicks the download report button:
Answer: This error is mainly caused by Rstudio version; you can fixed it by upgrading your RStudio to the latest version.

### Developers:
Qi Zhao, zhaoqi3@mail2.sysu.edu.cn <br/>
Peng Nie, niepeng@mail2.sysu.edu.cn <br/>
Rucheng Diao, diaorch@mail2.sysu.edu.cn <br/>
Lichen Sun, sunlch@mail2.sysu.edu.cn <br/>
Yi Shi, shiy26@mail2.sysu.edu.cn
### Maintainer:
Qi Zhao, Peng Nie <br/>
Please feel free contact us. <br/>
