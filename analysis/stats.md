


# knitr citation11k manuscript
 * author Heather Piwowar, <hpiwowar@gmail.com>
 * license: CC0
 * Acknowledgements: thanks to Carl Boettiger and knitr for this literate programming framework!
 * Generated on <code class="knitr inline">Sun Jun 10 06:47:53 2012</code>

To run this I start R, set the working directory to match where this file is, then run the following in R:

    library(knitr)  
    knit("stats_knit_.md")

or, from the command line

    R -e "library(knitr); knit('stats_knit_.md')"; pandoc --toc -r markdown -w html -H static/header.html stats.md > stats.html
    view in browser: file:///Users/hpiwowar/Documents/Projects/citation%20benefit%20in%2011k%20study/citation11k/analysis/stats.html

to see just the R code in a separate .R file called stats_knit_.R, run 
    R -e "library(knitr); knit('stats_knit_.md', tangle=T)"


tjv: my first comment!











# Data Reuse and the Open Data Citation Advantage
*Piwowar, Carlson, Vision*


## Goal
1. Is there an association between data availability and citation rate, independently of important known citation predictors?
1. Is there supporting evidence that additional citations are due to data reuse?

## Abstract

See the [end of this document](#abstract-1) (at the end so it can pull in results from the R analysis).

## Introduction

"Sharing information facilitates science. Publicly sharing detailed research data–sample attributes, clinical factors, patient outcomes, DNA sequences, raw mRNA microarray measurements–with other researchers allows these valuable resources to contribute far beyond their original analysis[1]. In addition to being used to confirm original results, raw data can be used to explore related or new hypotheses, particularly when combined with other publicly available data sets. Real data is indispensable when investigating and developing study methods, analysis techniques, and software implementations. The larger scientific community also benefits: sharing data encourages multiple perspectives, helps to identify errors, discourages fraud, is useful for training new researchers, and increases efficient use of funding and patient population resources by avoiding duplicate data collection." [Piwowar, Sharing] 

When research data is made publicly available, is there a demonstrable benefit to scientific progress and the study investigators?  

Citations are often used as a proxy for the scientific contribution of a paper.  Citations are also used in research funding and promotion decisions; Boosting citation rate is thus is a potentially important motivator for publication authors.  Scientists report that an increase in citations is an important motivator for sharing their data [Tenopir].

Previous studies, across several disciplines, have explored the relationship between the citation rate of a publication and whether its data was made publicly available[list].  However, these studies were relatively small (confirm true for all of them) and did not include key covariates that may have conflated estimates of citation boost.  Number of authors, author experience, author institution, open access status, and subject area have been shown to predict citation rate (cite) and may also be correlated with the public availability of datasets.

Here, we report an analysis based on a large cohort of relatively homogenious studies. The size our cohort has facilitated controlling for many more variables than previous studies, allowing us to make further progress in isolating the citation rate relationship with data archiving itself.

Clinical microarray data provides a useful environment for the investigation: despite being valuable for reuse valuable for reuse [butte] and well-supported by data sharing standards and infrastructure [], fewer than half of the studies that collect this data make it publicly available [Ochsner, Piwowar]


## Methods

Analysis run on <code class="knitr inline">Sun Jun 10 06:47:54 2012</code>.

### Identification of relevant studies

- Piwowar HA (2011) Who shares? Who doesn’t? Factors associated with openly archiving raw research data. PLoS ONE 6(7): e18657. doi:10.1371/journal.pone.0018657
- Piwowar HA (2011) Data from: Who shares? Who doesn’t? Factors associated with openly archiving raw research data. Dryad Digital Repository. doi:10.5061/dryad.mf1sd




### Assessment of data availability

Summarize approach in Who shares? paper.

### Collection of study attributes

Summarize approach in Who shares? paper.

### Citation data

Citations from Scopus.



### Statistical methods

### Data and script availability


## Results

### Primary analysis of relationship between data availability and citation

#### Description of cohort

We begin with articles that have been identified as collecting gene expression microarray data by automatic algorithms looking for keywords in article full text (Piwowar 2011).  

<pre class="knitr"><div class="output">## [1] 10555    86
</div></pre>


For this analysis of citation behaviour, we retain articles published between 2001 and 2009: <code class="knitr inline">1.0555 &times; 10<sup>4</sup></code> articles.

The composition of this sample is spread across XXX journals, with the top N journals accounting for XXX% of the papers.

<pre class="knitr"><div class="source"><span class="symbol">a</span> <span class="assignement">=</span> <span class="functioncall">sort</span><span class="keyword">(</span><span class="functioncall">table</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributesRaw</span><span class="keyword">$</span><span class="symbol">pubmed_journal</span><span class="keyword">)</span><span class="keyword">/</span><span class="functioncall">nrow</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributesRaw</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">dec</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span><span class="keyword">[</span><span class="number">1</span><span class="keyword">:</span><span class="number">10</span><span class="keyword">]</span>
<span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">cbind</span><span class="keyword">(</span><span class="functioncall">names</span><span class="keyword">(</span><span class="symbol">a</span><span class="keyword">)</span><span class="keyword">,</span> <span class="functioncall">round</span><span class="keyword">(</span><span class="symbol">a</span><span class="keyword">,</span> <span class="number">2</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## | Cancer Res               | 0.04 |
## | Proc Natl Acad Sci U S A | 0.04 |
## | J Biol Chem              | 0.04 |
## | BMC Genomics             | 0.03 |
## | Physiol Genomics         | 0.03 |
## | PLoS One                 | 0.02 |
## | J Bacteriol              | 0.02 |
## | J Immunol                | 0.02 |
## | Blood                    | 0.02 |
## | Clin Cancer Res          | 0.02 |
</div></pre>


Collecting gene expression micorarray data became more popular over time: XX% of articles in our sample were published in 2001, compared to YY% in 2009.

<pre class="knitr"><div class="source"><span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">table</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributesRaw</span><span class="keyword">$</span><span class="symbol">pubmed_year_published</span><span class="keyword">)</span><span class="keyword">/</span><span class="functioncall">nrow</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributesRaw</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## |   | 2001 | 2002 | 2003 | 2004 | 2005 | 2006 | 2007 | 2008 | 2009 |
## |---|------|------|------|------|------|------|------|------|------|
## | 1 | 0.02 | 0.05 | 0.08 | 0.11 | 0.13 | 0.12 | 0.17 | 0.18 | 0.15 |
</div></pre>



Searching for associated datasets in the GEO and ArrayExpress repository uncovered links between XXX% of papers in this sample and publicly available data.  Articles published more recently were more likely to have associated datasets.

<pre class="knitr"><div class="source"><span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">table</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">$</span><span class="symbol">dataset.in.geo.or.ae.int</span><span class="keyword">)</span><span class="keyword">/</span><span class="functioncall">nrow</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## |   | 0    | 1    |
## |---|------|------|
## | 1 | 0.75 | 0.25 |
</div><div class="source">
<span class="functioncall">table</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">$</span><span class="symbol">dataset.in.geo.or.ae.int</span><span class="keyword">)</span>
</div><div class="output">## 
##    0    1 
## 7938 2617 
</div><div class="source">
<span class="symbol">df.long</span> <span class="assignement">=</span> <span class="functioncall">melt</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">,</span> <span class="argument">measure.vars</span><span class="argument">=</span><span class="functioncall">c</span><span class="keyword">(</span><span class="string">'pubmed.year.published'</span><span class="keyword">)</span><span class="keyword">)</span>
<span class="functioncall">dim</span><span class="keyword">(</span><span class="symbol">df.long</span><span class="keyword">)</span>
</div><div class="output">## [1] 10555    87
</div><div class="source"><span class="symbol">df.long.summary</span> <span class="assignement">=</span> <span class="functioncall">ddply</span><span class="keyword">(</span><span class="symbol">df.long</span><span class="keyword">,</span> <span class="functioncall">.</span><span class="keyword">(</span><span class="symbol">variable</span><span class="keyword">,</span> <span class="symbol">value</span><span class="keyword">)</span><span class="keyword">,</span> <span class="symbol">summarize</span><span class="keyword">,</span> <span class="argument">proportion</span><span class="argument">=</span><span class="functioncall">sum</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae.int</span> <span class="keyword">&gt;</span> <span class="number">0</span><span class="keyword">)</span> <span class="keyword">/</span> <span class="functioncall">length</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae.int</span><span class="keyword">)</span><span class="keyword">)</span>

<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="argument">data</span><span class="argument">=</span><span class="symbol">df.long.summary</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">x</span><span class="argument">=</span><span class="symbol">value</span><span class="keyword">,</span> <span class="argument">y</span><span class="argument">=</span><span class="symbol">proportion</span><span class="keyword">)</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">geom_smooth</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">facet_wrap</span><span class="keyword">(</span><span class="keyword">~</span><span class="symbol">variable</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">formatter</span><span class="argument">=</span><span class="string">'percent'</span><span class="keyword">)</span>
</div><img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/sharing.png" class="plot" />
</pre>


The articles in our sample were cited between 0 and 2640 times, with an average of 32 citations per paper and a median of 16.  

Without accounting for any confounding factors, the mean number of citations between papers with available data and those without are the same, and there is little visible difference in the distribution of citations between these two groups.

