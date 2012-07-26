<!--roptions dev='png', fig.width=7, fig.height=7, tidy=FALSE, cache=TRUE, echo=TRUE, message=FALSE, warning=FALSE, autodep=TRUE, cache.path='/tmp/knitr-cache/' -->

<!--begin.rcode setup, echo=FALSE, cache=FALSE

# use imgur for hosting figures
# go to static/my_imgur_api_key.txt and add your own api key

upload_images = TRUE

if (upload_images) {
   #opts_knit$set(upload.fun = imgur_upload)
   opts_knit$set(upload.fun = function (file){
      sprintf("http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/%s", file)    
    })
} else {
    opts_chunk$set(fig.path='figure/')
}

# use html style links to plots, rather than markdown style         
knit_hooks$set(plot = hook_plot_html)
build_dep()

require(knitcitations)
cleanbib()
# to get knitcitations:
#library(devtools)
#install_github("knitcitations", "cboettig")
 
end.rcode-->

See the [end of this document](#about-this-doc) for information about generating this document and how to generate it from source with knitr.

<!--begin.rcode knitCitationsBibtexExperiment, echo=FALSE, results="asis", cache=FALSE
  # set up citations
biblio <- read.bibtex("citation11k.bib")
end.rcode-->

<!--begin.rcode workspace, messages=FALSE, echo=FALSE
# Clear the workspace and load package dependencies: 
rm(list=ls())   
require(ggplot2, quietly=T)
require(scales, quietly=T)
require(gplots, quietly=T)
require(reshape2, quietly=T)
require(plyr, quietly=T)
require(rms, quietly=T)
require(polycor, quietly=T)
require(ascii, quietly=T)
require(knitcitations)
# to get knitcitations:
#library(devtools)

options(scipen=999)

source("helpers.R")
source("preprocess_raw_data.R")

end.rcode-->

<!--begin.rcode gfmtable, echo=FALSE, cache=FALSE
# From https://gist.github.com/2050761
gfm_table <- function(x, ...){
  y <- capture.output(print(ascii(x, ...), type = 'org'))
  # substitute + with | for table markup
  # TODO: modify regex so that only + signs in markup, like -+- are substituted
  y <- gsub('[+]', '|', y)
  return(writeLines(y))
} 
#gfm_table(anova(fit))
end.rcode-->

<!--begin.rcode calcCI, echo=FALSE

###### ANALYSIS
  
# Some helper functions
calcCI.exp= function(res, param) {
  coefs = summary(res)$coeff
  coeff = coefs[param,]
  x = coeff[1]
  stderr = coeff[2]
  p = coeff[4]
  return(data.frame(param = param,
              est = round(exp(x), 2), 
              ciLow = round(exp(x - 1.96*stderr), 2),
              ciHigh = round(exp(x + 1.96*stderr), 2), 
              p = round(p, 3)))
}

calcCI.noexp= function(res, param) {
  coefs = summary(res)$coeff
  coeff = coefs[param,]
  x = coeff[1]
  stderr = coeff[2]
  p = coeff[4]
  return(data.frame(param = param,
              est = round(x, 2), 
              ciLow = round(x - 1.96*stderr, 2),
              ciHigh = round(x + 1.96*stderr, 2), 
              p = round(p, 3)))
}
end.rcode-->

<!--begin.rcode colours, echo=FALSE
#colourblind friendly palettes from http://wiki.stdout.org/rcookbook/Graphs/Colors%20(ggplot2)
cbgRaw = c("#E69F00", "#56B4E9", "#009E73", "#999999", "#F0E442", "#0072B2", "#D55E00", "#CC79A7", "#000000")
cbgFillPalette <- scale_fill_manual(values=cbgRaw)
cbgColourPalette <- scale_colour_manual(values=cbgRaw)
cbgColorPalette = cbgColourPalette

end.rcode-->


# Data Reuse and the Open Data Citation Advantage
*Piwowar, Carlson, Vision*


## Goal
1. Is there an association between data availability and citation rate, independently of important known citation predictors?
1. Is there supporting evidence that additional citations are due to data reuse?
1. What is the timeline of data reuse?

## Abstract

### Background
Attribution upon reuse of scientific data is important to reward data creators and to document the provenance of research findings.  Previous studies have found that papers with publicly available datasets receive a higher number of citations than similar studies without available data.  However, previous analyses did not control for many variables known to predict citation rate.  In this analysis we look at citation rates while controlling for many known citation predictors, and investigate patterns of data reuse.

### Methods and Results
In a multivariate regression on 10555 studies that created gene expression microarray data, we found that studies with data in centralized public repositories received 9% (95% confidence interval: [5% to 13%]) more citations than similar studies without available data.  Date of publication, journal impact factor, open access status, number of authors, first and last author publication history, corresponding author country, institution citation mean score, and study topic were included as covariates.  The open data citation benefit varied with date of dataset deposition: a citation boost was most clear for papers published in 2004 and 2005, at about 30%.  Data-depositing authors published almost all their papers associated with their dataset within two years, whereas papers published by third-party investigators began to ramp up after two years and continued to accumulate rapidly past year six.  The level of third-party data use was high: for 100 datasets deposited in year 0, we estimate that 40 papers in PubMed reused a dataset by year 2, 100 by year 4, and more than 150 by year 5. Recent reuse analyses include more third-party datasets, on average, than early reuse studies: a quarter of reuse studies in 2010 used at least 3 datasets. Data reuse was distributed across a broad base of datasets: a conservative estimate finds that 20% of the datasets deposited between 2003 and 2007 have been reused at least once by third-parties. Third-party reuse papers are being published at about the same rate as new datasets are made available.

### Discussion
Robust evidence of an open data citation benefit requires multivariate analysis to isolate contributing effects.  Attribution practices that directly mention the dataset facilitate study of data reuse patterns. Evidence of data reuse and citation benefit is important for documenting the value of data archiving to both scientific progress and the data-collecting investigator.


## Introduction

"Sharing information facilitates science. Publicly sharing detailed research data–sample attributes, clinical factors, patient outcomes, DNA sequences, raw mRNA microarray measurements–with other researchers allows these valuable resources to contribute far beyond their original analysis. In addition to being used to confirm original results, raw data can be used to explore related or new hypotheses, particularly when combined with other publicly available data sets. Real data is indispensable when investigating and developing study methods, analysis techniques, and software implementations. The larger scientific community also benefits: sharing data encourages multiple perspectives, helps to identify errors, discourages fraud, is useful for training new researchers, and increases efficient use of funding and patient population resources by avoiding duplicate data collection." <!--rinline citep(biblio["piwowar2007sharing"])-->

Making research data publicly available also has costs. Some of these costs are to society: For example, data archives must be created and maintained. Many costs, however, are born by the data-collecting investigators: Data must be documented, formatted, and uploaded. Investigators may be afraid that other researchers will find errors in their results, or "scoop" additional analyses they have planned for the future.

Personal incentives are important to balance these personal costs.  Scientists report that receiving additional citations is an important motivator for publicly archiving their data <!--rinline citep(biblio["tenopir2011data-sh"])-->.

Evidence suggests that studies which make their data available do indeed receive more citations than similar studies that do not make their data available <!--rinline citep(biblio[c("gleditsch2003posting", "piwowar2007sharing", "ioannidis2009repeata", "pienta2010the-end", "henneken2011linking", "sears2011data-sh", "dorch2012on-the-")]) -->.
This evidence has been <a href="http://scholar.google.com/scholar?cites=10688057049876523086&amp;as_sdt=5,39&amp;sciodt=0,39&amp;hl=en">frequently referenced</a> and highlighted in new policies that encourage and require data archiving [<a href="http://scholar.google.com/scholar?cites=10688057049876523086&amp;as_sdt=5,39&amp;sciodt=0,39&amp;hl=en">http://datadryad.org/jdap</a>], demonstrating the appetite for evidence of personal benefit.

It important to continue striving for an accurate estimate of citation correlation, to avoid over- or under-stating the benefits of data availability.

This report on open data citation advantage improves on previous work in several ways. First, the sample size is large, so we have been able to include additional variables in the statistical analyses.  The resulting estimates isolate the association between data availability and citation rate with more accuracy. Second, this report goes beyond citation analysis to include analysis of data reuse attribution directly.  We explore how data reuse patterns change over both the lifespan of a data repository and the lifespan of a dataset.

## Methods


###Relationship between data availability and citation

####Data collection

<!--begin.rcode dfAttributes, echo=FALSE
dfAttributes = read.csv("data/PLoSONE2011_rawdata.txt", sep="\t", header=TRUE, stringsAsFactors=F)
end.rcode-->

The primary analysis in this paper examines the citation count of a gene expression microarray experiment, relative to availability of the experiment's data.

The sample of microarray experiments used in the current analysis was previously determined <!--rinline citep(biblio[c("piwowar2011who-sha", "piwowar2011data-fr")])-->.  Briefly, a full-text query uncovered papers with wet-lab methods related to gene expression microarray data collection.  The full-text query was characterized with high precision (90%, 95% confidence interval 86% to 93%) and a moderate recall (56%, 52% to 61%) for this task.  Running the query in PubMed Central, HighWire Press, and Google Scholar identified <!--rinline dim(dfAttributes)[1] --> distinct gene expression microarray papers.  The papers were published between 2000 and 2009.

The independent variable of interest is the availability of gene expression microarray data.  Data availability had been previously determined for our sample articles in <!--rinline citep(biblio["piwowar2011who-sha"])-->, so we directly reused that dataset <!--rinline citep(biblio["piwowar2011data-fr"])-->.  Datasets were considered available if they were discovered in either of the two predominant gene expression microarray databases: NCBI's Gene Expression Omnibus (GEO), and EBI's ArrayExpress.

"An earlier evaluation found that querying GEO and ArrayExpress with article PubMed identifiers located a representative 77% of all associated publicly available datasets [Piwowar 2010]. [We] used the same method for finding datasets associated with published articles in this study: [we] queried GEO for links to the PubMed identifiers in the analysis sample using the “pubmed_gds [filter]” and queried ArrayExpress by searching for each PubMed identifier in a downloaded copy of the ArrayExpress database. Articles linked from a dataset in either of these two centralized repositories were considered to have [publicly available data] for the endpoint of this study, and those without such a link were considered not to have [available] data." <!--rinline citep(biblio["piwowar2011who-sha"])-->

<!--rinline citep(biblio["piwowar2011who-sha"])--> included 124 attributes for each of the gene expression microarray studies in our sample.  We used a subset of these --attributes previously shown or suspected to correlate with citation rate:

<!--begin.rcode correlation_variables, warning=FALSE, echo=FALSE
correlation_variables_string = "dataset.in.geo.or.ae
nCitedBy.log
pubmed.date.in.pubmed
pubmed.year.published
pubmed.is.bacteria
pubmed.is.plants
pubmed.is.humans
pubmed.is.cultured.cells
pubmed.is.animals
pubmed.is.cancer
pubmed.is.geo.reuse
journal.impact.factor.tr
journal.microarray.creating.count.tr
journal.num.articles.2008.tr
pubmed.is.core.clinical.journal
pubmed.is.open.access
institution.rank
institution.mean.norm.citation.score
institution.is.govnt
institution.nci
institution.stanford
institution.harvard
pubmed.is.funded.nih.intramural
pubmed.is.funded.nih
num.grants.via.nih.tr
nih.cumulative.years.tr
nih.sum.avg.dollars.tr
has.R.funding
num.authors.tr
last.author.num.prev.microarray.creations.tr
last.author.num.prev.pmc.cites.tr
last.author.female
last.author.num.prev.pubs.tr
last.author.year.first.pub.ago.tr
first.author.num.prev.microarray.creations.tr
first.author.num.prev.pmc.cites.tr
first.author.female
first.author.num.prev.pubs.tr
first.author.year.first.pub.ago.tr
country.usa
country.uk
country.japan
country.china
country.korea
country.canada
country.australia"
correlation_variables = strsplit(correlation_variables_string, "\n")[[1]]

end.rcode-->

* date of publication
* journal
* journal impact factor (2008)
* journal open access status
* size of the journal
* number of authors
* years since first publication by the first and last author
* number of papers published by first and last author
* number of PubMed Central citations received by first and last author
* country of corresponding author
* institution of corresponding author
* institution rank and citation score
* study topic (human/animal/plant study, cancer/not cancer)
* NIH funding of the study, if applicable


This study required citation counts for thousands of articles, based on  PubMed ID. At the time of data collection, neither Thomson Reuter's Web of Science nor Google Scholar supported looking up number of citations by PubMed ID. This type of query was (and is) supported by Elsevier's Scopus citation database. Alas, none of our affiliated institutions subscribed to Scopus. Scopus does not offer individual subscriptions. A personal email to a Scopus Product Manager went unanswered.

One author (HAP) attempted to use the British Library's walk-in access of Scopus on its Reading Room computers during a trip overseas. Unfortunately, the British Library did not permit any method of electronic transfer of our PubMed identifier list onto the Reading Room computers, including internet document access, transferring a text file from a USB drive, or using the help desk as an intermediary (see related policies at http://www.bl.uk/reshelp/inrrooms/stp/cond/conditions.html). The Library was not willing to permit an exception in this case, and we were unwilling to manually type ten thousand PubMed identifiers into the Scopus search box in the Reading Room.
HAP eventually obtained Scopus access through a Research Worker agreement with Canada's National Research Library (NRC-CISTI), after being fingerprinted to obtain a police clearance certificate.

Although Scopus now has an API that would facilitate easy programmatic access to citation counts, at the time of data collection the authors were not aware of any way to retrieve Scopus data through researcher-developed computer programs.  We queried and exported Scopus citation data manually through interaction with the Scopus website. The Scopus website had a limit to the length of query and the number of citations that could be exported at once. To work within these restrictions we concatenated up to 500 PubMed IDs at a time into 22 queries, where each query took the form "PMID(1234) OR PMID(5678) OR ..."

<!--begin.rcode dfCitations, echo=FALSE

# Read in citations
dfCitations = read.csv("data/scopus_all.csv", header=TRUE, stringsAsFactors=F)

# Merge with attributes
dfCitationsAttributesRaw = merge(dfAttributes, dfCitations, by.x="pmid", by.y="PubMed.ID")

# Keep those published after 2000 and before 2010
dfCitationsAttributesRaw = subset(dfCitationsAttributesRaw, dfCitationsAttributesRaw$pubmed_year_published > 2000)
dfCitationsAttributesRaw = subset(dfCitationsAttributesRaw, dfCitationsAttributesRaw$pubmed_year_published < 2010)

# Turn citations read in to numbers; zeros were represented as blanks
dfCitationsAttributesRaw$nCitedBy = as.numeric(dfCitationsAttributesRaw$Cited.by)
dfCitationsAttributesRaw[which(is.na(dfCitationsAttributesRaw$nCitedBy)),]$nCitedBy=0

end.rcode-->

Citation counts for <!--rinline dim(dfCitationsAttributesRaw)[1] --> papers were exported from Scopus in November 2011. 


####Primary analysis

The analysis included several multivariate linear regressions to evaluate the association between the public availability of a study's microarray data and the number of citations (after log transformation) it received. 

We began with a simple correlations between number of citations and other variables, using Pearson correlations for numberic variables and polyserial correlations for binary and factor covariates.  We also calculated correlations between data availability and other variables to investigate collinearity. 

<!--begin.rcode preprocessing, warning=FALSE, echo=FALSE
# Preprocess attributes as per Piwowar 2011
dfCitationsAttributes = preprocess.raw.data(dfCitationsAttributesRaw)
options(scipen=999)
dfCitationsAttributes = merge(dfCitationsAttributes, dfCitationsAttributesRaw[,c("pmid", "pubmed_journal")], by="pmid", )
end.rcode-->

<!--begin.rcode univariate_correlations, warning=FALSE, echo=FALSE

myhetcorr = hetcor.modified(dfCitationsAttributes[,correlation_variables], use="pairwise.complete.obs", std.err=FALSE, pd=FALSE)
mycorr = myhetcorr$correlations
colnames(mycorr) = colnames(myhetcorr$correlations)    
rownames(mycorr) = rownames(myhetcorr$correlations)    

# Correlations with citation
correlations_with_citations = sort(mycorr[,"nCitedBy.log"], dec=T)

# Correlations with data availability
correlations_with_data_avail = sort(mycorr[,"dataset.in.geo.or.ae"], dec=T)

end.rcode-->

The main analysis was run across all papers in the sample with many  covariates known or found (in the correlation analysis above) to correlate with citation rate.  Covariates included the date of publication, the journal which published the study, the journal impact factor, the journal citation halflife, the number of articles published by the journal, the journal's open access policy, whether the journal is considered a core clinical journal by MEDLINE, the number of authors of the study, the country of the corresponding author, the citation score of the institution of the corresponding author, the publishing experience of the first and last author, and the subject of the study itself.  

Publishing experience was characterized by the number of years since the author's first paper in PubMed, the number of papers they have published, and the number of citations they have received in PubMed Central, estimated using Authority clusters as described in [Piwowar 2011].  The subject of the study was characterized by whether the paper was classified as cancer, animals, or plants.  For more information on study attributes see <!--rinline citep(biblio["piwowar2011who-sha"])-->.

Citation count was log transformed to be consistent with prior literature.  Other count variables square-root transformed.  Continuous variables were represented with 3-part spines in the regression, using the rcs function in the R rms library.

The independent variable of interest was represented as a 0 or 1 in the regression, describing whether or not associated data had been found in the data repositories.  The relationship of the data availability variable to  citation count was described with 95% confidence intervals after raising the regression coefficient to the power of e (since the log of the number of citations was used in the regression).

<!--begin.rcode regressionAll, echo=FALSE
fit_w_journal = lm(nCitedBy.log ~ 
          rcs(journal.impact.factor.tr, 3) +               
          rcs(pubmed.date.in.pubmed, 3) +
          rcs(journal.num.articles.2008.tr, 3) +           
          rcs(journal.cited.halflife, 3) +                 
          factor(pubmed.is.open.access) +              
          rcs(num.authors.tr, 3) + 
          rcs(first.author.num.prev.pubs.tr, 3) +           
          rcs(first.author.num.prev.pmc.cites.tr, 3) +     
          rcs(first.author.year.first.pub.ago.tr, 3) +     
          rcs(last.author.num.prev.pubs.tr, 3) +           
          rcs(last.author.num.prev.pmc.cites.tr, 3) +      
          rcs(last.author.year.first.pub.ago.tr, 3) +
          rcs(institution.mean.norm.citation.score, 3) +
          factor(country.usa) +              
          factor(country.china) +              
          factor(country.korea) +              
          factor(pubmed.is.cancer) +
          factor(pubmed.is.animals) +
          factor(pubmed.is.plants) +
          factor(pubmed.is.core.clinical.journal) +
          factor(pubmed_journal) +          
          factor(dataset.in.geo.or.ae)
           , dfCitationsAttributes)
end.rcode-->

Because publication date is such a strong correlate with both citation rate and data availability, we performed another analysis which stratified the sample by publication year, in addition to including publication date as a covariate.  Because the yearly regressions included fewer datapoints than the full regression, they supported a smaller number of covarites.  The yearly regressions included  date of publication, the journal which published the study, the journal impact factor, the journal's open access policy, the number of authors of the study, the citation score of the institution of the corresponding author, the previous number of PubMed Central citations recieved by the first and last author, whether the study was on cancer, and whether it used animals.

<!--begin.rcode regressionByYear, echo=FALSE

# has a few less covariates than full model
do_analysis = function(mydat) {
  myfit = lm(nCitedBy.log ~ 
      rcs(pubmed.date.in.pubmed, 3) +
      rcs(journal.impact.factor.tr, 3) + 
      rcs(journal.num.articles.2008.tr, 3) +           
      rcs(num.authors.tr, 3) + 
      rcs(first.author.num.prev.pmc.cites.tr, 3) +     
      rcs(last.author.num.prev.pmc.cites.tr, 3) +      
      rcs(institution.mean.norm.citation.score, 3) +
      factor(pubmed.is.cancer) +
      factor(pubmed.is.animals) +
      factor(dataset.in.geo.or.ae)
             , mydat)
  calcCI.exp(myfit, "factor(dataset.in.geo.or.ae).L")
}

estimates_by_year = data.frame()
for (year in seq(2001, 2009)) {
  dat.subset.year = subset(dfCitationsAttributes, pubmed.year.published==year)
  results = do_analysis(dat.subset.year)
  estimates_by_year = rbind(estimates_by_year, cbind(year=year, results))
}

end.rcode-->





#### Comparison to Piwowar et al 2007

We ran two modified analyses to attempt to reproduce the findings of <!--rinline citep(biblio["piwowar2007sharing"])-->.  First, we used a subset with roughly the same inclusion criteria as <!--rinline citep(biblio["piwowar2007sharing"])--> -- studies on cancer, with humans, published prior to 2003 -- and the same regression coefficients: publication date, impact factor, and whether the corresponding author's address is in the USA.

<!--begin.rcode RegressionAlaPrevStudy, echo=FALSE
  dat.subset.previous.study = subset(dfCitationsAttributes, (pubmed.year.published<2003) & (pubmed.is.cancer==1) & (pubmed.is.humans==1))

  myfitprev = lm(nCitedBy.log ~ 
      rcs(pubmed.date.in.pubmed, 3) +
      rcs(journal.impact.factor.tr, 3) +               
      factor(country.usa) +              
      factor(dataset.in.geo.or.ae)
               , dat.subset.previous.study)
end.rcode-->

We followed that with a second regression that included several additional important covariates:  number of authors and number of previous citations by the last author.

<!--begin.rcode RegressionAlaPrevStudyMoreCovariates, echo=FALSE
  myfit_prev_more = lm(nCitedBy.log ~ 
      rcs(pubmed.date.in.pubmed, 3) +
      rcs(journal.impact.factor.tr, 3) +               
      rcs(num.authors.tr, 3) + 
      rcs(last.author.num.prev.pmc.cites.tr, 3) +      
      factor(country.usa) +              
      factor(dataset.in.geo.or.ae)
             , dat.subset.previous.study)
end.rcode-->

####Manual review of citation context

We manually reviewed the citation context of some of these papers to determine how many of the citations were in the context of data reuse.

We randomly selected 100 datasets deposited in GEO in 2005.  For each dataset, we located the data collection article within ISI Web of Science based on its title and authors, and exported the list of all articles that cited this data collection article. This list of all citations was processed to subselect  50 random  citations, stratified by  the totalnumber of times the data collection article had been cited.

Manual review was performed for each instance of potential data reuse.  We located the article full text, read the relevant sections of the papers, and manually determined if the data from the associated dataset had been reused within the study. <!--rinline citep(biblio["piwowar2011beginni"])-->

<!--begin.rcode citationContextData, echo=FALSE
dfTracking1k = read.csv("data/tracking1k_20111008.csv", sep=",", header=TRUE, stringsAsFactors=F)
dfTracking1k.GEO.subset = subset(dfTracking1k, TAG.source=="WoS" & TAG.confidence!="low confidence" & is.na(duplicates & TAG.repository=="GEO" & (TAG.dataset.reused=="dataset reused" | TAG.dataset.reused=="dataset not reused")))

num.GEO.total = dim(dfTracking1k.GEO.subset)[1]
num.GEO.reused = dim(subset(dfTracking1k.GEO.subset, TAG.dataset.reused=="dataset reused"))[1]
annotated.prop = binconf(num.GEO.reused, num.GEO.total)
end.rcode-->

###Data reuse patterns from accession number attribution

####Data collection

We collected a separate dataset to study reuse patterns because identifying data reuse from citations requires time-consuming classification of citation context.

Datasets are sometimes attributed directly, through mention of the dataset identifier or accession number in the full-text of a research paper.  Third-party dataset resuse can be estimated from these mentions by excluding papers that were authored by members of the dataset collection team.

We used the NCBI eUtils library and custom Python code to obtain a list of all datasets deposited into the Gene Expression Omnibus data repository, then searched PubMed Central for each of these dataset identifiers (using queries of the form "'GSEnnnn' OR 'GSE nnnn'"). For each hit we recorded the PubMed Central ID of the paper that mentioned the accession number, the year of paper publication, and the author surnames.  We also recorded the dataset accession number, the year of dataset publication, and the investigator names associated with the dataset record.

####Statistical analysis

We excluded papers with author surnames in common with those authors who deposited the original dataset, as described in  <!--rinline citep(biblio["piwowar2011beginni"])-->.  

PubMed Central contains only a subset of papers recorded in PubMed. As described in  <!--rinline citep(biblio["piwowar2011beginni"])-->, to extrapolate from the number of data reuses in PubMed Central to all possible data reuses in PubMed, we divided the yearly number of hits by the ratio of papers in PMC to papers in PubMed for this domain (domain was measured as the number of articles indexed with the MeSH term “gene expression profiling”).

<!--begin.rcode accessionRatios, echo=FALSE

dfPubmedPmcRatios = read.csv("data/pubmed_pmc_ratios.csv", header=TRUE, stringsAsFactors=F)

# get a more exact ratio
dfPubmedPmcRatios$pmc_pmid_ratio = dfPubmedPmcRatios$num_pmc/dfPubmedPmcRatios$num_pubmed
dfPubmedPmcRatios$year = as.numeric(dfPubmedPmcRatios$year)

cbind(with(dfPubmedPmcRatios, paste(year, ": ", round(pmc_pmid_ratio*100, 0), "%", sep="")))
end.rcode-->

We retained reuse candidates for papers published between 2001 and 2010: 2011 was within 12 months of data collection, and had a dramatically lower proportion of papers in PubMed Central because the NIH archiving requirements permit a 12 month embargo.

To understand our findings on a per-dataset basis, we stratified reuse estimates by year of dataset submission and normalized our reuse findings by the number of datasets deposited that year.

<!--begin.rcode dfPubmedGseCount, echo=FALSE

dfPubmedGseCount = read.csv("data/pubmed_gse_count.csv", header=TRUE, stringsAsFactors=F)

subset(dfPubmedGseCount, year<2011)

end.rcode-->

<!--begin.rcode dfMentions, echo=FALSE

header_string = "accession,gse,gds,submit_pmids,reuse_pmcid,reuse_pmids_for_pmc,this_submit_authors,this_reuse_authors,intersect,submit_affiliation,release_date,sep1,bioloink_filter,basic_reuse_filter,creation_filter,oa_excerpts,word_filters,sep2,reuse_affiliation,journal,year,date_published,medline_status,is_geo_reuse,reuse_is_oa,metaanal,mesh_filters,blank,setname"
header = strsplit(header_string, ",")[[1]]

# cat records_20*.csv > all_records.csv
dfMentions = read.csv("data/all_records.csv", header=FALSE, stringsAsFactors=F)
names(dfMentions) = header

dfMentions$noAuthorOverlap = ((dfMentions$intersect) == "[]")
dfMentions$thirdPartyReuse = dfMentions$noAuthorOverlap
dfMentions$dataSubmissionYear = as.numeric(substr(dfMentions$setname, 10, 13))
dfMentions$paperPublishedYear = dfMentions$year
dfMentions$elapsedYears = dfMentions$paperPublishedYear  - dfMentions$dataSubmissionYear 
dfMentions = subset(dfMentions, select=c(thirdPartyReuse, dataSubmissionYear, paperPublishedYear, elapsedYears, gse, reuse_pmcid))

dfMentions = merge(dfMentions, dfPubmedPmcRatios[,c("pmc_pmid_ratio", "year")], by.x="paperPublishedYear", by.y="year", )
dfMentions = merge(dfMentions, dfPubmedGseCount, by.x="dataSubmissionYear", by.y="year", )

# has to be long enough ago that papers are in PMC
dfMentions = subset(dfMentions, paperPublishedYear < 2011)

end.rcode-->

<!--begin.rcode dfMentions_calcs, echo=FALSE

dfCountReusePapers = ddply(dfMentions, .(elapsedYears, thirdPartyReuse, dataSubmissionYear, pmc_pmid_ratio), summarise, count=length(elapsedYears))
dfCountReusePapers$extrap = dfCountReusePapers$count / dfCountReusePapers$pmc_pmid_ratio
dfCountReusePapers = merge(dfCountReusePapers, dfPubmedGseCount, by.x="dataSubmissionYear", by.y="year")

dfCountReusePapersThirdParty = ddply(subset(dfCountReusePapers, thirdPartyReuse==TRUE), c("dataSubmissionYear", "elapsedYears"), summarise, total = sum(extrap))
dfCountReusePapersThirdParty= merge(dfCountReusePapersThirdParty, dfPubmedGseCount, by.x="dataSubmissionYear", by.y="year")

dfCountReusePapersThirdPartyCumulative = ddply(dfCountReusePapersThirdParty[with(dfCountReusePapersThirdParty, order(elapsedYears)),], c("dataSubmissionYear"), transform, NT = cumsum(total))

total_extrap_reuse_papers = floor(sum(subset(dfCountReusePapers, thirdPartyReuse==TRUE, extrap)))

end.rcode-->

### Data and script availability

Statistical analyses were last run on <!--rinline date() --> with <!--rinline version$version.string -->.  Packages used include reshape2 <!--rinline citep(list(citation("reshape2"))) -->, plyr <!--rinline citep(list(citation("plyr"))) -->, rms <!--rinline citep(list(citation("rms"))) -->, polycor <!--rinline citep(list(citation("polycor"))) -->, ascii <!--rinline citep(list(citation("ascii"))) -->, ggplot2 <!--rinline citep(list(citation("ggplot2"))) -->, gplots <!--rinline citep(list(citation("gplots"))) -->, knitr <!--rinline citep(list(citation("knitr"))) -->, and knitcitations <!--rinline citep(list(citation("knitcitations"))) -->. P-values are two-tailed.

Raw data and statistical scripts are available in the Dryad data repository at [url and citation to be determined and included upon article acceptance].  Data collection scripts are at [GitHub pypub.  Heather, push changes!]

The Markdown version of this manuscript with interleaved statistical scripts <!--rinline citep(list(citation("knitr"))) --> is also at Dryad and in GitHub [https://github.com/hpiwowar/citation11k](https://github.com/hpiwowar/citation11k).

## Results

#### Description of cohort

We identified <!--rinline dim(dfCitationsAttributes)[1] --> articles published between 2001 and 2009 as collecting gene expression microarray data.  

<!--begin.rcode sortedjournals, echo=FALSE
sorted_journals = sort(table(dfCitationsAttributesRaw$pubmed_journal)/nrow(dfCitationsAttributesRaw), dec=T)[1:12]
end.rcode-->

The papers were published in <!--rinline length(table(dfCitationsAttributesRaw$pubmed_journal)) --> journals, with the top 12 journals accounting for <!--rinline 100*round(sum(sorted_journals), 2)-->% of the papers.

<!--begin.rcode table1, echo=FALSE
gfm_table(cbind(names(sorted_journals), round(sorted_journals, 2)))
prop2001 = 100*round(nrow(subset(dfCitationsAttributesRaw, pubmed_year_published=="2001"))/nrow(dfCitationsAttributesRaw)[1], 2)
prop2009 = 100*round(nrow(subset(dfCitationsAttributesRaw, pubmed_year_published=="2009"))/nrow(dfCitationsAttributesRaw)[1], 2)
end.rcode-->
*Table 1: Proportion of sample published in most common journals*

More microarray papers were published in later years: <!--rinline prop2001-->% of articles in our sample were published in 2001, compared to <!--rinline prop2009--> % in 2009.

<!--begin.rcode table2, echo=FALSE
gfm_table(table(dfCitationsAttributesRaw$pubmed_year_published)/nrow(dfCitationsAttributesRaw))
end.rcode-->
*Table 2: Proportion of sample published each year*

The papers were cited between <!--rinline min(dfCitationsAttributes$nCitedBy)--> and <!--rinline max(dfCitationsAttributes$nCitedBy)--> times, with an average of <!--rinline round(mean(dfCitationsAttributes$nCitedBy), 0)--> citations per paper and a median of <!--rinline median(dfCitationsAttributes$nCitedBy)--> citations.

The GEO and ArrayExpress repositories had links to associated datasets for <!--rinline 100*summary(dfCitationsAttributes$dataset.in.geo.or.ae.int)["Mean"]-->% of these papers.

### Data availability is associated with citation boost

Without accounting for any confounding factors, the mean number of citations were the same for papers with and without archived data, and the distribution of citations across the paper subsets were similar.

We hasten to mention strong confounders.  The number of citations a paper has recieved is strongly correlated to the date it was published: older papers have had more time to accumulate citations.  Publication date is also strongly correlated with the probability of data archiving -- more recent articles are more likely to archive data <!--rinline citep(biblio["piwowar2011who-sha"])-->.  

After stratifying by publication date, the distributions of citations were noticibly higher for paper with available data than those without, as seen in Figure 1.

<!--begin.rcode figure1, echo=FALSE 

citation_breaks = c(0, 10, 100, 1000, 3000)
ggplot(dfCitationsAttributes, aes(1+nCitedBy, fill=factor(dataset.in.geo.or.ae)), color="black") + 
geom_density(alpha=0.2) + 
scale_fill_manual(name="",
                    breaks=c("0", "1"),
                    labels=c("data NOT available", "data available"), 
                    values=cbgRaw) + 
facet_grid(pubmed.year.published~.) + 
scale_y_continuous(breaks=c(0, 0.5), labels=c(0, 0.5)) + 
scale_x_log10(name="\nnumber of citations", breaks=citation_breaks+1, labels=citation_breaks) + 
#cbgFillPalette + 
cbgColourPalette + theme_bw(base_size=16)

end.rcode-->
*Figure 1: Citation density for papers with and without available microarray data, by year of study publication*

Other variables have been shown to correlate with citation rate <!--rinline citep(biblio["fu2008models-"])-->. Because single-variable correlations can be misleading, we performed multivariate regression to isolate the relationship between data availability and citation rate from confounders.

<!--begin.rcode display_regressionAll, echo=FALSE

#gfm_table(anova(fit_w_journal))
citation.boost.coefs.journal = calcCI.exp(fit_w_journal, "factor(dataset.in.geo.or.ae).L")
#print(citation.boost.coefs.journal)

end.rcode-->

The multivariate regression included attributes to represent an article's journal, journal impact factor, date of publication, number of authors, number of previous citations of the fist and last author, number of previous publications of the last author, whether the paper was about animals or plants, and whether the data was made publicly available.  Citations were <!--rinline 100*(citation.boost.coefs.journal$est-1) -->%
higher for papers with available data, independent of other variables (95% confidence intervals [<!--rinline 100*(citation.boost.coefs.journal$ciLow-1) -->%
, <!--rinline 100*(citation.boost.coefs.journal$ciHigh-1) -->% ], p < 0.01).

An analysis on a subset of manually-curated articles led to similar findings as the whole sample, verifying that inclusion of a few articles mistakenly identified as creating microarry data had at most a small influence on the estimate (see supplementary materials).

### More covariates led to a more conservative estimate

Our estimate of citation boost, <!--rinline 100*(citation.boost.coefs.journal$est-1) -->% as per the multivariate regression, is notably smaller than the 69% (95% confidence intervals of 18 to 143%) citation advantage found by <!--rinline citep(biblio["piwowar2007sharing"])-->, even though both studies looked at publicly available gene expression microarray data. There are several possible reasons for this difference.

First, <!--rinline citep(biblio["piwowar2007sharing"])--> concentrated on datasets from high-impact studies: human cancer microarray trials published in the early years of microarray analysis (between 1999 and 2003), whereas the current study included gene expression microarray data studies on any subject published between 2001 and 2009. Second, because the <!--rinline citep(biblio["piwowar2007sharing"])--> sample was small, the previous analysis included only a few covariates: publication date, journal impact factor, and country of the corresponding author.

We attempted to reproduce the prior analysis with the current data points to better understand the reasons for the disparate estimates.  Limiting the inclusion criteria for the current sample to papers published between 2001 and 2003, with MeSH terms "human" and "cancer", reduced the cohort to 308 papers.  Running this subsample with the covariates used in the <!--rinline citep(biblio["piwowar2007sharing"])--> paper (date of publication, impact factor, corresponding author country) resulted in a comperable estimate to the 2007 paper: a citation increase of 47% (95% confidence intervals of 6% to 103%) when data is made available.

<!--begin.rcode display_RegressionAlaPrevStudy, echo=FALSE, eval=FALSE
  gfm_table(anova(myfitprev))
  calcCI.exp(myfitprev, "factor(dataset.in.geo.or.ae).L")
end.rcode-->

The subsample of 308 papers was large enough to include a few additional covariates: number of authors and citation history of the last author.  Including these important variables decreased the estimated effect of data archiving to 18% with a confidence interval that spanned a *loss* of 17% citations to a boost of 66%.

<!--begin.rcode display_RegressionAlaPrevStudyMoreCovariates, echo=FALSE, eval=FALSE
  gfm_table(anova(myfit_prev_more))
  calcCI.exp(myfit_prev_more, "factor(dataset.in.geo.or.ae).L")
end.rcode-->

### Data reuse is a demonstrable component of citation boost

To provide evidence on the proportion of the citation boost that may be caused by data reuse, we randomly selected and manually reviewed  138 citations.  We identified <!--rinline num.GEO.reused --> of the citations (<!--rinline 100*(round(annotated.prop[1], 2)) -->%, 95% CI: <!--rinline 100*(round(annotated.prop[2], 2)) -->% to 
<!--rinline 100*(round(annotated.prop[3], 2)) -->% ) were in the context of attribution for data reuse.


### Citation boost varies with time

Because publication date is such as strong correlate with both citation rate and data availability, we also ran regressions for each publication year individually.  The estimate of citation boost  varied by year of publication.  Papers published recently had little evidence of citation boost for making data available.  The estimates for papers published in the early 2000s had wide confidence intervals, due to the relatively small number of publications.  As seen in Table 3 and Figure 2, a citation boost was most clear for papers published in 2004 and 2005, estimated at about 30%.

<!--begin.rcode table3, echo=FALSE
with(estimates_by_year, cbind(year, est, ciLow, ciHigh))
end.rcode-->

<!--begin.rcode figure2, echo=FALSE
ggplot(estimates_by_year, aes(x=year, y=est)) + geom_line() + 
  geom_errorbar(width=.1, aes(ymin=ciLow, ymax=ciHigh)) +
  scale_x_continuous(name="", breaks=seq(2001, 2009)) +
  scale_y_continuous(name='change in citation count when data is publicly available\n', limits=c(.5, 2.5), breaks=seq(0.5, 2.5, .5), labels=c("-50%", "0", "+50%", "+100%", "+150%")) + 
  theme_bw(base_size=16) +
  geom_hline(color="grey50", linetype="dashed", aes(yintercept=1))
end.rcode-->
*Figure 2: Change in citation count when data is publicly available.  Estimates from multivariate analysis, lines indicate 95% confidence intervals of coefficient estimates.*


### Third-party data reuse grows at same rate as data archive

A complementary dataset was collected and analyzed to characterize data reuse: direct mention of dataset accession numbers in the full text of papers.

GEO contains <!--rinline sum(subset(dfPubmedGseCount, year<2011, num_gse_ids))--> datasets deposited between 2000 and 2010.

In total there were <!--rinline dim(dfMentions)[1]--> mentions of GEO datasets in papers published between 2000 and 2010 within PubMed Central across <!--rinline round(sum(subset(dfCountReusePapers, thirdPartyReuse==TRUE, count)), 0) -->  papers written by third-party authors (author teams whose last names do not overlap with those who deposited the data).  Extrapolating these findings from PubMed Central to all of PubMed, we estimate there may be about <!--rinline total_extrap_reuse_papers--> third-party papers based on reuse of GEO data in papers published between 2000 and 2010, where the data reuse attribution is made through direct mention of a dataset accession number (rather than, for example, citing an associated paper).

The number of datasets and the number of reuse papers have been growing rapidly, and are now growing at about the same rate.

<!--begin.rcode growthOfReusePapers, echo=FALSE
dfCountUnique3rdpartyPapers = ddply(subset(dfMentions, thirdPartyReuse==TRUE), .(paperPublishedYear, pmc_pmid_ratio), summarise, count=length(unique(reuse_pmcid)))

dfCountUnique3rdpartyPapers$extrap = with(dfCountUnique3rdpartyPapers, count/pmc_pmid_ratio)

dfCountUnique3rdpartyPapers = ddply(dfCountUnique3rdpartyPapers, .(), transform, cumul_extrap=cumsum(extrap))

dfCountUnique3rdpartyPapers = merge(dfCountUnique3rdpartyPapers, dfPubmedGseCount, by.x="paperPublishedYear", by.y="year")

dfCountUnique3rdpartyPapers = ddply(dfCountUnique3rdpartyPapers, .(), transform, cumul_gse=cumsum(num_gse_ids))
end.rcode-->

<!--begin.rcode figure3, echo=FALSE
# log
ggplot(data=dfCountUnique3rdpartyPapers, aes(x=paperPublishedYear, y=cumul_gse)) + geom_point() + geom_line(aes(color="datasets")) + 
scale_x_continuous(name="\nyear of publication", limits=c(2001, 2010)) +
scale_y_continuous(name="Cumulative count (log scale)\n", trans="log", breaks=log_breaks()) +
scale_colour_manual(name="", values=cbgRaw) +
theme_bw(base_size=16) +
geom_line(aes(y=cumul_extrap, color="third-party reuse papers")) + geom_point(aes(y=cumul_extrap)) 
end.rcode-->
*Figure 3: Cumulative number of datasets deposited in GEO each year, and cumulative number of third-party reuse papers published that directly attribute GEO data published each year, log scale.*

### Data reuse picks up as primary author use winds down

We found that almost all papers by the author team that archived a given dataset (identified by surname overlap with data submission record) were published within two years of dataset publication.  This pattern contrasts sharply with data reuse by outside authors, as seen in Figure 4.  

<!--begin.rcode figure4, echo=FALSE
ggplot(dfCountReusePapers, aes(x=elapsedYears, y=extrap, color=thirdPartyReuse)) + geom_line() + 
scale_x_continuous(name="\nyears since data publication", limits=c(0, 8)) +
scale_y_continuous(name="Number of papers\n", labels=comma_format()) +
facet_wrap(~dataSubmissionYear) +
scale_colour_manual(name="",
                    values=cbgRaw,
                    breaks=c(FALSE, TRUE),
                    labels=c("orig authors", "third-party authors")) +
theme_bw(base_size=16)
end.rcode-->
*Figure 4: Number of papers mentioning GEO accession numbers.  Each panel represents reuse of a particular year of dataset submissions, with number of mentions on the y axis, years since the intial publication on the x axis, and a line for reuses by the data collection team and a line for third-party investigators*

The cumulative number of third-party reuse papers is illustrated in Figure 5.  Separate lines are displayed for different dataset submission years.

<!--begin.rcode figure5, echo=FALSE
# cumulative
ggplot(data=subset(dfCountReusePapersThirdPartyCumulative, dataSubmissionYear>2000), aes(x=dataSubmissionYear+elapsedYears, y=NT, group=dataSubmissionYear, color=factor(dataSubmissionYear))) + geom_point() + geom_line() + 
scale_x_continuous(name="\npublication year of reuse paper", limits=c(2001, 2010)) +
scale_y_continuous(name="Cumulative number of papers\n", labels=comma_format()) +
scale_color_hue(name="year of data publication") +
theme_bw(base_size=16)
end.rcode-->
*Figure 5: Cumuative number of third-party reuse papers, by date of reuse paper publication. Separate lines are displayed for different dataset submission years*

Early years of data submission, 2001-2002, had relatively few data deposits.  Relatively, their levels of data reuse are very high.

We exclude the early years from the next plot to examine the pattern of data reuse once gene expression datasets became more common.  Figure 6 shows cumulative third-party reuse, normalized by number of datasets deposited each year, plotted as elapsed years since data publication.

<!--begin.rcode figure6, echo=FALSE
ggplot(data=subset(dfCountReusePapersThirdPartyCumulative, dataSubmissionYear>2002), aes(x=elapsedYears, y=NT/num_gse_ids, group=dataSubmissionYear, color=factor(dataSubmissionYear))) + geom_point() + geom_line() + 
scale_x_continuous(name="\nyears since data publication", limits=c(0, 8)) +
scale_y_continuous(name="Cumulative number of papers\nnormalized by number of datasets deposited in given year\n", labels=comma_format()) +
scale_colour_manual(name="year of data publication", values=cbgRaw) +
theme_bw(base_size=16)
end.rcode-->
*Figure 6: Cumulative third-party reuse, normalized by number of datasets deposited each year, plotted as elapsed years since data publication*

### Number of datasets in a reuse paper has increased

The number of datasets used in a reuse paper was found to increase over time. By 2010, 25% of reuse papers used 3 or more datasets. 

<!--begin.rcode figure7, echo=FALSE

dfGsePerReusePaper = ddply(subset(dfMentions, thirdPartyReuse==TRUE), .(reuse_pmcid, paperPublishedYear), summarise, count=length(unique(gse)))

breaks = c(1,5,10,50)
ggplot(data=dfGsePerReusePaper, aes(x=factor(paperPublishedYear), y=count)) + 
  geom_jitter(position = position_jitter(w = 0.1, h = 0.1), color=cbgRaw[1]) +
  scale_x_discrete("\nYear reuse paper was published") +
  scale_y_continuous(name="Number of datasets per reuse paper\n", trans="log", breaks=breaks) +  
  stat_summary(fun.y=mean, geom=c("line"), color=cbgRaw[2], aes(group=1)) +
  cbgColourPalette + theme_bw(base_size=16)

end.rcode-->
*Figure 7: Scatterplot of year of publication of third-party reuse paper (with jitter) vs number of GEO datasets mentioned in the paper (log scale).  The line connects the mean number of datasets attributed in reuse papers vs publication year.*

### Reuse is distributed across many datasets

Reuse was not limited to just a few datasets. All datasets published in 2001 have been reused at least once and half were reused by at least three papers.  Newer datasets have been used less often, but we observed reuse of 20% of the datasets deposited between 2003 and 2007.  The actual rate, across all methods of attribution and extrapolated to all of PubMed, is likely much higher. 

<!--begin.rcode figure8, echo=FALSE

dfCountReuseByGse = ddply(subset(dfMentions, thirdPartyReuse==TRUE), .(gse, dataSubmissionYear), summarise, count=length(elapsedYears))

for (year in seq(2001, 2009)) {
    num_with_reuse = sum(with(subset(dfCountReuseByGse, dataSubmissionYear==year), table(count)))
    num_total = dfPubmedGseCount[which(dfPubmedGseCount$year==year), "num_gse_ids"]
    if (num_with_reuse < num_total) {
      placeholder_zero_accessions = paste("FAKE", seq(num_with_reuse+1, num_total), sep="")
      for (accession in placeholder_zero_accessions) {
          dfCountReuseByGse = rbind(dfCountReuseByGse, data.frame(gse=accession, dataSubmissionYear=year, count=0))  
      }
    }
}

q = ddply(dfCountReuseByGse, "dataSubmissionYear", summarise, quantile=seq(0,1,0.01), count=quantile(count, seq(0,1,0.01)))

qq = subset(q, count %in% c(1, 3))

qqq = ddply(qq, c("dataSubmissionYear", "count"), summarise, maxq=min(quantile))

ggplot(data=qqq, aes(x=dataSubmissionYear, y=1-maxq, color=factor(count, labels=c("reused at least once", "reused at least three times")))) + geom_line() + geom_point() + 
  scale_x_continuous("\nYear data was submitted") +
  scale_y_continuous(name="Proportion of data submissions\n") + 
scale_color_hue(name="") + theme_bw(base_size=16)

end.rcode-->
*Figure 8: Proportion of data reused by third-party papers vs year of data submission.  Conservative estimate, because only considers reuse by papers in PubMed Central, and only when reuse is attributed through direct mention of a GEO accession number*

### Data reused most often 3-6 years after publication

Data reuse papers are most likely to use data that is 3-6 years old by the time their paper is published, normalized for how many datasets were deposited each year.

<!--begin.rcode figure9, echo=FALSE

# distribution of elapsed years per publication date
dfDatasetsByElapsed = ddply(subset(dfMentions, (dataSubmissionYear>2002) & (thirdPartyReuse==TRUE)), .( elapsedYears, paperPublishedYear, num_gse_ids), summarise, count=length(gse))

  ggplot(data=subset(dfDatasetsByElapsed, paperPublishedYear>2004), aes(x=elapsedYears, y=count/num_gse_ids)) + geom_line() +
    scale_x_continuous(name="\nyears since data publication", limits=c(0,9)) +
    scale_y_continuous(name="Proportion of all datasets published in given year\n") +    
    facet_wrap(~paperPublishedYear) + 
    cbgColourPalette +
    theme_bw(base_size=16) 

end.rcode-->
*Figure 9: Each panel includes a cohort of data reuse papers published in a given year.  Line plots proportion of datasets submitted in previous years that were reused by reuse papers in the panel year.*  


## Discussion

Studies with publicly available datasets received more citations than similar studies without available datasets, even after controlling for many variables known to influence citation rate.  We found the open data citation boost for this sample to be <!--rinline 100*(citation.boost.coefs.journal$est-1) -->% overall
(95% confidence interval: <!--rinline 100*(citation.boost.coefs.journal$ciLow-1) -->%
to <!--rinline 100*(citation.boost.coefs.journal$ciHigh-1) -->%).  The specific boost depended heavily on the year the dataset was made available.  Datasets deposited very recently received no (or few) additional citations, while those deposited in 2004-2005 showed a clear boost of about 30% (confidence intervals 15% to 48%).  Older datasets also appeared to receive a citation boost, but the estimate has less precision because relatively little microarray data was collected or archived in the early 2000s.

### previous estimates

Estimates calculated in this study are lower than those reported by previous studies.  The most similar study, <!--rinline citep(biblio["piwowar2007sharing"])-->, found a citation boost of 69% (95% confidence intervals of 18 to 143%) for human cancer gene expression microarray studies published before 2003.  The high estimate of <!--rinline citep(biblio["piwowar2007sharing"])--> could be due to its specific inclusion criteria; perhaps clinically relevant datasets, released early in the history of microarray analysis, were particularly impactful. Secondary analysis in the current study, attempting to replicate <!--rinline citep(biblio["piwowar2007sharing"])--> suggests this may be true.  Importantly, however, the new  analysis also suggested that the previous estimate was confounded by significant citation correlates, including number of authors, citation history of last author.  This finding reinforces the importance of accounting for covariates, multivariate analysis, and the need for large samples to support full analysis: the 69% estimate is probably too high, even for its high-impact sample.

So how can we interpret a citation boost of 10 to 30%?  Is it a large enough to motivate data-collecting investigators to archive their data?  Future research is needed to understand author views on the trade-off between citation advantage (and other data archiving benefits) and perceived archiving costs.  For journals, a 10-30% citation boost is likely very motivating, given that journals currently fight for impact factor scores to two decimal places.  What about funders?  How is this citation boost related to more efficient and effective science?

### data reuse

A clear case can be made that data reuse contributes to a stellar scientific ROI <!--rinline citep(biblio["piwowar2011data-ar"])-->, and at least part of an open data citation boost likely comes from attribution for data reuse.  To verify that some of the data collection papers in this study were cited in the context of data reuse, we manually reviewed some of the citations: <!--rinline 100*(round(annotated.prop[1], 2)) -->%
(95% CI: <!--rinline 100*(round(annotated.prop[2], 2)) -->%
to <!--rinline 100*(round(annotated.prop[3], 2)) -->%) were in the context of data reuse.  

Understanding data reuse patterns required a larger sample could easily be assembled through manual classification of citation context, so we collected a second dataset.  Our explorations of data reuse patterns leveraged an alternate practice for data reuse attribution: direct mention of a dataset's identifier (traditionally an accession number) within the body of a full-text research article.  

We found the data collection team published almost all of its papers within two years of publishing the paper directly associated with the dataset.  In contrast, data reuse papers by third-party authors continued to accumulate 6 years after the data was made publicly available.  The level of third-party data use was high: for 100 datasets deposited in year 0, we estimate that 40 papers in PubMed reused a dataset by year 2, 100 by year 4, and more than 150 by year 5.  This data reuse curve held remarkably constant for data deposited between 2004 and 2009.  Microarray datasets made available in 2001 and 2002 were reused much more often -- every single dataset deposited in 2001 has been reused in a third-party paper.  This is probably because few datasets were available.  The reuse growth trend for data deposited in 2003 has been slower, perhaps because 2003 data is not as ground-breaking as earlier data, and is likely less standards-compliant and technically relevant than later data.

Third-party reuse papers are being published at about the same rate as new datasets are made available.  Recent reuse analyses use more datasets, on average, than older reuse studies. A quarter of reuse studies in 2010 used at least 3 datasets. Published analyses are most likely to include data published between 3 and 6 years ago, normalized for how many datasets were deposited each year.  

These results suggest that the lower citation boost we observed for recent papers is due, at least in part, to a relatively short followup time.

Analysis of the accession number mentions revealed that data reuse was driven by a broad base of datasets: more than 20% of the datasets deposited between 2003 and 2007 have been reused by third parties.  We note these proportions are gross underestimates since they only include reuses we observed as accession number mentions in PubMed Central; no attempt has been made to extrapolate these distribution statistics to all of PubMed, or to reflect attributions through citations.  Further, many important instances of data reuse do not leave a trace in the published literature, such as those in education and training. Nonetheless, even these conservative estimates suggest that third-party investigators find value in a wide range of datasets, not simply a "very reusable" elite.

Future work can improve on these results by considering and integrating all methods of data use attribution.  This holistic effort would include identifying citations to the paper that describes the data collection, mentions of the dataset identifier itself -- whether in full text, the references section, or supplementary information -- citations to the dataset as a first-class research object, and even mentions of the data collection investigators in acknowledgement sections.  The citations and mentions would need classification based on context to ensure they are in the context of data reuse.  Also, the method we use for determining third-party reuse based on author last name overlap with the data submission record is extremely crude.

Data from current and future studies can start to be used to estimate the impact of policy decisions.  For example, do embargo periods decrease the level of data reuse?  Do restrictive or poorly articulated licencing terms decrease data reuse?  Which types of data reuse are facilitate by robust data standards and which types are unaffected?

### other sources of citations

While an open data citation boost is likely largely driven by citation for data reuse, open data may directly or indirectly inspire citations for other reasons as well.  The literature on "Open Access citation benefit" articulates several possible sources of OA citation boost, including Selection Bias and Early View  <!--rinline citep(biblio["craig2007do-open"])-->.  Inspired by this work, we suggest several possible sources for an "Open Data citation benefit":

1. *Data Reuse*. Papers with available datasets can be used in more ways than papers without data, and may therefore receive additional attributions.
1. *Credibility Signalling*. The credibility of research findings may be higher for research papers with available data. Such papers may be preferentially chosen as background citations and/or the foundation of additional research.
1. *Increased Visibility*. Citing authors may be more likely to encounter a research project with available data. More artifacts associated with a research project gives the project a larger footprint, increasing the likelihood that someone finds an aspect of the research. Links from data to the research paper may also increase the search ranking of the research paper.
1. *Early View*. When data is made available before a paper is published, some citations may accrue earlier than otherwise because research methods and findings are encountered prior to paper publication.
1. *Selection Bias*. Authors may be more likely to publish data for papers they judge to be their best quality work, because they are particularly proud or confident in the results <!--rinline citep(biblio["wicherts2011willing"])-->. Alternatively, author self-selection bias may have a negative correlation with research quality in the case of Open Data: authors may be less willing to archive data for their most important and visible research in order to maintain a competitive edge. 

Importantly, almost all of these mechanisms are aligned with more efficient and effective scientific progress: increased data use, facilitated credibility determination, earlier access, improved discoverability, and a focus on best work through data availability are good for both investigators and the science community as a whole.  Working through the one area where incentives between scientific good and author incentives conflict, finding weaknesses or faults in published research, may require mandates.  Or, instead, perhaps the research community will quickly learn to associate withheld data with poor quality research.

The citation boost in the current study is consistent with observed data reuse alone, but it is possible some of the other sources postulated above may have contributed citations for the studies with available data.  Further work will be needed to understand the relative contributions from each source.  For example, in-depth analyses of all publications from a set of data-collecting authors could support measurement of selection bias.  Observing search behaviour of researchers, and the returned search hit results, could characterize increased visibility due to data availability.  Hypothetical examples could be provided to authors to determine whether they would be systematically more likely to cite a paper with available data in situations where they are considering the credibility of research findings.

### tools, practices, and research

The obstacles encountered in executing this study and associated work demonstrate that improvements in tools and practice are needed to make impact tracking easier and more accurate, for day-to-day analysis as well as studies for evidence-based policy.  Researchers and research tools need libre access to the full-text of the research literature to support sophisticated text mining.  Citation databases are key building blocks of discovery: researchers need access to the services, and also to the code that support the services so they are empowered to add missing features.  Practices need improvement too.  Currently data attribution practices are nonstandard, which inevitably leads to confusion and undercounting. 

Citations are blind to many important types of data reuse.  The impact of data on practitioners, educators, data journalists, and industry researchers are not captured by attibution patterns in the scientific literature.  Altmetrics indicators uncover discussions in social social media, syallabi, patents, and theses: analyzing such indicators for datasets would provide valuable evidence of reuse beyond the scientific literature.  As evaluators move away from assessing research based on journal impact factor and toward article-level metrics, post-publication metrics rates will become icreasingly important indicators of research impact.  

It is important to remember that the primary rationale for making research data broadly available has nothing to do with evaluation metrics: full description of experimental process and findings is a tenant of science and [publicly-funded science is a public resource](http://www.nature.com/news/open-your-minds-and-share-your-results-1.10895).  Nonetheless, robust evidence of personal benefit will help as science transitions from "data not shown" to a culture that simply expects data to be part of the published record.



## Acknowledgements

- CISTI for Scopus access
- British Library helpers
- Angus, Jonathan, Estephanie
- my funding, Jonathan + Estephanie’s funding

## Author Contributions

HAP: initial idea, study design, data collection, analysis, initial manuscript draft
TJV: study design, substantial manuscript revisions
Both authors discussed the results and implications and commented on the manuscript at all stages.

## References

<!--begin.rcode citations, echo=FALSE, cache=FALSE, results="asis"
bibliography(sort=TRUE)
end.rcode-->  

References are available in a publicly-available [Mendeley group](http://www.mendeley.com/groups/2223913/11k-citation/papers/) 


## Supplementary material


###Validation of automated method of detecting data availability

Our method of identifying which articles create gene expression microarray data made a nontrivial number of errors: about 10% of the articles it identified as creating gene expression microarray data do not in fact create gene expression datasets [cite].

The papers that are erroniously included in our subset to not create gene expression data, so they certainly don''t have associated archived datasets: all  erroniously included papers were automatically classified in the "no archived data" group. 

If it were true that these erroniously-included articles recieved many more or many fewer citations than other articles in the group, their inclusion could influence the findings of this study.  To verify our assumption that the influence of these mistakenly-included articles is in fact small, we manually reviewed a random 226 of the 11k (get exact number) articles.  Of these manually reviewed articles, 206 did indeed create gene expression microarray data, and 20 did not (but satisfied the boolean-search query for other reasons).  

<!--begin.rcode percent
206/226
end.rcode-->

Examining the citations of the 20 articles that did not create gene expression data revealed that these studies were cited less often than those that did create data: a mean of 26 citations compared to a mean of 32 citations.  The overall distribution of citations for articles that did not create gene expression data is closer to zero than the distribution of citations for articles that did create gene expression data.


<!--begin.rcode manualAnnotationCreatedData, echo=FALSE
dfAnnotations = read.csv("data/Mendeley_annotated_250_of_11k.csv", header=TRUE, stringsAsFactors=F)
# Get subset that has been annotated
dfAnnotationsAnnotated = subset(dfAnnotations, TAG.annotated == "11k-subset-reviewed")
# Merge together annotations with citation information
dfCitationsAnnotated = merge(dfAnnotationsAnnotated, dfCitations, by.x="pmid", by.y="PubMed.ID")

# Clean the data, get variables in useful formats
dfCitationsAnnotated$isCreated = factor(dfCitationsAnnotated$TAG.created)
dfCitationsAnnotated$nCitedBy = as.numeric(dfCitationsAnnotated$Cited.by)

dat.annotated.merged = merge(dfCitationsAnnotated, dfCitationsAttributes, by="pmid")
dat.annotated.merged.created = subset(dat.annotated.merged, isCreated==levels(isCreated)[1])
end.rcode-->

We took steps to verify our assumption that the influence of articles erroniously identified these mistakenly-included articles is in fact small.  We began by manually reviewed a random 226 of the 11k (get exact number) articles to identify those which we were assuming had created gene expression microarray data but in fact had not.

We compared the distribution of those with errors to those without, calculated whether they were statitically different, and ran a regression with the known-correct sample only.

<!--begin.rcode manualAnnotationCreatedStats, echo=FALSE
ttest_citedby = with(dfCitationsAnnotated, t.test(nCitedBy~isCreated))
ttest_log_citedby = with(dfCitationsAnnotated, t.test(log(1+nCitedBy)~isCreated))
wilcox_citedby = with(dfCitationsAnnotated, wilcox.test(nCitedBy~isCreated))
end.rcode-->


<!--begin.rcode manualAnnotationCreatedRegression, echo=FALSE
annotated_merged_created = lm(nCitedBy.log ~ 
  rcs(pubmed.date.in.pubmed, 3) +
  rcs(journal.impact.factor.tr, 3) +               
  rcs(num.authors.tr, 3) + 
  rcs(last.author.num.prev.pmc.cites.tr, 3) +      
  factor(country.usa) +              
  factor(dataset.in.geo.or.ae)
             , dat.annotated.merged.created)
end.rcode-->


<!--begin.rcode manualAnnotationCreatedCitations
with(dfCitationsAnnotated, summary(nCitedBy~isCreated))
end.rcode-->


<!--begin.rcode display_manualAnnotationCreatedCitations, echo=FALSE, eval=FALSE
citation_breaks = c(0, 10, 100, 1000, 3000)
ggplot(dfCitationsAnnotated, aes(1+nCitedBy, fill=factor(isCreated))) + geom_density(alpha=0.2) + 
scale_fill_manual(name="",
                    values=cbgRaw, 
                    breaks=c("created-microarray-data", "created-microarray-data-not"),
                    labels=c("did collect microarray data", "did NOT collect microarray data")) + 
scale_x_log10(name="\nnumber of citations", breaks=citation_breaks+1, labels=citation_breaks) + 
#cbgFillPalette + 
cbgColourPalette + theme_bw(base_size=16)
end.rcode-->

This difference, however, was found to be not statisitically significantly different at the p<0.05 level, using either a t-test on the log of the citation counts or a Wilcoxon rank sum test on the raw citation counts.

<!--begin.rcode display_manualAnnotationCreatedStats
print(ttest_citedby)
print(ttest_log_citedby)
print(wilcox_citedby)
end.rcode-->

To confirm that the erroniously-included articles were not driving the findings about the citation relationship with data availability, we ran a multivariate regression analysis on the subsample of 206 articles that we manually determined did in fact generate gene expression microarray data.  The estimated effect is statistically significant and similar to the findings from the whole sample.

<!--begin.rcode display_manualAnnotationCreatedRegression
gfm_table(anova(annotated_merged_created))
calcCI.exp(annotated_merged_created, "factor(dataset.in.geo.or.ae).L")
end.rcode-->

# about this doc
 * author of this file: Heather Piwowar, <hpiwowar@gmail.com>
 * license: CC0
 * Acknowledgements: thanks to Yihui Xie for knitr and Carl Boettiger for his clear examples of this literate programming framework. 
 * Generated on <!--rinline date() -->

To execute the R code in this file and embed the results in the text, I start R, set the working directory, then run the following:

    library(knitr)  
    knit("stats_knit_.md")

or, from the command line, to generate an html file:

    R -e "library(knitr); knit('stats_knit_.md')"; pandoc --toc -r markdown -w html -H static/header.html stats.md > stats.html

The stats.html file can be viewed directly in a browser.
The images are stored in my Public Dropbox folder.

After pushing the .md files to GitHub, the stats.md file can also be viewed at [https://github.com/hpiwowar/citation11k/blob/master/analysis/stats.md](https://github.com/hpiwowar/citation11k/blob/master/analysis/stats.md) .

To extract the R code in a separate .R file called stats_knit_.R, run knit with tangle set to TRUE:

    R -e "library(knitr); knit('stats_knit_.md', tangle=T)"


