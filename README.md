# Analysis of liver and kidney microarrays of Ahr-null or wild-type C57BL/6 mice ravaged with TCDD or vehicle

This repo contains my reproduction of the liver and kidney Affymetrix microarray analysis found in the paper by [Boutros *et al*, 2009](https://pubmed.ncbi.nlm.nih.gov/19759094/) [[1]](#1).

2,3,7,8-tetrachlorodibenzo-*p*-dioxin (TCDD) is a "forever" chemical and byproduct of industrial chemical synthesis and incomplete combustion. As described in Boutros *et al.*, C57BL/6 mice with or without a knocked-out *Ahr* (*Ahr*-null or wild-type, respectively) were gavaged with 1000 µg/kg TCDD or vehicle. 19 hours later, the mice were sacrificed and their liver and kidneys were harvested. Although the authors say in their Methods the livers and kidneys were collected from the same mice, I was not able to find animal identifiers, indicating if a liver microarray from a certain mouse matched to a kidney microarray and vice versa.

Firstly, for determining differential expression, the liver and kidney data were considered separately. Then, considering all samples from each tissue, a general linear model was fitted using the R package `limma`[[2]](#2):

$$Y = Basal + AhR + TCDD + AhR:TCDD$$

Where $Y$ refers to a ProbeSet's expression level, $Basal$ refers to the basal gene expression of a ProbeSet, $AhR$ refers to the animal's *Ahr* genotype, and $TCDD$ refers to whether the animals was gavaged with 1000 µg/kg TCDD or vehicle. The final term, $AhR:TCDD$, refers to an effect on a ProbeSet's expression level explained by the interaction of TCDD and AhR, which makes sense since TCDD selectively binds and activates the AhR, promoting gene regulation. The linear model was fit separately for each organ's 45,101 unique ProbeSets and for each organ, kidney and liver. After fitting the model to each Probeset, each coefficient's p-value (for $AhR$, $TCDD$, and $AhR:TCDD$) were adjusted by the Benjamanini-Hochberg False Discovery Ratio method. This contrasts with their cited Bayesian-FDR method. Although this leads to differences in the exact number of significantly regulated ProbeSets in the liver and kidney as as response to *Ahr* genotype or TCDD-AhR interactions, my non-Bayesian approach produces roughly similar results.

In the R Markdown folder, the html output from knitting the Rmd file may be opened in your browser of choice. The analysis is mostly un-novel and is meant to follow the figures and tables in the Boutros *et al.* paper, except for the functional enrichment analysis of subsets of unique differential Gene Symbols (Gene Symbols are 1:n with ProbeSets). 

Upcoming Analysis:
- PLIER?

## References
<a id="1">[1]</a> 
Boutros, P. C., Bielefeld, K. A., Pohjanvirta, R. & Harper, P. A. (2009) "Dioxin-dependent and dioxin-independent gene batteries: Comparison of liver and kidney in AHR-null mice." Toxicol. Sci. 112, 245–256. doi: 10.1093/toxsci/kfp191.

<a id="2">[2]</a> 
Ritchie ME, Phipson B, Wu D, Hu Y, Law CW, Shi W, Smyth GK (2015). “limma powers differential expression analyses for RNA-sequencing and microarray studies.” Nucleic Acids Research, 43(7), e47. doi: 10.1093/nar/gkv007.