<pre class="knitr"><div class="source"><span class="functioncall">summary</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">$</span><span class="symbol">nCitedBy</span><span class="keyword">)</span>
</div><div class="output">##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     0.0     7.0    16.0    31.5    35.0  2640.0 
</div><div class="source">
<span class="functioncall">with</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">,</span> <span class="functioncall">tapply</span><span class="keyword">(</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="symbol">dataset.in.geo.or.ae.int</span><span class="keyword">,</span> <span class="symbol">summary</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## $`0`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     0.0     7.0    16.0    31.6    35.0  2560.0 
## 
## $`1`
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     0.0     7.0    16.0    31.3    34.0  2640.0 
## 
</div><div class="source">
<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributesRaw</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="functioncall">log</span><span class="keyword">(</span><span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">in_ae_or_geo</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_density</span><span class="keyword">(</span><span class="argument">alpha</span><span class="argument">=</span><span class="number">0.2</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>
</div><img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/sharingVCitations.png" class="plot" />
</pre>


#### Multivariate visualization

The number of citations a paper has recieved is strongly correlated to the date it was published: older papers have had more time to accumulate citations.  Because data archiving was relatively infrequent for articles published earlier, a difference in citation behaviour may be confounded with publication date.

Indeed, we can see that for any given publication date, papers with associated data recieve more citations than those without.

<pre class="knitr"><div class="output">## 2001 2002 2003 2004 2005 2006 2007 2008 2009 
## 76.0 54.0 40.0 30.0 24.0 18.0 14.0  9.5  5.0 
</div><img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/citationsByYearBySharing.png" class="plot" />
</pre>

    
This difference in citation is not driven by outliers: as shown by the distribution of citations over time, the distribution of citations for older papers with available data is centered at a higher median than citations for papers without data available.

<pre class="knitr"><img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/citationDist.png" class="plot" />
</pre>


These differences could be because journals with high impact are more likely to require data archiving.  To investigate this, we consider the most common 12 journals in our subset.  Journal by journal, the mean citation rate for papers with data available is not always greater than the citation rate of papers without data available.

<pre class="knitr"><img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/citationDistByJournal.png" class="plot" />
</pre>


We turn again to the distribution of citation rates to understand the patterns in more depth.  Considering only papers published in 2005, we see that papers with available data do tend to receive more citations than those without.  Molecular Cell Biology and Blood are perhaps exceptions to this trend.

<pre class="knitr"><img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/citationDistByJournalOneYear.png" class="plot" />
<div class="output">##                           in_ae_or_geo
## pubmed_journal                  0      1
##   Blood                    0.8750 0.1250
##   Cancer Res               0.7797 0.2203
##   Clin Cancer Res          0.8571 0.1429
##   J Bacteriol              0.8400 0.1600
##   J Biol Chem              0.7660 0.2340
##   J Immunol                0.8125 0.1875
##   Mol Cell Biol            0.7931 0.2069
##   Physiol Genomics         0.1875 0.8125
##   Plant Physiol            0.7037 0.2963
##   Proc Natl Acad Sci U S A 0.8125 0.1875
</div></pre>


#### Multivariate regression

Other factors are also known or suspected to be correlated with citation rate, including number of authors, author experience, author institution, open access status, and subject area.  Regression analysis can be useful to identify the relationship between data availability and citation rate, independently of these other variables.

<pre class="knitr"><div class="source">
<span class="symbol">dfCitationsAttributes_with_journal</span> <span class="assignement">=</span> <span class="functioncall">merge</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">,</span> <span class="symbol">dfCitationsAttributesRaw</span><span class="keyword">[</span><span class="keyword">,</span><span class="functioncall">c</span><span class="keyword">(</span><span class="string">"pmid"</span><span class="keyword">,</span> <span class="string">"pubmed_journal"</span><span class="keyword">)</span><span class="keyword">]</span><span class="keyword">,</span> <span class="argument">by</span><span class="argument">=</span><span class="string">"pmid"</span><span class="keyword">,</span> <span class="keyword">)</span>
<span class="symbol">fit_w_journal</span> <span class="assignement">=</span> <span class="functioncall">lm</span><span class="keyword">(</span><span class="symbol">nCitedBy.log</span> <span class="keyword">~</span> <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">num.authors.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">journal.impact.factor.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">pubmed_journal</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">pubmed.date.in.pubmed</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">first.author.num.prev.pubs.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">first.author.num.prev.pmc.cites.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">first.author.year.first.pub.ago.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">last.author.num.prev.pubs.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">last.author.num.prev.pmc.cites.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">last.author.year.first.pub.ago.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="symbol">country.usa</span> <span class="keyword">+</span>
          <span class="symbol">pubmed.is.open.access</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">institution.mean.norm.citation.score</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">journal.num.articles.2008.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">journal.cited.halflife</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">pubmed.is.cancer</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">pubmed.is.animals</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">pubmed.is.plants</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">pubmed.is.core.clinical.journal</span><span class="keyword">)</span> <span class="keyword">+</span>
          <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span>
           <span class="keyword">,</span> <span class="symbol">dfCitationsAttributes_with_journal</span><span class="keyword">)</span>

<span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">anova</span><span class="keyword">(</span><span class="symbol">fit_w_journal</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## |                                              | Df      | Sum Sq  | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|---------|---------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00    | 165.79  | 82.90   | 164.83  | 0.00   |
## | rcs(journal.impact.factor.tr, 3)             | 2.00    | 995.77  | 497.89  | 990.00  | 0.00   |
## | factor(pubmed_journal)                       | 427.00  | 650.85  | 1.52    | 3.03    | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00    | 1280.36 | 640.18  | 1272.94 | 0.00   |
## | rcs(first.author.num.prev.pubs.tr, 3)        | 2.00    | 1.39    | 0.69    | 1.38    | 0.25   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00    | 23.34   | 11.67   | 23.21   | 0.00   |
## | rcs(first.author.year.first.pub.ago.tr, 3)   | 2.00    | 0.94    | 0.47    | 0.94    | 0.39   |
## | rcs(last.author.num.prev.pubs.tr, 3)         | 2.00    | 5.53    | 2.76    | 5.50    | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00    | 10.53   | 5.26    | 10.47   | 0.00   |
## | rcs(last.author.year.first.pub.ago.tr, 3)    | 2.00    | 1.35    | 0.67    | 1.34    | 0.26   |
## | country.usa                                  | 1.00    | 1.13    | 1.13    | 2.25    | 0.13   |
## | pubmed.is.open.access                        | 1.00    | 0.44    | 0.44    | 0.88    | 0.35   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00    | 0.58    | 0.29    | 0.57    | 0.56   |
## | factor(pubmed.is.cancer)                     | 1.00    | 0.13    | 0.13    | 0.26    | 0.61   |
## | factor(pubmed.is.animals)                    | 1.00    | 19.65   | 19.65   | 39.08   | 0.00   |
## | factor(pubmed.is.plants)                     | 1.00    | 3.17    | 3.17    | 6.29    | 0.01   |
## | factor(dataset.in.geo.or.ae)                 | 1.00    | 9.55    | 9.55    | 18.99   | 0.00   |
## | Residuals                                    | 3921.00 | 1971.93 | 0.50    |         |        |
</div><div class="source">
<span class="comment"># fit_w_journal</span>
<span class="symbol">citation.boost.coefs.journal</span> <span class="assignement">=</span> <span class="functioncall">calcCI.exp</span><span class="keyword">(</span><span class="symbol">fit_w_journal</span><span class="keyword">,</span> <span class="string">"factor(dataset.in.geo.or.ae).L"</span><span class="keyword">)</span>
<span class="functioncall">print</span><span class="keyword">(</span><span class="symbol">citation.boost.coefs.journal</span><span class="keyword">)</span>
</div><div class="output">##                                   param  est ciLow ciHigh p
## Estimate factor(dataset.in.geo.or.ae).L 1.09  1.05   1.13 0
</div></pre>


Estimate of citation boost is 
<code class="knitr inline">9</code>%
with 95% confidence intervals [<code class="knitr inline">5</code>%
, <code class="knitr inline">13</code>% ]
(p=<code class="knitr inline">0.00</code>)

Because publication rate is such as strong correlate with both citation rate and data availability, we also ran regressions for each publication year individually.

<pre class="knitr"><div class="source">
<span class="comment"># has a few less covariates than full model</span>
<span class="symbol">do_analysis</span> <span class="assignement">=</span> <span class="keyword">function</span><span class="keyword">(</span><span class="formalargs">mydat</span><span class="keyword">)</span> <span class="keyword">{</span>
  <span class="symbol">myfit</span> <span class="assignement">=</span> <span class="functioncall">lm</span><span class="keyword">(</span><span class="symbol">nCitedBy.log</span> <span class="keyword">~</span> <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">num.authors.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">pubmed.date.in.pubmed</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">first.author.num.prev.pmc.cites.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">last.author.num.prev.pmc.cites.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="symbol">pubmed.is.open.access</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">institution.mean.norm.citation.score</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">journal.num.articles.2008.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">journal.impact.factor.tr</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">pubmed_journal</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">pubmed.is.cancer</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">pubmed.is.animals</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span>
             <span class="keyword">,</span> <span class="symbol">mydat</span><span class="keyword">)</span>

  <span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">anova</span><span class="keyword">(</span><span class="symbol">myfit</span><span class="keyword">)</span><span class="keyword">)</span>

  <span class="symbol">myfit</span>

  <span class="functioncall">calcCI.exp</span><span class="keyword">(</span><span class="symbol">myfit</span><span class="keyword">,</span> <span class="string">"factor(dataset.in.geo.or.ae).L"</span><span class="keyword">)</span>
<span class="keyword">}</span>


<span class="symbol">estimates_by_year</span> <span class="assignement">=</span> <span class="functioncall">data.frame</span><span class="keyword">(</span><span class="keyword">)</span>
<span class="keyword">for</span> <span class="keyword">(</span><span class="symbol">year</span> <span class="keyword">in</span> <span class="functioncall">seq</span><span class="keyword">(</span><span class="number">2001</span><span class="keyword">,</span> <span class="number">2009</span><span class="keyword">)</span><span class="keyword">)</span> <span class="keyword">{</span>
  <span class="symbol">dat.subset.year</span> <span class="assignement">=</span> <span class="functioncall">subset</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes_with_journal</span><span class="keyword">,</span> <span class="symbol">pubmed.year.published</span>==<span class="symbol">year</span><span class="keyword">)</span>
  <span class="symbol">results</span> <span class="assignement">=</span> <span class="functioncall">do_analysis</span><span class="keyword">(</span><span class="symbol">dat.subset.year</span><span class="keyword">)</span>
  <span class="functioncall">print</span><span class="keyword">(</span><span class="symbol">results</span><span class="keyword">)</span>
  <span class="symbol">estimates_by_year</span> <span class="assignement">=</span> <span class="functioncall">rbind</span><span class="keyword">(</span><span class="symbol">estimates_by_year</span><span class="keyword">,</span> <span class="functioncall">cbind</span><span class="keyword">(</span><span class="argument">year</span><span class="argument">=</span><span class="symbol">year</span><span class="keyword">,</span> <span class="symbol">results</span><span class="keyword">)</span><span class="keyword">)</span>
<span class="keyword">}</span>
</div><div class="output">## |                                              | Df    | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|-------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00  | 10.07  | 5.03    | 8.33    | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00  | 0.49   | 0.24    | 0.40    | 0.67   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00  | 2.52   | 1.26    | 2.09    | 0.13   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00  | 14.09  | 7.04    | 11.66   | 0.00   |
## | pubmed.is.open.access                        | 1.00  | 0.02   | 0.02    | 0.03    | 0.87   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00  | 5.78   | 2.89    | 4.78    | 0.01   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00  | 0.96   | 0.48    | 0.79    | 0.46   |
## | rcs(journal.impact.factor.tr)                | 4.00  | 4.06   | 1.02    | 1.68    | 0.17   |
## | factor(pubmed_journal)                       | 32.00 | 16.68  | 0.52    | 0.86    | 0.67   |
## | factor(pubmed.is.cancer)                     | 1.00  | 0.24   | 0.24    | 0.40    | 0.53   |
## | factor(pubmed.is.animals)                    | 1.00  | 0.29   | 0.29    | 0.47    | 0.49   |
## | factor(dataset.in.geo.or.ae)                 | 1.00  | 0.40   | 0.40    | 0.66    | 0.42   |
## | Residuals                                    | 48.00 | 28.99  | 0.60    |         |        |
##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.31  0.68   2.54 0.422
## |                                              | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00   | 7.65   | 3.83    | 7.18    | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00   | 2.87   | 1.43    | 2.69    | 0.07   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00   | 1.88   | 0.94    | 1.76    | 0.18   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00   | 13.88  | 6.94    | 13.01   | 0.00   |
## | pubmed.is.open.access                        | 1.00   | 2.54   | 2.54    | 4.77    | 0.03   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00   | 5.35   | 2.67    | 5.02    | 0.01   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00   | 3.21   | 1.60    | 3.01    | 0.05   |
## | rcs(journal.impact.factor.tr)                | 4.00   | 33.47  | 8.37    | 15.69   | 0.00   |
## | factor(pubmed_journal)                       | 80.00  | 73.73  | 0.92    | 1.73    | 0.00   |
## | factor(pubmed.is.cancer)                     | 1.00   | 1.13   | 1.13    | 2.11    | 0.15   |
## | factor(pubmed.is.animals)                    | 1.00   | 0.54   | 0.54    | 1.01    | 0.32   |
## | factor(dataset.in.geo.or.ae)                 | 1.00   | 0.18   | 0.18    | 0.34    | 0.56   |
## | Residuals                                    | 149.00 | 79.43  | 0.53    |         |        |
##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.09  0.82   1.45 0.559
## |                                              | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00   | 11.90  | 5.95    | 12.18   | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00   | 11.65  | 5.83    | 11.93   | 0.00   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00   | 9.87   | 4.93    | 10.10   | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00   | 13.57  | 6.78    | 13.89   | 0.00   |
## | pubmed.is.open.access                        | 1.00   | 0.11   | 0.11    | 0.22    | 0.64   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00   | 2.46   | 1.23    | 2.52    | 0.08   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00   | 4.79   | 2.39    | 4.90    | 0.01   |
## | rcs(journal.impact.factor.tr)                | 4.00   | 26.55  | 6.64    | 13.59   | 0.00   |
## | factor(pubmed_journal)                       | 119.00 | 78.63  | 0.66    | 1.35    | 0.03   |
## | factor(pubmed.is.cancer)                     | 1.00   | 1.02   | 1.02    | 2.09    | 0.15   |
## | factor(pubmed.is.animals)                    | 1.00   | 2.80   | 2.80    | 5.74    | 0.02   |
## | factor(dataset.in.geo.or.ae)                 | 1.00   | 0.34   | 0.34    | 0.69    | 0.41   |
## | Residuals                                    | 234.00 | 114.31 | 0.49    |         |        |
##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.09  0.89   1.35 0.407
## |                                              | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00   | 24.77  | 12.39   | 23.64   | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00   | 5.73   | 2.87    | 5.47    | 0.00   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00   | 22.94  | 11.47   | 21.89   | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00   | 23.95  | 11.97   | 22.85   | 0.00   |
## | pubmed.is.open.access                        | 1.00   | 6.66   | 6.66    | 12.72   | 0.00   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00   | 2.72   | 1.36    | 2.60    | 0.08   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00   | 9.60   | 4.80    | 9.16    | 0.00   |
## | rcs(journal.impact.factor.tr)                | 4.00   | 48.00  | 12.00   | 22.90   | 0.00   |
## | factor(pubmed_journal)                       | 160.00 | 126.10 | 0.79    | 1.50    | 0.00   |
## | factor(pubmed.is.cancer)                     | 1.00   | 0.23   | 0.23    | 0.44    | 0.51   |
## | factor(pubmed.is.animals)                    | 1.00   | 0.07   | 0.07    | 0.14    | 0.71   |
## | factor(dataset.in.geo.or.ae)                 | 1.00   | 4.21   | 4.21    | 8.04    | 0.00   |
## | Residuals                                    | 330.00 | 172.91 | 0.52    |         |        |
##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.23  1.07   1.42 0.005
## |                                              | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00   | 25.81  | 12.90   | 28.05   | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00   | 0.60   | 0.30    | 0.66    | 0.52   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00   | 23.89  | 11.94   | 25.96   | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00   | 12.34  | 6.17    | 13.41   | 0.00   |
## | pubmed.is.open.access                        | 1.00   | 0.04   | 0.04    | 0.10    | 0.76   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00   | 7.80   | 3.90    | 8.48    | 0.00   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00   | 15.58  | 7.79    | 16.93   | 0.00   |
## | rcs(journal.impact.factor.tr)                | 4.00   | 52.61  | 13.15   | 28.59   | 0.00   |
## | factor(pubmed_journal)                       | 186.00 | 105.98 | 0.57    | 1.24    | 0.04   |
## | factor(pubmed.is.cancer)                     | 1.00   | 1.42   | 1.42    | 3.09    | 0.08   |
## | factor(pubmed.is.animals)                    | 1.00   | 0.98   | 0.98    | 2.13    | 0.15   |
## | factor(dataset.in.geo.or.ae)                 | 1.00   | 7.04   | 7.04    | 15.31   | 0.00   |
## | Residuals                                    | 380.00 | 174.80 | 0.46    |         |        |
##                                   param est ciLow ciHigh p
## Estimate factor(dataset.in.geo.or.ae).L 1.3  1.14   1.48 0
## |                                              | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00   | 35.85  | 17.92   | 37.15   | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00   | 2.45   | 1.23    | 2.54    | 0.08   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00   | 19.51  | 9.75    | 20.22   | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00   | 17.65  | 8.83    | 18.30   | 0.00   |
## | pubmed.is.open.access                        | 1.00   | 0.23   | 0.23    | 0.48    | 0.49   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00   | 2.80   | 1.40    | 2.91    | 0.06   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00   | 8.04   | 4.02    | 8.33    | 0.00   |
## | rcs(journal.impact.factor.tr)                | 4.00   | 65.35  | 16.34   | 33.86   | 0.00   |
## | factor(pubmed_journal)                       | 177.00 | 125.97 | 0.71    | 1.48    | 0.00   |
## | factor(pubmed.is.cancer)                     | 1.00   | 0.14   | 0.14    | 0.28    | 0.59   |
## | factor(pubmed.is.animals)                    | 1.00   | 8.09   | 8.09    | 16.76   | 0.00   |
## | factor(dataset.in.geo.or.ae)                 | 1.00   | 1.15   | 1.15    | 2.37    | 0.12   |
## | Residuals                                    | 383.00 | 184.77 | 0.48    |         |        |
##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.09  0.98   1.23 0.124
## |                                              | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00   | 24.19  | 12.10   | 26.67   | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00   | 5.35   | 2.67    | 5.90    | 0.00   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00   | 11.75  | 5.88    | 12.96   | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00   | 11.00  | 5.50    | 12.13   | 0.00   |
## | pubmed.is.open.access                        | 1.00   | 0.01   | 0.01    | 0.01    | 0.90   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00   | 2.76   | 1.38    | 3.05    | 0.05   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00   | 5.52   | 2.76    | 6.09    | 0.00   |
## | rcs(journal.impact.factor.tr)                | 4.00   | 57.39  | 14.35   | 31.64   | 0.00   |
## | factor(pubmed_journal)                       | 210.00 | 133.80 | 0.64    | 1.41    | 0.00   |
## | factor(pubmed.is.cancer)                     | 1.00   | 0.11   | 0.11    | 0.24    | 0.63   |
## | factor(pubmed.is.animals)                    | 1.00   | 1.66   | 1.66    | 3.66    | 0.06   |
## | factor(dataset.in.geo.or.ae)                 | 1.00   | 0.11   | 0.11    | 0.23    | 0.63   |
## | Residuals                                    | 500.00 | 226.72 | 0.45    |         |        |
##                                   param  est ciLow ciHigh    p
## Estimate factor(dataset.in.geo.or.ae).L 1.02  0.93   1.12 0.63
## |                                              | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00   | 41.30  | 20.65   | 36.27   | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00   | 15.52  | 7.76    | 13.62   | 0.00   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00   | 6.68   | 3.34    | 5.87    | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00   | 16.62  | 8.31    | 14.59   | 0.00   |
## | pubmed.is.open.access                        | 1.00   | 2.08   | 2.08    | 3.66    | 0.06   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00   | 5.42   | 2.71    | 4.76    | 0.01   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00   | 10.35  | 5.18    | 9.09    | 0.00   |
## | rcs(journal.impact.factor.tr)                | 4.00   | 59.85  | 14.96   | 26.28   | 0.00   |
## | factor(pubmed_journal)                       | 219.00 | 133.41 | 0.61    | 1.07    | 0.28   |
## | factor(pubmed.is.cancer)                     | 1.00   | 0.01   | 0.01    | 0.01    | 0.92   |
## | factor(pubmed.is.animals)                    | 1.00   | 5.79   | 5.79    | 10.16   | 0.00   |
## | factor(dataset.in.geo.or.ae)                 | 1.00   | 0.06   | 0.06    | 0.10    | 0.75   |
## | Residuals                                    | 448.00 | 255.09 | 0.57    |         |        |
##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.02  0.91   1.13 0.752
## |                                              | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(num.authors.tr, 3)                       | 2.00   | 43.96  | 21.98   | 50.23   | 0.00   |
## | rcs(pubmed.date.in.pubmed, 3)                | 2.00   | 15.57  | 7.78    | 17.79   | 0.00   |
## | rcs(first.author.num.prev.pmc.cites.tr, 3)   | 2.00   | 7.31   | 3.65    | 8.35    | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3)    | 2.00   | 10.94  | 5.47    | 12.50   | 0.00   |
## | pubmed.is.open.access                        | 1.00   | 0.57   | 0.57    | 1.30    | 0.26   |
## | rcs(institution.mean.norm.citation.score, 3) | 2.00   | 8.23   | 4.11    | 9.40    | 0.00   |
## | rcs(journal.num.articles.2008.tr, 3)         | 2.00   | 7.82   | 3.91    | 8.93    | 0.00   |
## | rcs(journal.impact.factor.tr)                | 4.00   | 59.57  | 14.89   | 34.04   | 0.00   |
## | factor(pubmed_journal)                       | 200.00 | 123.27 | 0.62    | 1.41    | 0.00   |
## | factor(pubmed.is.cancer)                     | 1.00   | 0.02   | 0.02    | 0.04    | 0.84   |
## | factor(pubmed.is.animals)                    | 1.00   | 2.58   | 2.58    | 5.90    | 0.02   |
## | factor(dataset.in.geo.or.ae)                 | 1.00   | 0.45   | 0.45    | 1.02    | 0.31   |
## | Residuals                                    | 331.00 | 144.84 | 0.44    |         |        |
##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 0.95  0.85   1.05 0.314
</div></pre>


