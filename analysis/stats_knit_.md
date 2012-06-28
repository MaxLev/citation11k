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

# knitr citation11k manuscript
 * author Heather Piwowar, <hpiwowar@gmail.com>
 * license: CC0
 * Acknowledgements: thanks to Carl Boettiger and knitr for this literate programming framework!
 * Generated on <!--rinline date() -->

To run this I start R, set the working directory to match where this file is, then run the following in R:

    library(knitr)  
    knit("stats_knit_.md")

or, from the command line

    R -e "library(knitr); knit('stats_knit_.md')"; pandoc --toc -r markdown -w html -H static/header.html stats.md > stats.html
    view in browser: file:///Users/hpiwowar/Documents/Projects/citation%20benefit%20in%2011k%20study/citation11k/analysis/stats.html

to see just the R code in a separate .R file called stats_knit_.R, run 
    R -e "library(knitr); knit('stats_knit_.md', tangle=T)"

<!--begin.rcode workspace, messages=FALSE, echo=FALSE
# Clear the workspace and load package dependencies: 
rm(list=ls())   
require(ggplot2, quietly=T)
require(gplots, quietly=T)
require(plyr, quietly=T)
require(rms, quietly=T)
require(polycor, quietly=T)
require(ascii, quietly=T)

options(scipen=8)

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
cbgRaw = c("#E69F00", "#56B4E9", "#009E73", "#999999", "#F0E442", "#0072B2", "#D55E00", "#CC79A7")
cbgFillPalette <- scale_fill_manual(values=cbgRaw)
cbgColourPalette <- scale_colour_manual(values=cbgRaw)
cbgColorPalette = cbgColourPalette

end.rcode-->


# Data Reuse and the Open Data Citation Advantage
*Piwowar, Carlson, Vision*

## Recent Changes
* updated introduction
* rearranged results, put regression before univarite analysis
* fleshed out the open data citation boost postulates in text
* discussion rearranging and requests for feedback

## Goal
1. Is there an association between data availability and citation rate, independently of important known citation predictors?
1. Is there supporting evidence that additional citations are due to data reuse?

## Abstract

