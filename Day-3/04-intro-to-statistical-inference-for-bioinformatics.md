# Introduction to Statistical Learning & Inference in bioinformatics

As we discussed on day 1, bioinformatics draws on knowledge from multiple disciplines. To effectively solve most bioinformatic problems, knowledge from each of these disciplines must be applied to address the overall problem. Developing a **working knowledge of statistics** is critical for anyone hoping to solve bioinformatic problems, particularly in genomics.

In particular, this working knowledge of statistics is required to understand data analysis after data reduction of specific data types, e.g. performing a differential expression analysis.


<p align="center">
  <img src="../figures/bioinfo-venn.png" height="400" width="400"/>
</p>

Adapted from Md Arman Hossen on [Medium](https://medium.com/datadriveninvestor/i-have-designed-my-own-bioinformatics-degree-260b24767d87).

Statistics involves the analysis of numerical data that we wish to use for making inferences on a larger population. Generally, ***statistical learning*** refers to the models and procedures with which we analyze these numerical datasets.

These models and procedures can be used to either make **predictions** (e.g. healthy vs. diseased tissue based on gene expression profiles) or for ***statistical inference***, where tools from statistical learning are applied to help us understand the relationship between a set of variables (e.g. which genes are associated with diseased tissue).

While a comprehensive introduction to *statistical learning and inference* is well beyond the scope of this workshop (and generally takes years of specific training and experience), we will introduce some fundamental concepts required for approriate statsitcal analysis of genomics data sets that are relevant to wet- or dry-lab scientists alike.

**What we *will* cover:**  
- What is statistical learning?
- Basics of supervised & unsupervised data analysis methods
- Fundamentals of statistical inference & hypothesis testing
- *P*-values & multiple testing correction

**What we *will not* cover:**  
- Math & probability theory underlying statistical learning procedures
- Model selection procedures & how to assess model fit
- A comprehensive introduction to the methods used for statistical learning and inference in bioinformatics

> **Important note:** Builing a more complete understanding of the statistical procedures commonly used in bioinformatics, such that you are able to confidently implement, interpret, and troubleshoot these procedures, requires a strong working knowledge of relevant math and probability theory. Such training is best sought out through formal instruction, and is usually not included in applied bioinformatics courses. While developing an understanding of the fundamental concepts in statistical learning and inference covered here will allow you to begin leveraging more complex statistical analyses in your own research, and act as a solid fondation upon which to further your training in this domain, it is also important to recognize when more specialist expertise is needed to address your analytical questions.

---

## Statistical learning

As described above, *statistical learning* describes the models and procedures we use in order to understand data. Generally, the methods we use can be categorized into *supervised* and *unsupervised* methods.

**Supervised methods** describe approaches used when a set of observations we have made (e.g. gene expression levels) are all associated with a response variable (e.g. diseased or healthy). The observations, also called predictors, are generally referred to as the *independent variable (X)*, while the response is the *dependant variable (Y)*.

By fitting statistical models to the data (the *learning* part), we aim to learn about the relationship between the *independent* and *dependent* variables (the *inference* part). For example, we may apply a form of linear model to gene expression data from heathly cases and diseased controls in order to address questions like:  
- *Which genes are associated with disease?*
- *How much does a given gene contribute to the disease phenotype?*

**Examples of supervised methods:**   
- Linear regression
- Generalized linear regression models
- Descision trees
- Support vector machines

**Unsupervised methods**

There are times when observations are not associated with a predictor (the *dependent variable*) and we simply wish to the relationships that exist in our data in a way that is *not supervised* by any such variable. This is often true in *exploratory data analysis*, where we want to explore how samples are related to each other without assigning samples to a specific group for modeling purposes.

Alternatively, we may not have a dependent variable that can be used to model our observations. For example, in analysis of single cell sequencing data, we are often interested in studying subpopulations of cells that come from the same sample, and therefore require some way assessing similarities and differences between the cells so that wse can identify potential subpopulations of interest.  

**Examples of unsupervised methods:**    
- Dimensionality reduction (e.g. PCA, NMF, t-SNE, UMAP)
- Clustering methods (e.g. hierachical, K-means)
- Hidden markov modeling

Below, we provide more specific introductions to both supervised and unsupervised learnings, using basic linear modeling as an example for supervised approaches, while exploring PCA and hierachical clustering for unsupervised analysis.

> A more comprehensive introduction to statistical learning can be found in the book: [An Introduction to Statistical Learning](http://faculty.marshall.usc.edu/gareth-james/ISL/).










## Statistical inference

statsitical inference refers to:
estimation
hypothesis testing



### What is hypothesis testing?

In bioinformatic data analysis, we often want to test a hypothesis such that we can assess how meaningful our results are. For example, in an RNA-seq differential expression analysis we would like to know which genes showed meaningfully different gene expression between the experimental groups. *Hypothesis testing* describes the statistical framework that we use to assess how meaningful our results are.

General procedure for hypothesis testing:
1. Decide on the null hypothesis (*H<sub>0*)
2. Use the sample data to calculate a test statistic
3. Use the test-statistic to calculate a P-value
4. Accept or reject the null hypothesis based on magnitude of P-value

To help understand the procedure for hypothesis tetsing, some useful definitions:
- **Null hypothesis (*H<sub>0*)** - the hypothesis that there is *no meaningful difference* between our samples
- **alternative hypothesis (*H<sub>A*)** there is a meaningful difference* between our samples
- **test-statistic** - quantity comparing your results to those you would expect under the null hypothesis
- **P-value** - probability that we would observe a test-statistic as extreme as the one we observed, simply due to chance

Through comparing our observed data to that expected under the *null hypothesis* (*H<sub>0*) we can calculate the *test-statistic* using a distribution of known area, such as the t-distribution. Since these distributions are of known area, we can calculate a probability value (*P*-value) that tells us how likely we are to observe a test-statistic this big due to chance.

<p align="center">
  <img src="../figures/t-dist.png" height="410" width="650"/>
</p>




Larger test-statistic values mean there is a larger difference between the distribution of our observed data, and the distribution of data under the null hypothesis. For the t-distribution, we test-statistic is calculated using the following equation:

*t-statistic* = (*x<sub>i</sub>* - &mu;) / (*s* x sqrt(*n*))

Using the t-statistic and the *degrees of freedom* (sample number - 1) the P-value is determined. If the P-value is < our threshold (&alpha;), we will reject the null and accept the alternative hypothesis. By setting &alpha; to 0.05, this means we will interpret results as meaningful if they have at most a 5 in 100 probability of being due to chance.

<p align="center">
  <img src="../figures/t-dist2.png" height="410" width="650"/>
</p>


> **P-value thresholds:** Although 5% is a commonly used P-value threshold, you can be more or less stringent depending on the nature of your experiment: if you want to be very conservative and restrict your results to few results that are likely to be true positives, you may wish to restrict the results to a more stringent threshold. If your experiment is very preliminary and you care less about capturing some false positives than missing true positives, you may wish to relax your threshold.  



### The multiple testing problem

In bioinformatics and genomics, we measure thousands upon thousands on features (e.g. genes, peaks, methylation sites) and we often run a statistical test for each of them when trying to identify meaningful features with regard to our hypothesis. As we defined above, P-values are *the probability that we would observe a result as extreme as the one we observed, simply due to chance*. Therefore, if we use 0.05 as a P-value threshold, and test 20,000 features for statistical significance, by definition 5% of those features will have a test-statistic that big simply due to chance.

Consider an typical RNA-seq experiment, where it is common to test >20,000 genes for differential expression across two or more conditions. If we set 5% as our &alpha;, 5% of the fold-changes we observe will be sufficiently different between the experimental groups due to chance. **5% of 20,000 is 1000 genes**, which is obviously an unacceptable amount of false-positives.


|         |           | truth                         |                            |
|---------|-----------|-------------------------------|----------------------------|
|         |           | H0 (null) is true             | HA alternative) is true    |
| descion | Accept H0 | True negative (P = 1 - alpha) | False-negative (P = beta)  |
|         | Reject H0 | False positive (P = alpha)    | True-positive (P = 1-beta) |


We can demonstrate the multiple testing problem by simulating some very simple data and running statistical tests repeatedly on them.


```r

```

```r
# how many P-values < 0.05
sum(res2$pvalue < 0.05, na.rm=TRUE)

# how many FDR adjusted P-values < 0.05
sum(res2$padj < 0.05, na.rm=TRUE)

# how many with Bonferonni adjusted P-values < 0.05
sum(res2$pvalue < (0.05/nrow(res2)), na.rm=TRUE)

# plot the results
plot(res2$log2FoldChange, -log10(res2$pvalue),
     main = "Volcano plot - DEG w/ scrambled sample labes",
     las = 1, col = "cornflowerblue",
     ylab = "- log10 P-value", xlab = "log2 Fold change", ylim = c(0,7))

# add significance lines
abline(h= -log10(0.05), lty = 2, col = "red") # nominal P-value
abline(h= -log10(0.05/nrow(res2)), lty = 2, col = "black") # Bonferonni
```

As you can see...


### Methods for multiple testing correction

We address this problem through *multiple testing correction*. While several methods that control different aspects of the multiple testing problem, we commonly use methods that control the false-discovery rate (FDR) in genomics experiments. Controlling the false discovery rate at 10% means that we are accepting that 1 in 10 of the features with a significant adjusted P-value, is actually a false-positive. However if your experiment requires more stringency, you may wish to use a method that controls the family-wise error rate (FWER), such as **Bonferonni** correction.


Bonferonni -

False discovery rate -


Many software package will calculate P-values adjusted for multiple testing for you, however you can always calculate them yourself in R, using a vector of the raw P-values.

```r
# bonferroni correction
p.adj.bonf <- p.adjust(pvalue, method = "FWER")

# FDR correction
p.adj.fdr <- p.adjust(pvalue, method = "fdr")
```




```r
# plot corrected P-values

```


You can see that there are **minimal results with statistical signficance after correction**, which is true since we scrambled the sample labels and created a fake dataset that should have no true DE. However, if we used the unadjusted P-values, we would identify **A LOT** of potentially interesting genes, that would in-fact be **false-positives**.

This example highlights the short coming of hypothesis testing approaches, and demonstrates how important it is to correct for multiple hypothesis testing.


link










### Supervised learning - Linear modeling

Simple linear models, or linear regression, is used pervasively in bioinformatics and genomics for statistical inference. Linear models are relatively simple, flexible, and interpretable, meaning they make excellent tools for statistical inference and scale well to thousands of observations, which is critical for common genomics datasets. Example applications of linear models include:  
- RNA-seq (differential expression)
- ChIP-seq (differential binding)
- ATAC-seq (differential accessibility)
- Microarray analysis (e.g. DNA methylation)
- Variant identification (WES/WGS/RNA-seq)
- Genome-wide association studies (GWAS)

Understanding the basics of linear modeling is central to being able to perform these types of analyses in a statistical programming environment such as R.

Given their importance and pervasive use in bioinformatics and genomics, we will introduce the fundamental concepts of linear models, and how you can fit these models in R.

> **Note:** Linear modeling is the topic of entire independent courses and again requires knowledge of appropriate mathematics and proprability to understand completely. Thus, this should be considered an introduction rather than a standalone resurce.

In a standard linear model, we assume that some *response* variable (*Y*) can be represented as a linear combination of a set of *predictors* (*X*, independent variables). In building a linear model, we estimate a set of *coefficients* that explain how the *predictors* are related to the *response*. We can use these *coefficients* for statistical inference to better understand which predictors are associated with our response, or for applying the model to new data where we wish to predict the response variable using only a set of predictors.

Before reviewing out the statistical notation for a simple linear model, it can be useful to first consider the main components:
`*response* = *predictor(s)* + *error*`

- **The *response*** is the dependent variable we wish to model based on some set of predictors  

- **The *predictor(s)*** is the independent variable, or variables, that we wish to model as a linear combination of the response (this is usually what we are interested in for statistical inference and hypothesis testing)  

- **The *error*** component represents the information not explained by the model, and exists since we know that no set of predictors will perfectly explain the response variable. These are often referred to as the *residuals* of the model.  

Using the statistical notation for a simple linear regression model:

Y = &beta;<sub>0</sub> +  &beta;<sub>i</sub> X<sub>i</sub> + &epsilon;

- Y is a continuous response variable that we assume is normally distributed
- &beta;<sub>i</sub> are the coefficients to be estimated (&beta;<sub>i</sub>-value)
- X<sub>i</sub> are the predictors
- &beta;<sub>0</sub> refers to the model intercept
- &epsilon; refers to the error term (residuals) and are assumed to follow a normal distribution

There can be any (reasonable) number of predictors (X) in a model, and predictors can be either *continuous* (e.g. age) or categorical (e.g. treatment group, batch).

Each predictor is associated with a coefficient that describes the relationship of that predictor to the response variable. In the context of linear regression, the coefficient is also referred to as the *slope*.

In R, the basic syntax for this model is: `lm(response ~ predictor)`. Lets simulate some data that we can use to illustrate the theory described above and fit out first linear model in R.

```r
# read in the example data
dat <- read.csv("lm-example-data.csv", stringsAsFactors=FALSE)

# explore it quickly
head(dat)
str(dat)

# plot
plot(dat$gene_exp ~ dat$hba1c,
	ylab = "Expression (Gene X)",
	xlab = "Hba1c score",
	main = "Gene X exp. vs Hba1c",
	col = "indianred", pch = 16, las = 1)

# fit a linear model with gene expression as the response
lm1 <- lm(dat$gene_exp ~ dat$hba1c)
lm1
```
<img src="../figures/lm_example-0.png" height="500" width="550"/>

The coefficient for the independent/predictor variable, Hba1c, describes its relation to the response variable, expression of gene X. Here, the coefficient is telling us that *for every 1 unit increase in gene expression measured, Hba1c levels increase by ~0.96 units*.

This is basic *statistical inference*, as we have used this procedure to model the relationship between two variables, and *infer* something about how those variables are related.

To help us better understand the model, we can plot the regression line on our scatterplot.
```r
# generate plot again
plot(dat$gene_exp ~ dat$hba1c,
	ylab = "Expression (Gene X)",
	xlab = "Hba1c score",
	main = "Gene X exp. vs Hba1c",
	col = "indianred", pch = 16, las = 1)

# add the model on the scatterplot
abline(lm1, lty=2)

# calculate the predicted gene expression values using the model
pre <- predict(lm1)

# plot the difference between the predicted and the true values
segments(dat$hba1c, dat$gene_exp, dat$hba1c, pre,
	col="cornflowerblue")
#### Note: These are the residuals!
```
<img src="../figures/lm_example.png" height="500" width="550"/>

The regression line (shown in black) illustrates the clear linear relationship between expression of gene X and Hba1c levels.

The residuals (blue lines) describe how far away each observation (the gene expression values) are from the predicted values from the linear model. All observations are close to the regression line, suggesting the model is a good fit for the data.

**However**, by virtue of this being a statistical model, all coefficients are estimated with some level of uncertainty. If the model is a poor fit for the data, there will be a high uncertainty in the coefficient.

One way to evaluate how much meaning we should attribute to the coefficient, is to calculate a *P*-value for it through hypothesis testing, which we will explore below.

> **Note:** Although standard models for modeling gene expression data would include expression values as the response variable, these models usually take on a more complicated form (see note on *Generalized linear models* below), however we have set up a simple model for teaching purposes.

#### Hypothesis testing with linear models

In order to test how much certainty we have for a particular coefficient from a linear model, we estimate a quantity called **the standard error (SE)**. Without discussing the underlying stastics that define it, the SE is essentially a *measure of certainty around the coefficient*, and is dependent on the variance of the residuals (&epsilon;).

Importantly, the SE can be used to perform **hypothesis testing** to determine if the coefficient is statistically significant. In this case, we can test the null hypothesis that the coefficient is equal to zero, using the following equation to calculate the *t-score*:

*t-score* = &beta;<sub>i</sub> - 0 / SE(&beta;<sub>i</sub>)

The *t-score* can then be used to calculate a *P*-value, as described in the hypothesis testing section. In R, the `summary()` function will test all model coefficients against the null hypothesis:
```r
sum_lm1 <- summary(lm1)
sum_lm1

# get the coefficients table
coef(sum_lm1)

# get the coefficients themselves
coef(sum_lm1)[,1]

# get the P-value for the hba1c coefficient
coef(sum_lm1)[2,4]
```

The *P*-value is very small, so we can reject the null, and conclude that Hba1c levels are associated with expression of gene X, and interpret the coefficient as a meaningful quantity.

If the *P*-value does not pass the *a priori* significance threshold for your analysis, the coefficient should be ignored as that predcitor is **not associated** with the response variable.

You can always confirm by looking at the slope in a simple linear model. To demonstrate this, explore the example below for Gene Y and its relation to Hba1c levels.
```r
# read in the example data
dat2 <- read.csv("lm-example-data-geneY.csv", stringsAsFactors=FALSE)

# plot
plot(dat2$gene_exp ~ dat2$hba1c,
	ylab = "Expression (Gene Y)",
	xlab = "Hba1c score",
	main = "Gene Y exp. vs Hba1c",
	col = "indianred", pch = 16, las = 1)

# fit a linear model with gene expression as the response
lm1 <- lm(dat2$gene_exp ~ dat2$hba1c)
summary(lm1)

# add the model on the scatterplot
abline(lm1, lty=2)

# plot the difference between the predicted and the true values
segments(dat2$hba1c, dat2$gene_exp, dat2$hba1c, pre, col="cornflowerblue")

```

<img src="../figures/lm_example-2.png" height="500" width="550"/>


The flatter slope of the regression line, and larger values of the residuals, suggests there is no useful relationship between Hba1c levels and expression of gene Y, which is supported by the large *P*-value returned by the model.


#### Simple Linear modeling with categorical variables

In genomics, we commonly have categorial predictor variables, in contrast to the continuous variable (Hba1c) from our example above. Example of categorial variable include:
- Wild-type vs knockout
- Vehicle vs treatment
- Control vs diseased

Importantly, linear models are capable of incorportaing categorical variables as predictors. Lets consider another example, where we have gene expression levels for gene X measured in 20 healthy tissues, and 20 diseased tissues, and we wish to use a linear model to explore the relationship between gene expression and disease status.

```r
# read in the example data
dat3 <- read.csv("lm-example-3.csv", stringsAsFactors=FALSE, row.names = 1)

# quickly explore it
head(dat3)
table(dat3$subject_group)
# Note: Controls are coded as 0, cases are coded as 1

# visualize the data
plot(dat3$subject_group, dat3$exp_geneX,
	ylab = "Expression (Gene X)",
	xlab = "Subject group",
	main = "Gene X exp. vs Hba1c",
	col = "indianred", pch = 16, las = 1)


# run the linear model and evaluate
lm_2 <- lm(dat3$exp_geneX ~ dat3$subject_group)
summary(lm_2)

# add regression line to the plot
abline(lm_2, lty=2)
```
<img src="../figures/lm_example-3.png" height="500" width="550"/>

Looking at the model output, the *P*-value is very small, therefore we can conclude that there is an association between expression of gene X and disease status in this sample.

Again, the coefficient tells us about the relationship between the predictor and the response. The coefficient for the predictor `subject_group` tells us that for each unit increase in this variable, there is an increase of 11.2 expression units for gene X.

Since a 'unit increase' in `subject_group` simply means controls vs diseased subjects, we can interpret this as the difference in expression between controls and cases. This is analgous to how we would calculate a fold-change value in an RNA-seq analysis.

---
#### Multiple regression

We could have simply addressed the above analysis using a more simple statistical test such as a *t-test*. However, we commonly want to include additional variables in our linear models, and approaches such as the t-test cannot handle this scenario.

For example, we might want to control for factors that could confound gene expression differences between the control and diseased groups. For example, we could control for age and sex of the subjects, or perhaps the batch the samples were collected in.

In this scenario, we can use linear models to control for the additional variables by adding them into the statsitical model e.g.

*Just an example do not run this code*
```r
lm(dat3$exp_geneX ~ dat3$subject_group + dat3$age + dat3$gender + dat3$batch)
```

This approach is referred to as **multiple regression**. If you will be doing any sort of complex bioinformatics data analysis involving linear models, I strongly encourage you to use this primer as a starting point to learn more about multiple regression and more complex linear modeling scenarios.  

---

#### Generalized linear models

Commonly in bioinformatics, we find ourselves needing a model that can assume a different statistical distribution than the normal. The rationale behind this is that many bioinformatics and genomic data types exhibit specific distributions that are different from the normal.

In these cases we fit the data using a **generalized linear model (GLM)**. GLM's are a family of statistical models that generalize standard linear regression in two ways:  
- use of probability distributions other than the normal distribution
- the use of a *link-function* that links the expression values in the linear model to the experimental groups, in a way that these other distributions can be used.

<p align="center">
<img src="../figures/neg-binom.png" title="xxxx" alt="context"
	width="75%" height="75%" />
</p>

For example, bulk RNA-seq data typically exhibit a distribution referred to as the *negative-binomial* and therefore require a GLM of the *negative-binomial family* in order to appropriately model RNA-seq counts and test them for differential expression.

While GLMs are beyond the scope of this workshop, and we simply do not have the time to cover them in this short course, we do cover the fundamentals of how GLMs are used in the context of RNA-seq data analysis in our RNA-seq workshop!



### Unsupervised learning - Dimension reduction & clustering

As discussed earlier in this lesson, there are times when we wish to simply explore the relationships between samples in our dataset in a way that is not supervised by any dependent variable (e.g. tx group, WT vs KO). This approach is commonly referred to as exploratory data analysis, and is used to answer such questions as:
- How similar are my samples based on genome-wide profiles?
- Are any samples in the dataset clear outliers?
- Do any variables systematically affect genome-wide profiles (e.g. batch)?

Unsupervised learning methods are very powerful tools to conduct exploratory data analysis. Two important groups of unsupervised methods include *dimensionality reduction methods* and *clustering-based methods*.

- ***dimensionality reduction methods***: involves the transformation of data from a high-dimensional space to a low-dimensional space, allowing intrinsic properties of the high-dimensional dataset to be identified

- ***clustering-based methods***: involves calculation of the similarity/dissimilarity between samples, resulting in organization of these samples into *'clusters'* defined by their relatedness to one-another

We will explore an example of each approach below, using *principal components analysis (PCA)* as an example of dimensionality reduction, and *unsupervised hierachical clustering* as an example of a clustering-based method.

#### Principal components analysis (PCA)

For high dimensional datasets (e.g. genomics data), where we have measured many thousands of features over a number of samples, it is often desirable to reduce the complexity of the dataset so that it can be viewed in fewer dimensions that hopefully reveal some intrinsic properties of the dataset.

> We will not discuss the mathematical procedures used to perform PCA here, however this is discussed exhaustively elsewhere. One excellent resource is the StatQuest video linked [here](https://www.youtube.com/watch?v=_UVHneBUBW0&ab_channel=StatQuestwithJoshStarmer).

PCA is a very popular approach to perform dimensionality reduction. At its simplest level, PCA accepts a matrix of numerical values (e.g. gene expression matrix, ChIP-seq counts, etc.), and returns a set of numerical vectors (principal components) that represent the axes of greatest variation in the dataset.

The principal components (PCs) explain distinct sources of variation in the data, and represent the lower-dimensional space that the original dataset has been projected into. Importantly, each PC explains more variation in the dataset than the last (e.g. PC1 explains more variation than PC2).

By using the projection values of each sample along the PCs, we can visualize this lower-dimensional space to learn defining properties of the dataset.

<p align="center">
  <img src="../figures/pca-example.png" height="410" width="650"/>
</p>

To demonstrate how you would perform a PCA analysis in R, we will use gene expression data (RNA-seq) collected as part of the same ENCODE study we discussed in previous lessons [Gorkin et al, 2020](https://www.nature.com/articles/s41586-020-2093-3?proof=t), but published in the companion paper: [*The changing mouse embryo transcriptome at whole tissue and single-cell resolution*](https://www.nature.com/articles/s41586-020-2536-x).

The expression data includes samples collected as multiple time-points during development of the mouse embryo across 17 different tissues and organs (see Figure 1A from the manuscript below).

<p align="center">
  <img src="../figures/fig-1a-he.png" height="70%" width="70%"/>
</p>

To help get you started, you have been provided with a matrix of FPKM counts (*fragments per kilobase million mapped reads*, which represent normalized expression values) and sample metadata. In the code chunks below, we will explore the relationships between samples from different tissues using PCA.

```r
# read in data
fpkm <- read.table("fpkm_sub.txt", stringsAsFactors = FALSE, header=TRUE)
meta <- read.delim("metadata_sub.tsv", stringsAsFactors = FALSE, header = TRUE)

# have a quick look at top of files
head(fpkm[,1:6])
head(meta)

# log transform fpkm counts
log_fpkm <- log2(fpkm+1)

# calculate variance across samples for each gene
vars <- apply(log_fpkm, 1, var)
# order variances based on magnitude of variance
vars <- rev(vars[order(vars)])

# plot variance for genes accross samples
plot(vars, las = 1, main="Sample gene expression variance", xlab = "Gene", ylab = "Variance")
abline(v=5000, col="red")
```

<p align="left">
  <img src="../figures/var-pca.png" height=height="60%" width="60%"/>
</p>

In-fact, most genes show little variance in expression levels across tissues. Features that do not vary across samples are not informative for dimensionality reduction or clustering methods, therefore it is generally useful to remove them.

In these data, it seems that ~ the top 5000 genes will explain most of the variance in these data, so we will only use these genes for the PCA.

```r
# perform PCA and order by variance
vars_sub <- vars[1:5000]

# perform the PCA on the fpkm matrix
pca <- prcomp(t(log_fpkm[names(vars_sub), ]))

# look at the object returned
str(pca)
head(pca$x)

# construct data frame w/ PC loadings and add sample labels
pca_df <- as.data.frame(pca$x)
pca_df$tissue <- as.factor(meta$Biosample.term.name)
pca_df$sample_ids <- meta$File.accession

# extract percent variance explained by each PC
percentVar <- pca$sdev^2/sum(pca$sdev^2)

# add colors for plotting to df
cols <- grDevices::rainbow(length(levels(pca_df$tissue)))

# create an empty variable in pca_df to be filled with colors from cols
pca_df$col <- NA

# loop over tissue types in pca_df and assign colors for plotting
for(i in 1:length(levels(pca_df$tissue))){
  ind1 <- which(pca_df$tissue == levels(pca_df$tissue)[i])
  pca_df$col[ind1] <- cols[i]
}

# plot PC1 vs PC2
plot(pca_df[, 1], pca_df[, 2],
     xlab = paste0("PC1 (", (round(percentVar[1], digits=3)*100), "% variance)"),
     ylab = paste0("PC2 (", (round(percentVar[2], digits=3)*100), "% variance)"),
     main = paste0("PC1 vs PC2 genome-wide RNA-seq tissue profiles"),
     pch = 16, cex = 1.35, cex.lab = 1.3, cex.axis = 1.15, las = 1,
     panel.first = grid(),
     col = pca_df$col)

# add a legend to the plot
legend(1.5, 105, levels(pca_df$tissue), pch = 16, col = cols, cex = 0.9)
```

<p align="center">
  <img src="../figures/pca-plot.png" height="75%" width="75%"/>
</p>

Viewing the dataset using this lower dimensional representation provides us with critical insights into the data that would be too challenging to obtain by looking at the expression levels of individual genes separately. We see that samples from similar tissues generally cluster together on the PCA plot, while more distinct tissue types appear further away from each other. This fits with our expectations, as samples from the same (or similar tissues) should have similar gene expression profiles.


#### Unsupervised hierarchical clustering

Generally, clustering analysis describes a collection of methods used to group samples into groups called *clusters* which define their relation to one-another. These relationships are defined by the *distance metric* used, which is simply a measure of how similar/dissimilar samples or features in a dataset are to each other. Several distance metrics exist, such as *manhattan* or *euclidean* distance, and are calculated differently (and affect the clustering).

Unsupervised hierarchical clustering describes a specific approach to performing clustering analysis, in which a tree-like structure is generated to describe the relatedness of samples and features. The results of this procedure are commonly represented using heatmaps, with samples defining columns and features (e.g. genes) defining the rows. Visualization using heatmaps is valuable as it allows us to identify *'modules'* of similar/dissimilar features across samples, making approaches like hierarchical clustering complimentary to dimensionality-reductions methods like PCA.

<p align="center">
  <img src="../figures/heatmap-exp.png" height="90%" width="90%"/>
</p>

To demonstrate how one could perform an unsupervised hierachical clustering analysis in R, we will use the same RNA-seq dataset from [He *et al*](https://www.nature.com/articles/s41586-020-2536-x), describing transcriptomic changes in the developing mouse embryo.

Clustering can be quite computationally intensive, therefore we will first generate a subset of the gene expression data containing five specific tissues, rather than all 17.
```r
# subset mnetadata to 5 tissues of interest
meta_ord <- meta[meta$Biosample.term.name=="forebrain" |
                 meta$Biosample.term.name=="heart" |
                 meta$Biosample.term.name=="limb" |
                 meta$Biosample.term.name=="liver" |
                 meta$Biosample.term.name=="intestine", ]

# subset FPKM matrix to contain the same subset of samples
log_fpkm_sub <- log_fpkm[, c(colnames(log_fpkm) %in%  meta_ord$File.accession)]
```

Since we took a subset of the original dataset, we need to recalculate the variance of each gene across the samples in this subset, so that we can assess how many features will be informative for the clustering procedure.
```r
# calculate variance of each gene across samples for new subset of data
vars <- apply(log_fpkm_sub, 1, var)

# order variances based on magnitude of variance
vars <- rev(vars[order(vars)])

# plot variance for genes across samples
plot(vars, las = 1, main="Sample gene expression variance",
     xlab = "Gene", ylab = "Variance")
# add vertical line
abline(v=1000, col="red")
```

<p align="left">
  <img src="../figures/var-hclust.png" height="60%" width="60%"/>
</p>


The per gene variances look similar to before, however we will focus on the top 2000 most variable genes to help speed up the hierarchical clustering.
```r
# subset var to only top 2000 genes with most variance
vars_sub <- vars[1:2000]

# subset the fpkm matrix to these genes
mat <- log_fpkm_sub[names(vars_sub), ]

# order the samples in the same order they are in in the metadata file
mat <- mat[, c(match(meta_ord$File.accession, colnames(mat)))]

# scale the fpkm matrix by row
mat_scaled = t(apply(mat, 1, scale))

# set column names for this matrix (they were removed during transposition)
colnames(mat_scaled) <- colnames(mat)
```

We will be using the *pheatmap()* from the `pheatmap` package, which provides extensive functionality for performing and visualizing the results of clustering analyses using heatmaps.
```r
# load the pheatmap package
library(pheatmap)

# create data frame to annotate heatmap with
annotation_col = data.frame(Tissue = meta_ord$Biosample.term.name)
rownames(annotation_col) = meta_ord$File.accession

# use pheatmap() to perform clustering on scaled data matrix
pheatmap(mat_scaled,
         show_rownames=FALSE, show_colnames=FALSE,
         annotation_col = annotation_col,
         cluster_cols = TRUE,
         clustering_method = "average",
         clustering_distance_cols = "correlation")
```

<p align="center">
  <img src="../figures/hclust.png" height="80%" width="80%"/>
</p>

The sample dendrogram and annotation bar tells us that all five tissues were assigned to their own cluster, with sub-clusters among each. It also allows us to compare between these groups to make inferences, for example, liver seems to be the most dissimilar tissue from forebrain based on gene expression.

In addition, the heatmap shows us the genes and their accompanying dendrogram, which allows us to visualize sets of genes that show coordinated changes across the samples, providing insight into the differences described by the clusters. For example, there is a set of ~200-300 genes that are uniquely up-regulated in intestinal tissue, and an additional ~100-200 genes that are shared with liver tissue.  