The estimates of citation boost for papers published in each year, with 95% confidence intervals:

<pre class="knitr"><div class="source">
<span class="symbol">estimates_by_year</span>
</div><div class="output">##           year                          param  est ciLow ciHigh     p
## Estimate  2001 factor(dataset.in.geo.or.ae).L 1.31  0.68   2.54 0.422
## Estimate1 2002 factor(dataset.in.geo.or.ae).L 1.09  0.82   1.45 0.559
## Estimate2 2003 factor(dataset.in.geo.or.ae).L 1.09  0.89   1.35 0.407
## Estimate3 2004 factor(dataset.in.geo.or.ae).L 1.23  1.07   1.42 0.005
## Estimate4 2005 factor(dataset.in.geo.or.ae).L 1.30  1.14   1.48 0.000
## Estimate5 2006 factor(dataset.in.geo.or.ae).L 1.09  0.98   1.23 0.124
## Estimate6 2007 factor(dataset.in.geo.or.ae).L 1.02  0.93   1.12 0.630
## Estimate7 2008 factor(dataset.in.geo.or.ae).L 1.02  0.91   1.13 0.752
## Estimate8 2009 factor(dataset.in.geo.or.ae).L 0.95  0.85   1.05 0.314
</div><div class="source">
<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">estimates_by_year</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">x</span><span class="argument">=</span><span class="symbol">year</span><span class="keyword">,</span> <span class="argument">y</span><span class="argument">=</span><span class="symbol">est</span><span class="keyword">)</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_line</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">geom_errorbar</span><span class="keyword">(</span><span class="argument">width</span><span class="argument">=</span><span class="number">.1</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">ymin</span><span class="argument">=</span><span class="symbol">ciLow</span><span class="keyword">,</span> <span class="argument">ymax</span><span class="argument">=</span><span class="symbol">ciHigh</span><span class="keyword">)</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">scale_x_continuous</span><span class="keyword">(</span><span class="argument">name</span><span class="argument">=</span><span class="string">'year of publication'</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">limits</span><span class="argument">=</span><span class="functioncall">c</span><span class="keyword">(</span><span class="number">0</span><span class="keyword">,</span> <span class="number">3.0</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">name</span><span class="argument">=</span><span class="string">'estimated increase in citations\nfor papers with data available (95% confidence intervals)'</span><span class="keyword">)</span>
</div><img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/regressionEstimatesByYear.png" class="plot" />
</pre>


    

### Subset analysis to compare findings with Piwowar et al 2007

These results differ from  those found by (Piwowar et al 2007). There are several possible reasons for this.  

First, those analyses were only using human cancer microarray trials published between 1999 and 2003 <check>.  

Second, because the Piwowar et al 2007 sample was small, Piwowar et al 2007 analysis included only a few possible covariates: publication date, journal impact factor, and country of the corresponding author.

When we limit the current sample to datasets with MeSH terms "human" and "cancer" in that timeframe, we are left with 308 papers.  Running this subsample with the covariates used in the Piwowar 2007 paper finds a comperable result to that of the 2007 paper: a citation increase of 47% (95% confidence intervals of 6% to 103%).

<pre class="knitr"><div class="source">  <span class="symbol">dat.subset.previous.study</span> <span class="assignement">=</span> <span class="functioncall">subset</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">,</span> <span class="keyword">(</span><span class="symbol">pubmed.year.published</span><span class="keyword">&lt;</span><span class="number">2003</span><span class="keyword">)</span> <span class="keyword">&amp;</span> <span class="keyword">(</span><span class="symbol">pubmed.is.cancer</span>==<span class="number">1</span><span class="keyword">)</span> <span class="keyword">&amp;</span> <span class="keyword">(</span><span class="symbol">pubmed.is.humans</span>==<span class="number">1</span><span class="keyword">)</span><span class="keyword">)</span>

  <span class="functioncall">dim</span><span class="keyword">(</span><span class="symbol">dat.subset.previous.study</span><span class="keyword">)</span>
</div><div class="output">## [1] 308  86
</div><div class="source">
  <span class="symbol">myfitprev</span> <span class="assignement">=</span> <span class="functioncall">lm</span><span class="keyword">(</span><span class="symbol">nCitedBy.log</span> <span class="keyword">~</span>
    <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">pubmed.date.in.pubmed</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
    <span class="symbol">country.usa</span> <span class="keyword">+</span>
    <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">journal.impact.factor.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
    <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span>
               <span class="keyword">,</span> <span class="symbol">dat.subset.previous.study</span><span class="keyword">)</span>

  <span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">anova</span><span class="keyword">(</span><span class="symbol">myfitprev</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## |                                  | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |----------------------------------|--------|--------|---------|---------|--------|
## | rcs(pubmed.date.in.pubmed, 3)    | 2.00   | 5.33   | 2.67    | 3.27    | 0.04   |
## | country.usa                      | 1.00   | 0.00   | 0.00    | 0.01    | 0.94   |
## | rcs(journal.impact.factor.tr, 3) | 2.00   | 68.86  | 34.43   | 42.26   | 0.00   |
## | factor(dataset.in.geo.or.ae)     | 1.00   | 4.35   | 4.35    | 5.34    | 0.02   |
## | Residuals                        | 294.00 | 239.53 | 0.81    |         |        |
</div><div class="source">  <span class="symbol">myfitprev</span>
</div><div class="output">## 
## Call:
## lm(formula = nCitedBy.log ~ rcs(pubmed.date.in.pubmed, 3) + country.usa + 
##     rcs(journal.impact.factor.tr, 3) + factor(dataset.in.geo.or.ae), 
##     data = dat.subset.previous.study)
## 
## Coefficients:
##                                               (Intercept)  
##                                                 26.970315  
##        rcs(pubmed.date.in.pubmed, 3)pubmed.date.in.pubmed  
##                                                 -0.000699  
##       rcs(pubmed.date.in.pubmed, 3)pubmed.date.in.pubmed'  
##                                                  0.000211  
##                                             country.usa.L  
##                                                  0.015396  
##  rcs(journal.impact.factor.tr, 3)journal.impact.factor.tr  
##                                                  0.908247  
## rcs(journal.impact.factor.tr, 3)journal.impact.factor.tr'  
##                                                 -0.080694  
##                            factor(dataset.in.geo.or.ae).L  
##                                                  0.383500  
## 
</div><div class="source">
  <span class="functioncall">calcCI.exp</span><span class="keyword">(</span><span class="symbol">myfitprev</span><span class="keyword">,</span> <span class="string">"factor(dataset.in.geo.or.ae).L"</span><span class="keyword">)</span>
</div><div class="output">##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.47  1.06   2.03 0.021
</div></pre>


We found that adding in a few additional covariates to analysis with this subsample, number of authors and citation history of the last author, decreased the estimated effect to 18% and its confidence interval to span a *loss* of 17% citations to a boost of 66%.  This range is too wide to be instructive: mostly it is just useful to note it is largely consistent with previous findings.

<pre class="knitr"><div class="source">  <span class="symbol">myfit_prev_more</span> <span class="assignement">=</span> <span class="functioncall">lm</span><span class="keyword">(</span><span class="symbol">nCitedBy.log</span> <span class="keyword">~</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">pubmed.date.in.pubmed</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="symbol">country.usa</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">num.authors.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="comment">#rcs(first.author.num.prev.pmc.cites.tr, 3) +     </span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">last.author.num.prev.pmc.cites.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">journal.impact.factor.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span>
             <span class="keyword">,</span> <span class="symbol">dat.subset.previous.study</span><span class="keyword">)</span>

  <span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">anova</span><span class="keyword">(</span><span class="symbol">myfit_prev_more</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## |                                           | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |-------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(pubmed.date.in.pubmed, 3)             | 2.00   | 5.55   | 2.78    | 3.60    | 0.03   |
## | country.usa                               | 1.00   | 0.05   | 0.05    | 0.07    | 0.79   |
## | rcs(num.authors.tr, 3)                    | 2.00   | 37.81  | 18.90   | 24.48   | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3) | 2.00   | 17.16  | 8.58    | 11.11   | 0.00   |
## | rcs(journal.impact.factor.tr, 3)          | 2.00   | 34.49  | 17.24   | 22.33   | 0.00   |
## | factor(dataset.in.geo.or.ae)              | 1.00   | 0.66   | 0.66    | 0.85    | 0.36   |
## | Residuals                                 | 283.00 | 218.53 | 0.77    |         |        |
</div><div class="source">  <span class="symbol">myfit_prev_more</span>
</div><div class="output">## 
## Call:
## lm(formula = nCitedBy.log ~ rcs(pubmed.date.in.pubmed, 3) + country.usa + 
##     rcs(num.authors.tr, 3) + rcs(last.author.num.prev.pmc.cites.tr, 
##     3) + rcs(journal.impact.factor.tr, 3) + factor(dataset.in.geo.or.ae), 
##     data = dat.subset.previous.study)
## 
## Coefficients:
##                                                                 (Intercept)  
##                                                                  24.4893104  
##                          rcs(pubmed.date.in.pubmed, 3)pubmed.date.in.pubmed  
##                                                                  -0.0006343  
##                         rcs(pubmed.date.in.pubmed, 3)pubmed.date.in.pubmed'  
##                                                                   0.0000496  
##                                                               country.usa.L  
##                                                                   0.0319357  
##                                        rcs(num.authors.tr, 3)num.authors.tr  
##                                                                   0.0030413  
##                                       rcs(num.authors.tr, 3)num.authors.tr'  
##                                                                   0.4026870  
##  rcs(last.author.num.prev.pmc.cites.tr, 3)last.author.num.prev.pmc.cites.tr  
##                                                                  -0.0008419  
## rcs(last.author.num.prev.pmc.cites.tr, 3)last.author.num.prev.pmc.cites.tr'  
##                                                                   0.0113416  
##                    rcs(journal.impact.factor.tr, 3)journal.impact.factor.tr  
##                                                                   0.8674805  
##                   rcs(journal.impact.factor.tr, 3)journal.impact.factor.tr'  
##                                                                  -0.1908536  
##                                              factor(dataset.in.geo.or.ae).L  
##                                                                   0.1631734  
## 
</div><div class="source">
  <span class="functioncall">calcCI.exp</span><span class="keyword">(</span><span class="symbol">myfit_prev_more</span><span class="keyword">,</span> <span class="string">"factor(dataset.in.geo.or.ae).L"</span><span class="keyword">)</span>
</div><div class="output">##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.18  0.83   1.66 0.357
</div></pre>



### Subset analysis with manual classification of data availability 




Because of the limited power of text mining through boolean search queries, our method of identifying which articles create gene expression microarray data makes a nontrivial number of errors: about 10% of the identified articles do not in fact create gene expression datasets.  

Because they do not create datasets, they certainly don''t have archived datasets: they are all classified in the "no archived data" group. 

If it were true that these erroniously-included articles recieve many more or many fewer citatios than other articles in the group, their inclusion could influence the findings of the study.

To verify our assumption that the influence of these mistakenly-included articles is in fact small, we manually reviewed a random 226 of the 11k (get exact number) articles.

Of these, manual review of the article full-text confirmed that 206 of the studies did indeed create gene expression microarray data, and 20 of the studies did not (but satisfied the boolean-search query for other reasons).  T

The 20 articles that did not create gene expression data were cited less often than those that did create data: 26 times compared to 32 times.  The overall distribution for articles that did not create gene expression data is shifted downward.

<pre class="knitr"><div class="source"><span class="functioncall">with</span><span class="keyword">(</span><span class="symbol">dfCitationsAnnotated</span><span class="keyword">,</span> <span class="functioncall">summary</span><span class="keyword">(</span><span class="symbol">nCitedBy</span><span class="keyword">~</span><span class="symbol">isCreated</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## nCitedBy    N=226, 4 Missing
## 
## +---------+---------------------------+---+--------+
## |         |                           |  N|nCitedBy|
## +---------+---------------------------+---+--------+
## |isCreated|    created-microarray-data|206|   31.86|
## |         |created-microarray-data-not| 20|   26.30|
## +---------+---------------------------+---+--------+
## |  Overall|                           |226|   31.37|
## +---------+---------------------------+---+--------+
</div><div class="source"><span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dfCitationsAnnotated</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="functioncall">log</span><span class="keyword">(</span><span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">isCreated</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_density</span><span class="keyword">(</span><span class="argument">alpha</span><span class="argument">=</span><span class="number">0.2</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>
</div><img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/manualAnnotationCreatedCitations.png" class="plot" />
</pre>


This difference, however, was found to be not statisitically significantly different, using either a t-test on the log of the citation counts or a Wilcoxon rank sum test on the  citation counts.

<pre class="knitr"><div class="source"><span class="functioncall">with</span><span class="keyword">(</span><span class="symbol">dfCitationsAnnotated</span><span class="keyword">,</span> <span class="functioncall">print</span><span class="keyword">(</span><span class="functioncall">t.test</span><span class="keyword">(</span><span class="symbol">nCitedBy</span><span class="keyword">~</span><span class="symbol">isCreated</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## 
## 	Welch Two Sample t-test
## 
## data:  nCitedBy by isCreated 
## t = 0.5747, df = 22.61, p-value = 0.5712
## alternative hypothesis: true difference in means is not equal to 0 
## 95 percent confidence interval:
##  -14.47  25.59 
## sample estimates:
##     mean in group created-microarray-data 
##                                     31.86 
## mean in group created-microarray-data-not 
##                                     26.30 
## 
</div><div class="source"><span class="functioncall">with</span><span class="keyword">(</span><span class="symbol">dfCitationsAnnotated</span><span class="keyword">,</span> <span class="functioncall">print</span><span class="keyword">(</span><span class="functioncall">t.test</span><span class="keyword">(</span><span class="functioncall">log</span><span class="keyword">(</span><span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">)</span><span class="keyword">~</span><span class="symbol">isCreated</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## 
## 	Welch Two Sample t-test
## 
## data:  log(1 + nCitedBy) by isCreated 
## t = 1.331, df = 21.77, p-value = 0.1968
## alternative hypothesis: true difference in means is not equal to 0 
## 95 percent confidence interval:
##  -0.2003  0.9175 
## sample estimates:
##     mean in group created-microarray-data 
##                                     2.991 
## mean in group created-microarray-data-not 
##                                     2.632 
## 
</div><div class="source"><span class="functioncall">with</span><span class="keyword">(</span><span class="symbol">dfCitationsAnnotated</span><span class="keyword">,</span> <span class="functioncall">print</span><span class="keyword">(</span><span class="functioncall">wilcox.test</span><span class="keyword">(</span><span class="symbol">nCitedBy</span><span class="keyword">~</span><span class="symbol">isCreated</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## 
## 	Wilcoxon rank sum test with continuity correction
## 
## data:  nCitedBy by isCreated 
## W = 2440, p-value = 0.1733
## alternative hypothesis: true location shift is not equal to 0 
## 
</div></pre>


To confirm that the erroniously-included articles were not driving the findings, we reran the analysis on the subsample of 206 articles that we manually determined did in fact generate gene expression microarray data.  The estimated effect is statistically significant and similar to the findings from the whole sample.

<pre class="knitr"><div class="source"><span class="symbol">annotated_merged_created</span> <span class="assignement">=</span> <span class="functioncall">lm</span><span class="keyword">(</span><span class="symbol">nCitedBy.log</span> <span class="keyword">~</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">pubmed.date.in.pubmed</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="symbol">country.usa</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">num.authors.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="comment">#rcs(first.author.num.prev.pmc.cites.tr, 3) +     </span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">last.author.num.prev.pmc.cites.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">rcs</span><span class="keyword">(</span><span class="symbol">journal.impact.factor.tr</span><span class="keyword">,</span> <span class="number">3</span><span class="keyword">)</span> <span class="keyword">+</span>
  <span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span>
             <span class="keyword">,</span> <span class="symbol">dat.annotated.merged.created</span><span class="keyword">)</span>

<span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">anova</span><span class="keyword">(</span><span class="symbol">annotated_merged_created</span><span class="keyword">)</span><span class="keyword">)</span>
</div><div class="output">## |                                           | Df     | Sum Sq | Mean Sq | F value | Pr(>F) |
## |-------------------------------------------|--------|--------|---------|---------|--------|
## | rcs(pubmed.date.in.pubmed, 3)             | 2.00   | 83.82  | 41.91   | 73.91   | 0.00   |
## | country.usa                               | 1.00   | 0.21   | 0.21    | 0.38    | 0.54   |
## | rcs(num.authors.tr, 3)                    | 2.00   | 11.18  | 5.59    | 9.86    | 0.00   |
## | rcs(last.author.num.prev.pmc.cites.tr, 3) | 2.00   | 7.94   | 3.97    | 7.00    | 0.00   |
## | rcs(journal.impact.factor.tr, 3)          | 2.00   | 8.23   | 4.11    | 7.26    | 0.00   |
## | factor(dataset.in.geo.or.ae)              | 1.00   | 5.68   | 5.68    | 10.03   | 0.00   |
## | Residuals                                 | 177.00 | 100.37 | 0.57    |         |        |
</div><div class="source"><span class="functioncall">calcCI.exp</span><span class="keyword">(</span><span class="symbol">annotated_merged_created</span><span class="keyword">,</span> <span class="string">"factor(dataset.in.geo.or.ae).L"</span><span class="keyword">)</span>
</div><div class="output">##                                   param  est ciLow ciHigh     p
## Estimate factor(dataset.in.geo.or.ae).L 1.32  1.11   1.57 0.002
</div></pre>



### Complementary evidence of data reuse from citation context

To put data citation boost in the context of data reuse, we report evidence on the observed frequency with which papers that share gene expression microarray data are cited in the context of data attribution.  Citations to papers that describe 100 datasets deposited into GEO in 2005 were collected using Web of Science: XXX total citations were found.  138 citations were randomly selected and manually reviewed.  

<pre class="knitr"><div class="source"><span class="symbol">dfTracking1k</span> <span class="assignement">=</span> <span class="functioncall">read.csv</span><span class="keyword">(</span><span class="string">"data/tracking1k_20111008.csv"</span><span class="keyword">,</span> <span class="argument">sep</span><span class="argument">=</span><span class="string">","</span><span class="keyword">,</span> <span class="argument">header</span><span class="argument">=</span><span class="number">TRUE</span><span class="keyword">,</span> <span class="argument">stringsAsFactors</span><span class="argument">=</span><span class="symbol">F</span><span class="keyword">)</span>
<span class="symbol">dfTracking1k.GEO.subset</span> <span class="assignement">=</span> <span class="functioncall">subset</span><span class="keyword">(</span><span class="symbol">dfTracking1k</span><span class="keyword">,</span> <span class="symbol">TAG.source</span>==<span class="string">"WoS"</span> <span class="keyword">&amp;</span> <span class="symbol">TAG.confidence</span><span class="keyword">!=</span><span class="string">"low confidence"</span> <span class="keyword">&amp;</span> <span class="functioncall">is.na</span><span class="keyword">(</span><span class="symbol">duplicates</span> <span class="keyword">&amp;</span> <span class="symbol">TAG.repository</span>==<span class="string">"GEO"</span> <span class="keyword">&amp;</span> <span class="keyword">(</span><span class="symbol">TAG.dataset.reused</span>==<span class="string">"dataset reused"</span> <span class="keyword">|</span> <span class="symbol">TAG.dataset.reused</span>==<span class="string">"dataset not reused"</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>

<span class="symbol">num.GEO.total</span> <span class="assignement">=</span> <span class="functioncall">dim</span><span class="keyword">(</span><span class="symbol">dfTracking1k.GEO.subset</span><span class="keyword">)</span><span class="keyword">[</span><span class="number">1</span><span class="keyword">]</span>
<span class="symbol">num.GEO.reused</span> <span class="assignement">=</span> <span class="functioncall">dim</span><span class="keyword">(</span><span class="functioncall">subset</span><span class="keyword">(</span><span class="symbol">dfTracking1k.GEO.subset</span><span class="keyword">,</span> <span class="symbol">TAG.dataset.reused</span>==<span class="string">"dataset reused"</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">[</span><span class="number">1</span><span class="keyword">]</span>
<span class="symbol">annotated.prop</span> <span class="assignement">=</span> <span class="functioncall">binconf</span><span class="keyword">(</span><span class="symbol">num.GEO.reused</span><span class="keyword">,</span> <span class="symbol">num.GEO.total</span><span class="keyword">)</span>
<span class="symbol">num.GEO.total</span>
</div><div class="output">## [1] 138
</div><div class="source"><span class="symbol">num.GEO.reused</span>
</div><div class="output">## [1] 8
</div><div class="source"><span class="symbol">annotated.prop</span>
</div><div class="output">##  PointEst   Lower  Upper
##   0.05797 0.02966 0.1102
</div></pre>


Of the <code class="knitr inline">138</code> reviewed citations to articles with archived gene expression data, <code class="knitr inline">8</code> were in the context of data reuse
<code class="knitr inline">6</code>%
with 95% confidence intervals [<code class="knitr inline">3</code>%
, <code class="knitr inline">11</code>% ]

### Complementary evidence of data reuse from accession number attribution

Finally, we provide preliminary evidence that data reuse takes years to accumulate.  Large-scale evidence is difficult to gather because it requires manual citation context classification, as described above.  As described in  (Piwowar, Vision, Whitlock), a partial estimate is possible due to attribution norms in some fields: count the number of times a dataset accession number is mentioned in the scientific literature.  

A citation boost due to public data availability would come from authors who would not have otherwise had access to the data.  The timeline ofthird-party reuse can be estimated by identifying all papers that reuse data, then eliminating those with author names in common with the data collection team.  Results from tracking datasets deposited into GEO in 2007 were reported in (Piwowar, Vision, Whitlock).  As one can see from the figure, the rate of data reuse by third parties continues to increase three years after article publication.  


<img src="http://dl.dropbox.com/u/5485507/11kCitationStudy/paper/citation11k/analysis/figure/3rdpartygrowth.png" class="plot" />

## Discussion


- summary of results

*limitations. not sure it is worth including all of these, brainstorming*

- automated methods were imperfect: full text to the scientific literature would permit more sophisticated and accurate retrieval techniques based on full-text.
- This is an underestimate of total reuse (some attribution through accession number, some attribution is in citations in supplementary information which is not indexed by Scopus, some papers that may cite aren''t indexed by Scopus)
- Due to mechanics of accessing so many citations through Scopus website, weren''t able to get detailed timing of each citation, so all citation counts were censored as of the collection date rather than a fixed time period after the date of publication.  Also, we can''t tell if low level of citation boost in recent articles is because they have yet to accumulate or because the number of available datasets is now so large that the reuse level of any specific article has decreased.
- (negative binomial is probably a better statistical technique than linear regression, but it isn''t standard)
- we didn't gather evidence about when the data was made available, though previous work suggested it was usually at the time of paper publication (Piwowar 2007)
- Correlation doesn''t imply causation.  Although this analysis includes more variables, other important ones are still missing: funder, funding levels, etc.

*general discussion about these results*

- put these findings in the context of the history of gene expression microarray data: Todd''s email.
- potential for greater boost if authors always attributed data reuse through citations, rather than sometimes through in-text accession number (cite the "Beginning to Track 1k datasets" abstract for estimated breakdown of citations-to-papers vs attribution-through-accessionnumber for GEO data)
- the results we present about third party reuse illustrates that papers which reuse data take  start to emerge several years after data is released: the number of papers that reused data was still increasing rapidly after three years.  This suggests that the relatively low level of citation boost we observe for papers published in 2007-2009 may be because not enough time has passed for reuse articles to have been written in large quantity.
- citation boost consistent with previous findings of others.  Particularly consistent with multivariate analysis of (Milia et al).
"Our multivariate analysis showed that time since publication and impact factor are the main factor influencing the number of citations received by datasets (see Table S5). A slight increase (8.9%) in the number of citations was observed for shared datasets, with a more pronounced advantage (20.6%) for mtDNA (Table S6), but, again, no difference was found to be associated with a statistically significant result in our multivariate analysis."

*discussion about open data boost in general*
<blockquote>
direct excerpt from citep(biblio["Craig2007"]) "
Three non-exclusive postulates have been proposed to account for the observed
citation differences between OA and non-OA articles: an Open Access postulate,
a Selection Bias postulate, and an Early View postulate.
a. The Open Access (OA) postulate suggests that authors are more likely to read, and thus cite, articles that are made available in an OA model.
b. The Selection Bias (SB) postulate suggests that the most prominent (and thus
most citable) authors are more likely to make their articles available in an OA
model, and that they are more to likely do so with their most important (and
thus most citable) articles.
c. The Early View (EV) postulate relates only to articles posted before final
journal publication, and suggests that the period between the early posting of
an article (either pre-print or post-print) and the appearance of the cognate
published journal article allows for earlier accrual of citations. Failing to
account for this effect must necessarily give a biased result.
" </blockquote>

We suggest similar postulates to account for observed citation differences between articles with and without Open Data:

a) Data Reuse postulate suggests that authors can use papers in new ways and so cite them more

b) Selection Bias (SB) as with OA... authors more likely to make data avail if the paper is better because they are better paper because proud of it.  Or, higher quality papers more likely to be published in higher-impact journals, which have stricter data sharing requirements.
ALTERNATIVELY for Open Data this Bias could be negative, when left to the authors. Authors may be willing to share their poor quality research, but want to keep propriety access to their best, most competitive research.

c) Increased visibility (IV).  More places on the web == more artifacts and possibly higher in search rankings.  Authors may encounter these papers more often than similar papers without data available.

d) Credibility Signalling.  Data availability gives credibility to the study so that it is preferentially used for citations where alternatives are available.

Further work is needed to do understand the contributions of these three sources. Citation boost found in this paper is consistent with preliminary evidence on levels of data reuse.  There may also be a boost from Selection Bias and Increased Visibility: future work needed.

*future work*

- More estimates of reuse by exploring citation context, attribution to accession number
- explore citation impact to papers and datasets themselves as scientists begin to cite data directly
- impact of data embargo periods on reuse and citation boost

*wrap-up thoughts*

- Making data available doesn’t just increase the impact of research by a certain amount: opens it up to whole new *types* of use. 
- Citations are not the main reason to make data available: multiple benefits to science and the individual investigator
- There are other important metrics of reuse than just citation. Impact on practicioners, educational use, etc.
- article-level citation numbers are likely to become more important as the flaws in using journal impact factor for article-level assessment become more mainstream and article-level metrics become more available 



## Acknowledgements

- CISTI for Scopus access
- British Library helpers
- Angus, Todd, Jonathan, Estephanie
- my funding, Jonathan + Estephanie’s funding

## References

see references in [Mendeley library](http://www.mendeley.com/groups/2223913/11k-citation/papers/)

### Experimenting with knitr citations
Demo citing thank Carl for his great library! 
<div class="output">[1] "(Boettiger, 2012)"
</div>


Now cite everyone! 
<div class="output">[1] "(Bollen _et. al._ 2009; Chavan & Ingwersen, 2009; Gleditsch & Strand, 2003; Ib\\'{a}\\~{n}ez _et. al._ 2009; Ioannidis _et. al._ 2009; Ochsner _et. al._ 2008; Pienta _et. al._ 2010; Pienta _et. al._ 2006; Piwowar _et. al._ 2007; Piwowar _et. al._ 2011; Piwowar, 2011; character(0); Piwowar _et. al._ 2011; Piwowar & Chapman, 2010; Sears, 2011)"
</div>


### demo bibliography

<div class="output">Boettiger C (2012). _knitcitations: Citations for knitr markdown
files_. R package version 0.0-1.

Bollen J, Van de Sompel H, Hagberg A and Chute R (2009). "A principal
component analysis of 39 scientific impact measures." _PloS one_,
*4*(6), pp. e6022. ISSN 1932-6203, <URL:
http://dx.doi.org/10.1371/journal.pone.0006022>, <URL:
http://dx.plos.org/10.1371/journal.pone.0006022>.

Chavan VS and Ingwersen P (2009). "Towards a data publishing framework
for primary biodiversity data: challenges and potentials for the
biodiversity informatics community." _BMC bioinformatics_, *10 Suppl
1*(Suppl 14), pp. S2. ISSN 1471-2105, <URL:
http://dx.doi.org/10.1186/1471-2105-10-S14-S2>, <URL:
http://www.biomedcentral.com/1471-2105/10/S14/S2>.

Gleditsch NP and Strand H (2003). "Posting your data: will you be
scooped or will you be famous?" _International Studies Perspectives_,
*4*(1), pp. 89-97. <URL:
http://www.prio.no/Research-and-Publications/Publication/?oid=55406>.

Ibáñez A, Larrañaga P and Bielza C (2009). "Predicting citation count
of Bioinformatics papers within four years of publication."
_Bioinformatics (Oxford, England)_, *25*(24), pp. 3303-9. ISSN
1367-4811, <URL: http://dx.doi.org/10.1093/bioinformatics/btp585>,
<URL:
http://bioinformatics.oxfordjournals.org/cgi/content/abstract/25/24/3303>.

Ioannidis JPA, Allison DB, Ball CA, Coulibaly I, Cui X, Culhane AC,
Falchi M, Furlanello C, Game L, Jurman G, Mangion J, Mehta T, Nitzberg
M, Page GP, Petretto E and Noort Vv (2009). "Repeatability of published
microarray gene expression analyses." _Nature genetics_, *41*(2), pp.
149-55. ISSN 1546-1718, <URL: http://dx.doi.org/10.1038/ng.295>, <URL:
http://www.ncbi.nlm.nih.gov/pubmed/19174838>.

Ochsner SA, Steffen DL, Stoeckert CJ and McKenna NJ (2008). "Much room
for improvement in deposition rates of expression microarray datasets."
_Nature methods_, *5*(12), pp. 991. ISSN 1548-7105, <URL:
http://dx.doi.org/10.1038/nmeth1208-991>, <URL:
http://www.pubmedcentral.nih.gov/articlerender.fcgi?artid=2975491\&tool=pmcentrez\&rendertype=abstract>.

Pienta AM, Alter GC and Lyle JA (2010). "The Enduring Value of Social
Science Research: The Use and Reuse of Primary Research Data." _The
Organisation, Economics and Policy of Scientific Research workshop_.
<URL: http://hdl.handle.net/2027.42/78307>.

Pienta A, McNally J and Gutmann M (2006). "The Research Data Life Cycle
and the Probability of Secondary Use in Re-Analysis." <URL:
http://paa2006.princeton.edu/download.aspx?submissionId=61527>.

Piwowar HA, Day RB and Fridsma DS (2007). "Sharing detailed research
data is associated with increased citation rate." _PLoS ONE_, *2*(3).
<URL: http://dx.doi.org/10.1371%2Fjournal.pone.0000308>.

Piwowar HA, Vision TJ and Whitlock MC (2011). "Data archiving is a good
investment." _Nature_, *473*(7347), pp. 285. ISSN 1476-4687, <URL:
http://dx.doi.org/10.1038/473285a>, <URL:
http://dx.doi.org/10.1038/473285a>.

Piwowar HA (2011). "Who Shares? Who Doesn't? Factors Associated with
Openly Archiving Raw Research Data." _PLoS ONE_, *6*(7), pp. e18657.
ISSN 1932-6203, <URL: http://dx.doi.org/10.1371/journal.pone.0018657>,
<URL: http://dx.plos.org/10.1371/journal.pone.0018657>.

Piwowar HA (????). "Data from: Who shares? Who doesn't? Factors
associated with openly archiving raw research data." <URL:
http://dx.doi.org/10.5061/dryad.mf1sd>, <URL:
http://datadryad.org/handle/10255/dryad.33858>.

Piwowar HA, Carlson JD and Vision TJ (2011). "Beginning to track 1000
datasets from public repositories into the published literature."
_Proceedings of the American Society for Information Science and
Technology_, *48*(1), pp. 1-4. ISSN 00447870, <URL:
http://dx.doi.org/10.1002/meet.2011.14504801337>, <URL:
http://doi.wiley.com/10.1002/meet.2011.14504801337>.

Piwowar H and Chapman W (2010). "Recall and bias of retrieving gene
expression microarray datasets through PubMed identifiers." _Journal of
biomedical discovery and collaboration_, *5*, pp. 7-20. ISSN 1747-5333,
<URL: http://www.ncbi.nlm.nih.gov/pubmed/20349403>.

Sears J (2011). "Data Sharing Effect on Article Citation Rate in
Paleoceanography - KomFor." <URL:
http://www.komfor.net/blog/unbenanntemitteilung>.
</div>


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


## Abstract

### Background
Attribution upon reuse of scientific data is important to reward data creators and document the provenance of research findings.  In many fields, data attribution commonly takes the form of citation to the paper that described the primary data collection.  Previous studies have found that papers with publicly available datasets do indeed receive a higher number of citations than similar studies without available data.  However, previous studies were relatively small and did not control for many variables known to predict citation rate.  In this analysis we look at citation rates while controlling for many known citation predictors, and investigate whether the estimated citation boost is consistent with evidence of data reuse.

### Methods and Results
In a multivariate linear regression on <code class="knitr inline">1.0555 &times; 10<sup>4</sup></code> studies that created gene expression microarray data, we found that studies with data in centralized public repositories received 
<code class="knitr inline">9</code>%
(95% confidence interval: [<code class="knitr inline">5</code>%
to <code class="knitr inline">13</code>%)
more citations than similar studies without available data.  Date of publication, journal impact factor, journal citation half-life, journal size, number of authors, first and last author number of previous publications and citations, corresponding author country, institution citation mean score, and study topic were included as covariates.  A small independent investigation of citations to microarray studies with publicly available data found that about 
<code class="knitr inline">6</code>%
(95% CI: <code class="knitr inline">3</code>%
to <code class="knitr inline">11</code>%, 
n=<code class="knitr inline">138</code>)
of citations to those studies were in the context of data reuse attribution.

### Discussion
This analysis reveals a modest but substantiated boost in data citation rates across a wide selection of studies that made their data publicly available.  Though modest, the impact represented by these data attributions should not be underestimated: attribution in the context of data reuse demonstrates a real and demonstrable contribution to subsequent research.


<hr/>
<hr/>
<hr/>

# Additional analysis for reference during manuscript prep

(not currently configured to evaluate... )

<pre class="knitr"><div class="source">
<span class="symbol">myhetcorr</span> <span class="assignement">=</span> <span class="functioncall">hetcor.modified</span><span class="keyword">(</span><span class="symbol">dfCitationsAttributes</span><span class="keyword">,</span> <span class="argument">use</span><span class="argument">=</span><span class="string">"pairwise.complete.obs"</span><span class="keyword">,</span> <span class="argument">std.err</span><span class="argument">=</span><span class="number">FALSE</span><span class="keyword">,</span> <span class="argument">pd</span><span class="argument">=</span><span class="number">FALSE</span><span class="keyword">)</span>
<span class="symbol">mycor</span> <span class="assignement">=</span> <span class="symbol">myhetcorr</span><span class="keyword">$</span><span class="symbol">correlations</span>
<span class="functioncall">colnames</span><span class="keyword">(</span><span class="symbol">mycor</span><span class="keyword">)</span> <span class="assignement">=</span> <span class="functioncall">colnames</span><span class="keyword">(</span><span class="symbol">myhetcorr</span><span class="keyword">$</span><span class="symbol">correlations</span><span class="keyword">)</span>
<span class="functioncall">rownames</span><span class="keyword">(</span><span class="symbol">mycor</span><span class="keyword">)</span> <span class="assignement">=</span> <span class="functioncall">rownames</span><span class="keyword">(</span><span class="symbol">myhetcorr</span><span class="keyword">$</span><span class="symbol">correlations</span><span class="keyword">)</span>

<span class="comment"># Correlations with data availability</span>
<span class="comment">## See if anything is so collinear it will cause problems in regression</span>
<span class="symbol">a</span> <span class="assignement">=</span> <span class="functioncall">sort</span><span class="keyword">(</span><span class="symbol">mycor</span><span class="keyword">[</span><span class="keyword">,</span><span class="string">"dataset.in.geo.or.ae.int"</span><span class="keyword">]</span><span class="keyword">,</span> <span class="argument">dec</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span>
<span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">cbind</span><span class="keyword">(</span><span class="functioncall">names</span><span class="keyword">(</span><span class="symbol">a</span><span class="keyword">)</span><span class="keyword">,</span> <span class="functioncall">round</span><span class="keyword">(</span><span class="symbol">a</span><span class="keyword">,</span> <span class="number">2</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>

<span class="comment"># Correlations with citation</span>
<span class="symbol">a</span> <span class="assignement">=</span> <span class="functioncall">sort</span><span class="keyword">(</span><span class="symbol">mycor</span><span class="keyword">[</span><span class="keyword">,</span><span class="string">"nCitedBy.log"</span><span class="keyword">]</span><span class="keyword">,</span> <span class="argument">dec</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span>
<span class="functioncall">gfm_table</span><span class="keyword">(</span><span class="functioncall">cbind</span><span class="keyword">(</span><span class="functioncall">names</span><span class="keyword">(</span><span class="symbol">a</span><span class="keyword">)</span><span class="keyword">,</span> <span class="functioncall">round</span><span class="keyword">(</span><span class="symbol">a</span><span class="keyword">,</span> <span class="number">2</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>

<span class="symbol">univarate.citation.predictors</span> <span class="assignement">=</span> <span class="functioncall">which</span><span class="keyword">(</span><span class="functioncall">abs</span><span class="keyword">(</span><span class="symbol">mycor</span><span class="keyword">[</span><span class="keyword">,</span><span class="string">"nCitedBy.log"</span><span class="keyword">]</span><span class="keyword">)</span> <span class="keyword">&gt;</span> <span class="number">0.1</span><span class="keyword">)</span>
<span class="comment">#univarate.citation.predictors</span>
<span class="functioncall">length</span><span class="keyword">(</span><span class="symbol">univarate.citation.predictors</span><span class="keyword">)</span>
<span class="symbol">topcor</span> <span class="assignement">=</span> <span class="symbol">mycor</span><span class="keyword">[</span><span class="symbol">univarate.citation.predictors</span><span class="keyword">,</span> <span class="symbol">univarate.citation.predictors</span><span class="keyword">]</span>
</div></pre>

    
<pre class="knitr"><div class="source">
<span class="functioncall">heatmap.2</span><span class="keyword">(</span><span class="symbol">topcor</span><span class="keyword">,</span> <span class="argument">col</span><span class="argument">=</span><span class="functioncall">bluered</span><span class="keyword">(</span><span class="number">16</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">cexRow</span><span class="argument">=</span><span class="number">1</span><span class="keyword">,</span> <span class="argument">cexCol</span> <span class="argument">=</span> <span class="number">1</span><span class="keyword">,</span> <span class="argument">symm</span> <span class="argument">=</span> <span class="number">TRUE</span><span class="keyword">,</span> <span class="argument">dend</span> <span class="argument">=</span> <span class="string">"row"</span><span class="keyword">,</span> <span class="argument">trace</span> <span class="argument">=</span> <span class="string">"none"</span><span class="keyword">,</span> <span class="argument">main</span> <span class="argument">=</span> <span class="string">"Thesis Data"</span><span class="keyword">,</span> <span class="argument">margins</span><span class="argument">=</span><span class="functioncall">c</span><span class="keyword">(</span><span class="number">15</span><span class="keyword">,</span><span class="number">15</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">key</span><span class="argument">=</span><span class="number">FALSE</span><span class="keyword">,</span> <span class="argument">keysize</span><span class="argument">=</span><span class="number">0.1</span><span class="keyword">)</span>
</div></pre>


<pre class="knitr"><div class="source">
<span class="comment">##Other breakdowns</span>

<span class="symbol">num_authors_breaks</span> <span class="assignement">=</span> <span class="functioncall">c</span><span class="keyword">(</span><span class="number">1</span><span class="keyword">,</span> <span class="number">5</span><span class="keyword">,</span> <span class="number">10</span><span class="keyword">,</span> <span class="number">20</span><span class="keyword">,</span> <span class="number">40</span><span class="keyword">)</span>
<span class="functioncall">with</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">,</span> <span class="functioncall">tapply</span><span class="keyword">(</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="functioncall">cut</span><span class="keyword">(</span><span class="symbol">num.authors.tr</span><span class="keyword">,</span> <span class="symbol">num_authors_breaks</span><span class="keyword">)</span><span class="keyword">,</span> <span class="symbol">median</span><span class="keyword">,</span> <span class="argument">na.rm</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span><span class="keyword">)</span>

<span class="functioncall">qplot</span><span class="keyword">(</span><span class="symbol">num.authors.tr</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">data</span><span class="argument">=</span><span class="symbol">dat.subset</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_smooth</span><span class="keyword">(</span><span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="string">"black"</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_x_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">num_authors_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">num_authors_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="symbol">pubmed.is.core.clinical.journal</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>  <span class="keyword">+</span> <span class="functioncall">geom_jitter</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_boxplot</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="symbol">pubmed.is.open.access</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>  <span class="keyword">+</span> <span class="functioncall">geom_jitter</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_boxplot</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="symbol">pubmed.is.cancer</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>  <span class="keyword">+</span> <span class="functioncall">geom_jitter</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_boxplot</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="symbol">pubmed.is.humans</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>  <span class="keyword">+</span> <span class="functioncall">geom_jitter</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_boxplot</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="symbol">pubmed.is.cultured.cells</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>  <span class="keyword">+</span> <span class="functioncall">geom_jitter</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_boxplot</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="symbol">has.R.funding</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>  <span class="keyword">+</span> <span class="functioncall">geom_jitter</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_boxplot</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="functioncall">ggplot</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">,</span> <span class="functioncall">aes</span><span class="keyword">(</span><span class="symbol">country.usa</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">)</span>  <span class="keyword">+</span> <span class="functioncall">geom_jitter</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_boxplot</span><span class="keyword">(</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="functioncall">qplot</span><span class="keyword">(</span><span class="symbol">num.grants.via.nih.tr</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">data</span><span class="argument">=</span><span class="symbol">dat.subset</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_smooth</span><span class="keyword">(</span><span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="string">"black"</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="symbol">x_breaks</span> <span class="assignement">=</span> <span class="functioncall">quantile</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">$</span><span class="symbol">last.author.num.prev.microarray.creations.tr</span><span class="keyword">,</span> <span class="argument">na.rm</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span>
<span class="functioncall">qplot</span><span class="keyword">(</span><span class="symbol">last.author.num.prev.microarray.creations.tr</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">data</span><span class="argument">=</span><span class="symbol">dat.subset</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_smooth</span><span class="keyword">(</span><span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="string">"black"</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_x_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="symbol">x_breaks</span> <span class="assignement">=</span> <span class="functioncall">quantile</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">$</span><span class="symbol">first.author.num.prev.pubs.tr</span><span class="keyword">,</span> <span class="argument">na.rm</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span>
<span class="functioncall">qplot</span><span class="keyword">(</span><span class="symbol">first.author.num.prev.pubs.tr</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">data</span><span class="argument">=</span><span class="symbol">dat.subset</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_smooth</span><span class="keyword">(</span><span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="string">"black"</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_x_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="symbol">x_breaks</span> <span class="assignement">=</span> <span class="functioncall">quantile</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">$</span><span class="symbol">last.author.num.prev.pubs.tr</span><span class="keyword">,</span> <span class="argument">na.rm</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span>
<span class="functioncall">qplot</span><span class="keyword">(</span><span class="symbol">last.author.num.prev.pubs.tr</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">data</span><span class="argument">=</span><span class="symbol">dat.subset</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_smooth</span><span class="keyword">(</span><span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="string">"black"</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_x_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="symbol">x_breaks</span> <span class="assignement">=</span> <span class="functioncall">quantile</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">$</span><span class="symbol">last.author.num.prev.pmc.cites.tr</span><span class="keyword">,</span> <span class="argument">na.rm</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span>
<span class="functioncall">qplot</span><span class="keyword">(</span><span class="symbol">last.author.num.prev.pmc.cites.tr</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">data</span><span class="argument">=</span><span class="symbol">dat.subset</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_smooth</span><span class="keyword">(</span><span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="string">"black"</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_x_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>

<span class="symbol">x_breaks</span> <span class="assignement">=</span> <span class="functioncall">quantile</span><span class="keyword">(</span><span class="symbol">dat.subset</span><span class="keyword">$</span><span class="symbol">institution.mean.norm.citation.score</span><span class="keyword">,</span> <span class="argument">na.rm</span><span class="argument">=</span><span class="symbol">T</span><span class="keyword">)</span>
<span class="functioncall">qplot</span><span class="keyword">(</span><span class="symbol">institution.mean.norm.citation.score</span><span class="keyword">,</span> <span class="number">1</span><span class="keyword">+</span><span class="symbol">nCitedBy</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">data</span><span class="argument">=</span><span class="symbol">dat.subset</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">geom_smooth</span><span class="keyword">(</span><span class="functioncall">aes</span><span class="keyword">(</span><span class="argument">fill</span><span class="argument">=</span><span class="functioncall">factor</span><span class="keyword">(</span><span class="symbol">dataset.in.geo.or.ae</span><span class="keyword">)</span><span class="keyword">)</span><span class="keyword">,</span> <span class="argument">color</span><span class="argument">=</span><span class="string">"black"</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_x_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">x_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="functioncall">scale_y_continuous</span><span class="keyword">(</span><span class="argument">trans</span><span class="argument">=</span><span class="string">"log10"</span><span class="keyword">,</span> <span class="argument">breaks</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">,</span> <span class="argument">labels</span><span class="argument">=</span><span class="symbol">citation_breaks</span><span class="keyword">)</span> <span class="keyword">+</span> <span class="symbol">cbgFillPalette</span> <span class="keyword">+</span> <span class="symbol">cbgColourPalette</span>
</div></pre>