See the [end of this document](#abstract-1) (at the end so it can pull in results from the R analysis).

## Introduction

"Sharing information facilitates science. Publicly sharing detailed research data–sample attributes, clinical factors, patient outcomes, DNA sequences, raw mRNA microarray measurements–with other researchers allows these valuable resources to contribute far beyond their original analysis[1]. In addition to being used to confirm original results, raw data can be used to explore related or new hypotheses, particularly when combined with other publicly available data sets. Real data is indispensable when investigating and developing study methods, analysis techniques, and software implementations. The larger scientific community also benefits: sharing data encourages multiple perspectives, helps to identify errors, discourages fraud, is useful for training new researchers, and increases efficient use of funding and patient population resources by avoiding duplicate data collection." [Piwowar, Sharing] 

Making research data publicly available also has costs.  Data archives must be created and maintained. Data must be documented, formatted, and uploaded.  Data-collecting investigators may be asked to answer questions when others try to use their data.

Scientists report that receiving more citations would be an important motivator for publicly archiving their data [Tenopir].

Several studies across several disciplines have found an association between data availability and number of citations recieved by a publication [list].  This evidence has been included in several new policies that encourage and require data archiving [cite citations?].  It is important, therefore, to continue to strive for an accurate estimate of possible citation benefit. 

The present study hopes to improve previous estimates in several ways.  First, the present study is large enough to inluce many key covariates that may have conflated estimates of citation boost in previous, smaller studies:  Number of authors, author publication experience,  institution, open access availability, and subject area.  Second, the current analysis estimates how citation boost levels may change over time.  Third, the current analysis includes evidence on the nubmer of citations that may be due to data reuse.

Clinical microarray data provides a useful environment for the investigation: despite being valuable for reuse [butte] and well-supported by data sharing standards and infrastructure [], fewer than half of the studies that collect this data make it publicly available [Ochsner, Piwowar].


## Methods

Analysis run on <!--rinline date() -->.

### Identification of relevant studies

- Piwowar HA (2011) Who shares? Who doesn’t? Factors associated with openly archiving raw research data. PLoS ONE 6(7): e18657. doi:10.1371/journal.pone.0018657
- Piwowar HA (2011) Data from: Who shares? Who doesn’t? Factors associated with openly archiving raw research data. Dryad Digital Repository. doi:10.5061/dryad.mf1sd

<!--begin.rcode dfAttributes, echo=FALSE
dfAttributes = read.csv("data/PLoSONE2011_rawdata.txt", sep="\t", header=TRUE, stringsAsFactors=F)
end.rcode-->

### Assessment of data availability

Summarize approach in Who shares? paper.

### Collection of study attributes

Summarize approach in Who shares? paper.

### Citation data

Citations from Scopus.
<!--begin.rcode dfCitations, echo=FALSE
dfCitations = read.csv("data/scopus_all.csv", header=TRUE, stringsAsFactors=F)
dfCitationsAttributesRaw = merge(dfAttributes, dfCitations, by.x="pmid", by.y="PubMed.ID")
end.rcode-->

### Statistical methods

### Data and script availability


## Results

### Primary analysis of relationship between data availability and citation

#### Description of cohort

We begin with articles that have been identified as collecting gene expression microarray data by automatic algorithms looking for keywords in article full text (Piwowar 2011).  

<!--begin.rcode dfCitationsAttributesRaw, echo=FALSE
dfCitationsAttributesRaw = subset(dfCitationsAttributesRaw, dfCitationsAttributesRaw$pubmed_year_published > 2000)
dfCitationsAttributesRaw = subset(dfCitationsAttributesRaw, dfCitationsAttributesRaw$pubmed_year_published < 2010)
#dim(dfCitationsAttributesRaw)

# Get citations into the right format

dfCitationsAttributesRaw$nCitedBy = as.numeric(dfCitationsAttributesRaw$Cited.by)
dfCitationsAttributesRaw[which(is.na(dfCitationsAttributesRaw$nCitedBy)),]$nCitedBy=0
#dim(dfCitationsAttributesRaw)
 
dfCitationsAttributes = preprocess.raw.data(dfCitationsAttributesRaw)
dim(dfCitationsAttributes)
options(scipen=8)

end.rcode-->

For this analysis of citation behaviour, we retain articles published between 2001 and 2009: <!--rinline dim(dfCitationsAttributes)[1] --> articles.

The composition of this sample is spread across XXX journals, with the top 12 journals accounting for XXX% of the papers.

<!--begin.rcode journalTable
a = sort(table(dfCitationsAttributesRaw$pubmed_journal)/nrow(dfCitationsAttributesRaw), dec=T)[1:12]
gfm_table(cbind(names(a), round(a, 2)))
end.rcode-->

Collecting gene expression micorarray data became more popular over time: XX% of articles in our sample were published in 2001, compared to YY% in 2009.

<!--begin.rcode yearTable
gfm_table(table(dfCitationsAttributesRaw$pubmed_year_published)/nrow(dfCitationsAttributesRaw))

end.rcode-->


Searching for associated datasets in the GEO and ArrayExpress repository uncovered links between XXX% of papers in this sample and publicly available data.  
<!--begin.rcode sharing, fig.width=6, fig.height=6
table(dfCitationsAttributes$dataset.in.geo.or.ae.int)

gfm_table(table(dfCitationsAttributes$dataset.in.geo.or.ae.int)/nrow(dfCitationsAttributes))

end.rcode-->


Articles published more recently were more likely to have associated datasets.

<!--begin.rcode sharing_over_time, fig.width=6, fig.height=6

df.long = melt(dfCitationsAttributes, measure.vars=c('pubmed.year.published'))
df.long.summary = ddply(df.long, .(variable, value), summarize, proportion=sum(dataset.in.geo.or.ae.int > 0) / length(dataset.in.geo.or.ae.int))
ggplot(data=df.long.summary, aes(x=value, y=proportion)) +
  geom_smooth() +
  facet_wrap(~variable) +
  scale_y_continuous(formatter='percent')

end.rcode-->

The articles in our sample were cited between 0 and 2640 times, with an average of 32 citations per paper and a median of 16.  


<!--begin.rcode sharingVCitations
summary(dfCitationsAttributes$nCitedBy)
end.rcode-->

Without accounting for any confounding factors, the mean number of citations between papers with available data and those without are the same, and there is little visible difference in the distribution of citations between these two groups.

<!--begin.rcode sharingVCitations_breakdown
with(dfCitationsAttributes, tapply(nCitedBy, dataset.in.geo.or.ae.int, summary))
ggplot(dfCitationsAttributesRaw, aes(log(1+nCitedBy), fill=factor(in_ae_or_geo))) + geom_density(alpha=0.2) + cbgFillPalette + cbgColourPalette
end.rcode-->


The number of citations a paper has recieved is strongly correlated to the date it was published: older papers have had more time to accumulate citations.  Because data archiving was relatively infrequent for articles published earlier, a difference in citation behaviour may be confounded with publication date.

Indeed, we saw that for any given publication date, papers with associated data recieve more citations than those without.

<!--begin.rcode citationsByYearBySharing, echo=FALSE

dat.subset = dfCitationsAttributes
with(dat.subset, tapply(nCitedBy, pubmed.year.published, median, na.rm=T))
citation_breaks = c(1, 10, 40, 100, 400, 1000)
 
end.rcode-->
    
This difference in citation is not driven by outliers: as shown by the distribution of citations over time, the distribution of citations for older papers with available data is centered at a higher median than citations for papers without data available.

<!--begin.rcode citationDist, echo=FALSE 

ggplot(dat.subset, aes(1+nCitedBy.log, fill=factor(dataset.in.geo.or.ae)), color="black") + geom_density(alpha=0.2) + facet_grid(pubmed.year.published~.) + cbgFillPalette + cbgColourPalette

end.rcode-->

#### Multivariate regression

Other factors have been previously shown to be correlated with citation rate, including number of authors, author experience, author institution, open access status, and subject area [cite].  Regression analysis can be useful to identify the relationship between data availability and citation rate, independently of these other variables.


<!--begin.rcode regressionAll

dfCitationsAttributes_with_journal = merge(dfCitationsAttributes, dfCitationsAttributesRaw[,c("pmid", "pubmed_journal")], by="pmid", )
fit_w_journal = lm(nCitedBy.log ~ rcs(num.authors.tr, 3) + 
          rcs(journal.impact.factor.tr, 3) +               
          factor(pubmed_journal) +
          rcs(pubmed.date.in.pubmed, 3) +
          rcs(first.author.num.prev.pubs.tr, 3) +           
          rcs(first.author.num.prev.pmc.cites.tr, 3) +     
          rcs(first.author.year.first.pub.ago.tr, 3) +     
          rcs(last.author.num.prev.pubs.tr, 3) +           
          rcs(last.author.num.prev.pmc.cites.tr, 3) +      
          rcs(last.author.year.first.pub.ago.tr, 3) +
          country.usa +              
          pubmed.is.open.access +              
          rcs(institution.mean.norm.citation.score, 3) +
          rcs(journal.num.articles.2008.tr, 3) +           
          rcs(journal.cited.halflife, 3) +                 
          factor(pubmed.is.cancer) +
          factor(pubmed.is.animals) +
          factor(pubmed.is.plants) +
          factor(pubmed.is.core.clinical.journal) +
          factor(dataset.in.geo.or.ae)
           , dfCitationsAttributes_with_journal)

gfm_table(anova(fit_w_journal))

# fit_w_journal
citation.boost.coefs.journal = calcCI.exp(fit_w_journal, "factor(dataset.in.geo.or.ae).L")
print(citation.boost.coefs.journal)

end.rcode-->

In this analysis, we found many of the variables were independently associated with citation rate, including number of authors, journal impact factor, the journal itself, the date of publication, the number of previous citations of the fist and last author, the number of previous publications of the last author, whether the paper was about animals or plants, and whether the data was made publicly available.

Estimate of the independent increase of citations due to data availability is  
<!--rinline 100*(citation.boost.coefs.journal$est-1) -->%
with 95% confidence intervals [<!--rinline 100*(citation.boost.coefs.journal$ciLow-1) -->%
, <!--rinline 100*(citation.boost.coefs.journal$ciHigh-1) -->% ]
(p=<!--rinline format(citation.boost.coefs.journal$p, nsmall = 2) -->)

Because publication date is such as strong correlate with both citation rate and data availability, we also ran regressions for each publication year individually (with a subset of the covariates).

<!--begin.rcode regressionByYear

# has a few less covariates than full model
do_analysis = function(mydat) {
  myfit = lm(nCitedBy.log ~ rcs(num.authors.tr, 3) + 
  rcs(pubmed.date.in.pubmed, 3) +
  rcs(first.author.num.prev.pmc.cites.tr, 3) +     
  rcs(last.author.num.prev.pmc.cites.tr, 3) +      
  pubmed.is.open.access +              
  rcs(institution.mean.norm.citation.score, 3) +
  rcs(journal.num.articles.2008.tr, 3) +           
  rcs(journal.impact.factor.tr) + 
  factor(pubmed_journal) +              
  factor(pubmed.is.cancer) +
  factor(pubmed.is.animals) +
  factor(dataset.in.geo.or.ae)
             , mydat)

  gfm_table(anova(myfit))

  myfit

  calcCI.exp(myfit, "factor(dataset.in.geo.or.ae).L")
}


estimates_by_year = data.frame()
for (year in seq(2001, 2009)) {
  dat.subset.year = subset(dfCitationsAttributes_with_journal, pubmed.year.published==year)
  results = do_analysis(dat.subset.year)
  print(results)
  estimates_by_year = rbind(estimates_by_year, cbind(year=year, results))
}

end.rcode-->

The estimate of citation boost was different for different years of publication.

The estimates of citation boost for papers published in each year, with 95% confidence intervals:

<!--begin.rcode regressionEstimatesByYear

estimates_by_year

ggplot(estimates_by_year, aes(x=year, y=est)) + geom_line() + 
  geom_errorbar(width=.1, aes(ymin=ciLow, ymax=ciHigh)) +
  scale_x_continuous(name='year of publication') +
  scale_y_continuous(limits=c(0, 3.0), name='estimated increase in citations\nfor papers with data available (95% confidence intervals)')

end.rcode-->



 

### Subset analysis to compare findings with Piwowar et al 2007

These estimates of citation boost found in the multivariate regression were different from those found by (Piwowar et al 2007), even though both studies looked at publicly available gene expression microarray data. There are several possible reasons for this difference.  

First, Piwowar et al 2007 included only data from human cancer microarray trials published between 1999 and 2003 <check>, whereas the current study uses all gene expression microarray data studies in PubMed from 2001 to 2009. 

Second, because the Piwowar et al 2007 sample was small, the previous analysis included only a few possible covariates: publication date, journal impact factor, and country of the corresponding author.

We attempted to reproduce that environment in the current study to see if we would find more comperable results.

Limiting the current sample to datasets with MeSH terms "human" and "cancer" published from 2001 to 2003 retained 308 papers.  Running this subsample with  covariates from the Piwowar 2007 paper found a comperable estimate to the 2007 paper: a citation increase of 47% (95% confidence intervals of 6% to 103%).

<!--begin.rcode RegressionAlaPrevStudy
  dat.subset.previous.study = subset(dfCitationsAttributes, (pubmed.year.published<2003) & (pubmed.is.cancer==1) & (pubmed.is.humans==1))

  dim(dat.subset.previous.study)

  myfitprev = lm(nCitedBy.log ~ 
    rcs(pubmed.date.in.pubmed, 3) +
    country.usa +              
    rcs(journal.impact.factor.tr, 3) +               
    factor(dataset.in.geo.or.ae)
               , dat.subset.previous.study)

  gfm_table(anova(myfitprev))
  myfitprev

  calcCI.exp(myfitprev, "factor(dataset.in.geo.or.ae).L")
end.rcode-->

How is did this estimate change when we included additional covariates?  The subsample of 308 papers was large enough to include a few additional covariates:  number of authors and citation history of the last author.  Including these covariates returned  a smaller estimated effect: 18% with a confidence interval that spanned a *loss* of 17% citations to a boost of 66%.  This range is too wide to be instructive, other than to note its top end is close to the previous rough estimates.

<!--begin.rcode RegressionAlaPrevStudyMoreCovariates
  myfit_prev_more = lm(nCitedBy.log ~ 
  rcs(pubmed.date.in.pubmed, 3) +
  country.usa +              
  rcs(num.authors.tr, 3) + 
  #rcs(first.author.num.prev.pmc.cites.tr, 3) +     
  rcs(last.author.num.prev.pmc.cites.tr, 3) +      
  rcs(journal.impact.factor.tr, 3) +               
  factor(dataset.in.geo.or.ae)
             , dat.subset.previous.study)

  gfm_table(anova(myfit_prev_more))
  myfit_prev_more

  calcCI.exp(myfit_prev_more, "factor(dataset.in.geo.or.ae).L")
end.rcode-->

### Subset analysis with manual classification of data availability 

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

Our method of identifying which articles create gene expression microarray data made a nontrivial number of errors: about 10% of the articles it identified as creating gene expression microarray data do not in fact create gene expression datasets [cite].

The papers that are erroniously included in our subset to not create gene expression data, so they certainly don''t have associated archived datasets: all  erroniously included papers were automatically classified in the "no archived data" group. 

If it were true that these erroniously-included articles recieved many more or many fewer citations than other articles in the group, their inclusion could influence the findings of this study.

To verify our assumption that the influence of these mistakenly-included articles is in fact small, we manually reviewed a random 226 of the 11k (get exact number) articles.

Of these manually reviewed articles, 206 did indeed create gene expression microarray data, and 20 did not (but satisfied the boolean-search query for other reasons).  

<!--begin.rcode percent
206/226
end.rcode-->

Examining the citations of the  20 articles that did not create gene expression data revealed that these studies were cited less often than those that did create data: a mean of 26 citations compared to a mean of 32 citations.  The overall distribution of citations for articles that did not create gene expression data is closer to zero than the distribution of citations for articles that did create gene expression data.

<!--begin.rcode manualAnnotationCreatedCitations
with(dfCitationsAnnotated, summary(nCitedBy~isCreated))
ggplot(dfCitationsAnnotated, aes(log(1+nCitedBy), fill=factor(isCreated))) + geom_density(alpha=0.2) + cbgFillPalette + cbgColourPalette
end.rcode-->

This difference, however, was found to be not statisitically significantly different at the p<0.05 level, using either a t-test on the log of the citation counts or a Wilcoxon rank sum test on the raw citation counts.

<!--begin.rcode manualAnnotationCreatedStats
with(dfCitationsAnnotated, print(t.test(nCitedBy~isCreated)))
with(dfCitationsAnnotated, print(t.test(log(1+nCitedBy)~isCreated)))
with(dfCitationsAnnotated, print(wilcox.test(nCitedBy~isCreated)))
end.rcode-->

To confirm that the erroniously-included articles were not driving the findings about the citation relationship with data availability, we ran a multivariate regresssino analysis on the subsample of 206 articles that we manually determined did in fact generate gene expression microarray data.  The estimated effect is statistically significant and similar to the findings from the whole sample.

<!--begin.rcode manualAnnotationCreatedRegression
annotated_merged_created = lm(nCitedBy.log ~ 
  rcs(pubmed.date.in.pubmed, 3) +
  country.usa +              
  rcs(num.authors.tr, 3) + 
  #rcs(first.author.num.prev.pmc.cites.tr, 3) +     
  rcs(last.author.num.prev.pmc.cites.tr, 3) +      
  rcs(journal.impact.factor.tr, 3) +               
  factor(dataset.in.geo.or.ae)
             , dat.annotated.merged.created)

gfm_table(anova(annotated_merged_created))
calcCI.exp(annotated_merged_created, "factor(dataset.in.geo.or.ae).L")
end.rcode-->


### Complementary evidence of data reuse from accession number attribution

Finally, to provide evidence on the timeline of data attribution, we report preliminary data on third-party data reuse.  Large-scale evidence is difficult to gather because it requires manual citation context classification, as described above.  A partial estimate is possible, however, due to attribution norms in some fields: count the number of times a dataset accession number is mentioned in the scientific literature. (Piwowar, Vision, Whitlock) 

A citation boost due to public data availability would come from authors who would not have otherwise had access to the data.  The timeline of third-party reuse can be estimated by identifying all papers that reuse data, then eliminating those with author names in common with the data collection team.  Results from tracking datasets deposited into GEO in 2007 were reported in (Piwowar, Vision, Whitlock).  As one can see from the figure, the rate of data reuse by third parties continues to increase three years after article publication.  


<img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/3rdpartygrowth.png" class="plot" height=300 />


### Complementary evidence of data reuse from citation context

To provide evidence on the proportion of the citation boost that may be caused by data reuse, we report the observed frequency with which papers that shared gene expression microarray data were cited in the context of data attribution.  Citations to papers that describe 100 datasets deposited into GEO in 2005 were collected using Web of Science: XXX total citations were found.  138 citations were randomly selected and manually reviewed.  

<!--begin.rcode citationContextData
dfTracking1k = read.csv("data/tracking1k_20111008.csv", sep=",", header=TRUE, stringsAsFactors=F)
dfTracking1k.GEO.subset = subset(dfTracking1k, TAG.source=="WoS" & TAG.confidence!="low confidence" & is.na(duplicates & TAG.repository=="GEO" & (TAG.dataset.reused=="dataset reused" | TAG.dataset.reused=="dataset not reused")))

num.GEO.total = dim(dfTracking1k.GEO.subset)[1]
num.GEO.reused = dim(subset(dfTracking1k.GEO.subset, TAG.dataset.reused=="dataset reused"))[1]
annotated.prop = binconf(num.GEO.reused, num.GEO.total)
num.GEO.total
num.GEO.reused
annotated.prop
end.rcode-->

Of the <!--rinline num.GEO.total --> reviewed citations to articles with archived gene expression data, <!--rinline num.GEO.reused --> were in the context of data reuse
<!--rinline 100*(round(annotated.prop[1], 2)) -->%
with 95% confidence intervals [<!--rinline 100*(round(annotated.prop[2], 2)) -->%
, <!--rinline 100*(round(annotated.prop[3], 2)) -->% ]



## Discussion

*1. summary of results*
- summary of results
- citation boost consistent with some previous findings.  Particularly consistent with multivariate analysis of (Milia et al).
"Our multivariate analysis showed that time since publication and impact factor are the main factor influencing the number of citations received by datasets (see Table S5). A slight increase (8.9%) in the number of citations was observed for shared datasets, with a more pronounced advantage (20.6%) for mtDNA (Table S6), but, again, no difference was found to be associated with a statistically significant result in our multivariate analysis."
- the number of papers that reused data was still increasing rapidly after three years.  This suggests that the relatively low level of citation boost we observe for papers published in 2007-2009 may be because not enough time has passed for reuse articles to have been written in large quantity. 


- potential for greater boost if authors always attributed data reuse through citations, rather than sometimes through in-text accession number (cite the "Beginning to Track 1k datasets" abstract for estimated breakdown of citations-to-papers vs attribution-through-accessionnumber for GEO data)  *this is parallel to one of the points in the limitations section.*
- put these findings in the context of the history of gene expression microarray data: Todd''s email.  *suggestions on the discussion angle for this?*
- unknown factor of how important poorly documented over time. documentation changes over time.  high citation early on, if real, may be true that high standardization is not a prereq for data reuse.  todd's email
- don't know if changes over time

- what is the citaiton boost
- how uniform accross year and journals
- changing standards like MIAME
- role of covariates, and other ones we didn't look like liek standards
- other caveates to results
- how to does it relate to previous
(variablility, belief, etc)

*2. timeline of data reuse*

*3. what is the cause of the boost*

What might be the cause of a citation boost for papers with publicly available data?  The most obvious source of is attribution for data reuse, but there may be additional contributions from other sources.  The literature on the "Open Access Citation Benefit" has articulated several possible sources of OA citation boost, including Selection Bias and Early View.citep(biblio["Craig2007"]).  We suggest the possible sources for an "Open Data Citation Benefit" include:

1. *Data Reuse.*  Papers with available data can be used in additional ways than papers without available data, therefore additional citations may accrue due to new reasons for attribution.
1. *Credibility Signalling.*  The credibility of research findings may be higher for research papers with available data.  It might then be preferentially used for background citations and/or the foundation of additional research.
1. *Selection Bias.*  Authors may be more likely to make data available for papers they judge to be their best quality work, because they are most proud or confident in the results.  ALTERNATIVELY, it is possible that author self-selection bias may have a negative correlation with research impact in the case of Open Data: authors may be less willing to share details for their most important and visible research in order to maintain a competitive edge and avoid the upheaval of error detection.
1. *Increased Visibility.*  Citing authors may be more likely to encounter a research project with available data.  More artifacts associated with a research project gives the project a larger footprint, increasing the likelihood that someone finds an aspect of the research.  Citations from data to the research paper may also increase the search ranking of the research paper.  
1. *Early View.*  When data is made available before a paper is published, it is possible that some citations may accrue earlier than otherwise because the area of research has been disclosed prior to paper publication.

The estimated citation boost in the current study is consistent with observed data reuse alone, but the error bounds are large enough that other sources may also have contributed.  Unforuntaely, given the current dataset, it is difficult to establish which sources might have caused the observed boost.Further work, with additional data, will be needed to understand the relative contributions from each source.  For example, hypothetical examples could be provided to authors to determine whether they would be systematically more likely to cite a paper with available data in situations where they are considering the credibility of the findings.  Analyses within the pubication output of a selection of data-collecting authors may enable measurement of selection bias.  Observing search behaviour of researchers, and the returned search hit results, may provide evidence of increased visibility due to data availability.  The contribution of early views to citations would depend on the data availablily timeline within the domain and datatype of study.

*4. future work include this?*

Data reuse, and the attendant efficiencies and discoveries, is a primary motivation for requiring that research data be made available.  Consequently, accurate measurement of data reuse is particularly important.  Future work can improve on previous studies by considering all methods of data use attribution.  This holistic effort would include identifying citations to the paper that describes the data collection, mentions of the dataset identifier itself -- whether in full text, the references section, or supplementary information -- and even acknowledgements to the data collection investigators.  The citations and mentions would need classification based on context to ensure they are in the context of data reuse.

These estimates of data reuse could then be used to estimate the impact of policy decisions.  For example, do embargo periods decrease the level of data reuse?  Do restrictive or poorly articulated licencing terms decrease data reuse?  Which types of data reuse are facilitate by robust data standards and which types are unaffected?

Future work is also needed to assess other important metrics of reuse, beyond citation.  The impact on practitioners, education, data journalism, and industry research are not captured by attibution patterns in the scientific literature.  Altmetrics indicators uncover discussions in social social media, syallabi, patents, and theses: analyzing such indicators for datasets would provide valuable evidence of reuse beyond the scientific literature.


*5. implications.  wrap-up thoughts*

The findings of this study suggest that papers with available data make a bigger impact on the scientific literature than similar papers without available data.  Making an impact is important to funders, journals, and authors themselves.  A ten percent increase in the impact of a research project is noteworthy, given that today journals fight for journal impact factor scores to two decimal places.  As evaluators move away from assessing research based on journal impact factor and toward article-level metrics, post-publication citation rates are becoming key indicators of research impact.  

The strongest rationale for making research data broadly availble is not that it may increase one's citation count: full description of experimental process and findings is a tenant of science and publicly-funded science is a public resource[http://www.nature.com/news/open-your-minds-and-share-your-results-1.10895].  Nonetheless, robust evidence of personal benefit will help as science transitions to a culture that expects data to be widely available. 













*limitations.  work these into flow of discussion*

- automated methods were imperfect: full text to the scientific literature would permit more sophisticated and accurate retrieval techniques based on full-text.
- This is an underestimate of total reuse (some attribution through accession number, some attribution is in citations in supplementary information which is not indexed by Scopus, some papers that may cite aren''t indexed by Scopus) *maybe inlude this point in general discussion as mentioned below, rather than in limitations?*
- Due to mechanics of accessing so many citations through Scopus website, weren''t able to get detailed timing of each citation, so all citation counts were censored as of the collection date rather than a fixed time period after the date of publication.  Also, we can''t tell if low level of citation boost in recent articles is because they have yet to accumulate or because the number of available datasets is now so large that the reuse level of any specific article has decreased.
- (negative binomial is probably a better statistical technique than linear regression, but it isn''t standard)
- we didn't gather evidence about when the data was made available, though previous work suggested it was usually at the time of paper publication (Piwowar 2007)
- Correlation doesn''t imply causation.  Although this analysis includes more variables, other important ones are still missing: funder, funding levels, etc.

*fit these in?*

- Making data available doesn’t just increase the impact of research by a certain amount: opens it up to whole new *types* of use. 
- Archiving data has costs, but the hassle of archiving data is very small 
relative to all the work that goes into a research publication.


## Acknowledgements

- CISTI for Scopus access
- British Library helpers
- Angus, Todd, Jonathan, Estephanie
- my funding, Jonathan + Estephanie’s funding

## References

see references in [Mendeley library](http://www.mendeley.com/groups/2223913/11k-citation/papers/)

### Experimenting with knitr citations
Demo citing thank Carl for his great library! 
<!--begin.rcode knitCitationsExperiment, echo=FALSE, results="asis", cache=FALSE
citep(list(citation("knitcitations"))) 
end.rcode-->. 

Now cite everyone! 
<!--begin.rcode knitCitationsBibtexExperiment, echo=FALSE, results="asis", cache=FALSE
biblio <- read.bibtex("citation11k.bib")

citep(biblio[names(biblio)])
end.rcode-->

### demo bibliography

<!--begin.rcode bib, results='asis', echo=FALSE, cache=FALSE
bibliography()
end.rcode-->

### Other studies of citation benefit:

- Gleditsch, Nils Petter & Håvard Strand, 2003. 'Posting Your Data: Will You Be Scooped or Will You Be Famous?', International Studies Perspectives 4(1): 89–97.
- Henneken, Edwin A and Accomazzi, Alberto.  Linking to Data - Effect on Citation Rates in Astronomy. eprint arXiv:1111.3618 11/2011
- Ioannidis et al. Repeatability of published microarray gene expression analyses  Nature Genetics 41, 149 - 155 (2009) .  doi:10.1038/ng.295
- Pienta et al The Research Data Life Cycle and the Probability of Secondary Use in Re-Analysis 
The Research Data Life Cycle and the Probability of Secondary Use in Re-Analysis 
- Amy M. Pienta, George Alter, Jared Lyle.  The Enduring Value of Social Science Research: The Use and Reuse of Primary Research Data.  http://hdl.handle.net/2027.42/78307 
- Piwowar HA, Day RS, Fridsma DB (2007) Sharing Detailed Research Data Is Associated with Increased Citation Rate. PLoS ONE 2(3): e308. doi:10.1371/journal.pone.0000308
- http://www.komfor.net/blog/unbenanntemitteilung
- Milia N, Congiu A, Anagnostou P, Montinaro F, Capocasa M, et al. (2012) Mine, Yours, Ours? Sharing Data on Human Genetic Variation. PLoS ONE 7(6): e37552. doi:10.1371/journal.pone.0037552

### Used in this analysis

- Piwowar HA (2011) Who shares? Who doesn’t? Factors associated with openly archiving raw research data. PLoS ONE 6(7): e18657. doi:10.1371/journal.pone.0018657
- Piwowar HA (2011) Data from: Who shares? Who doesn’t? Factors associated with openly archiving raw research data. Dryad Digital Repository. doi:10.5061/dryad.mf1sd
- Heather A Piwowar, Wendy W Chapman (2010)  Recall and bias of retrieving gene expression microarray datasets through PubMed identifiers  Journal of Biomedical Discovery and Collaboration.  J Biomed Discov Collab. 2010; 5: 7–20.
- Heather A. Piwowar; Jonathan D. Carlson; and Todd J. Vision. Beginning to Track 1000 Datasets from Public Repositories into the Published Literature.  ASIS&T 2011.

### Also relevant:

- (Data Usage Index):  Chavan, V. S., & Ingwersen, P. (2009). Towards a data publishing framework for primary biodiversity data: challenges and potentials for the biodiversity informatics community. BMC Bioinformatics, 10(Suppl 14), S2. Retrieved from http://www.biomedcentral.com/1471-2105/10/S14/S2
- Piwowar, H. A., Vision, T. J., & Whitlock, M. C. (2011). Data archiving is a good investment. Nature, 473(7347), 285-285. Nature Publishing Group, a division of Macmillan Publishers Limited. All Rights Reserved. Retrieved from http://dx.doi.org/10.1038/473285a
- Bollen J, Van de Sompel H, Hagberg A, Chute R (2009) A Principal Component Analysis of 39 Scientific Impact Measures. PLoS ONE 4(6): e6022. doi:10.1371/journal.pone.0006022
- Ochsner, S. A., Steffen, D. L., Stoeckert, C. J., & McKenna, N. J. (2008). Much room for improvement in deposition rates of expression microarray datasets. Nature Methods. Retrieved from http://dx.doi.org/10.1038/nmeth1208-991
- Definitely some things about move to citation of datasets themselves
- altmetrics on CV, away from impact factor

### Other studies of correlation with citations:

- Bioinformatics, 25, 3303-3309 (2009). "Predicting citation count of Bioinformatics papers within four years of publication." Ibanez, A., Larrañaga, P. and Bielza, C.  http://bioinformatics.oxfordjournals.org/content/25/24/3303.full


## Abstract (hasn't been updated)

### Background
Attribution upon reuse of scientific data is important to reward data creators and document the provenance of research findings.  In many fields, data attribution commonly takes the form of citation to the paper that described the primary data collection.  Previous studies have found that papers with publicly available datasets do indeed receive a higher number of citations than similar studies without available data.  However, previous studies were relatively small and did not control for many variables known to predict citation rate.  In this analysis we look at citation rates while controlling for many known citation predictors, and investigate whether the estimated citation boost is consistent with evidence of data reuse.

### Methods and Results
In a multivariate linear regression on <!--rinline dim(dfCitationsAttributesRaw)[1] --> studies that created gene expression microarray data, we found that studies with data in centralized public repositories received 
<!--rinline 100*(citation.boost.coefs.journal$est-1) -->%
(95% confidence interval: [<!--rinline 100*(citation.boost.coefs.journal$ciLow-1) -->%
to <!--rinline 100*(citation.boost.coefs.journal$ciHigh-1) -->%)
more citations than similar studies without available data.  Date of publication, journal impact factor, journal citation half-life, journal size, number of authors, first and last author number of previous publications and citations, corresponding author country, institution citation mean score, and study topic were included as covariates.  A small independent investigation of citations to microarray studies with publicly available data found that about 
<!--rinline 100*(round(annotated.prop[1], 2)) -->%
(95% CI: <!--rinline 100*(round(annotated.prop[2], 2)) -->%
to <!--rinline 100*(round(annotated.prop[3], 2)) -->%, 
n=<!--rinline num.GEO.total -->)
of citations to those studies were in the context of data reuse attribution.

### Discussion
This analysis reveals a modest but substantiated boost in data citation rates across a wide selection of studies that made their data publicly available.  Though modest, the impact represented by these data attributions should not be underestimated: attribution in the context of data reuse demonstrates a real and demonstrable contribution to subsequent research.



