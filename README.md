# A mechanism-aware and multi-omic machine learning pipeline characterises yeast cell growth

## Objective 

This code uses strain-specific metabolic modelling coupled with gene expression data to produce predictive models for yeast growth.


## Workflow

**Step one:**

The msb145172 dataset (Duibhir et al., 2014) is used to create strain-specific metabolic models through tailoring microarray-based constraints on a baseline metabolic model. 

**Step two:**

Norm-2 regularised flux balance analysis is utilised to simulate the metabolic activity and obtain strain-specific reaction fluxes.

**Step three:**

Feature selection techniques are applied. We explore three state-of-the-art techniques:

* NSGA-II - a genetic algorithm that allows for multiple objectives
* Sparse group lasso - a regularisation technique for grouped variables which promotes group sparsity
* Iterative random forest - accounting for non-linear interations among variables

**Step four:**

Machine learning and deep learning techniques are applied to produce predictive models for yeast growth, starting from gene expression, metabolic flux and both together. We use early, intermediate and late data integration, exploring the following baseline methods: 

* Support vector regression
* Random forest 
* Deep neural networks 

Their multi-view counterparts are:

* Bayesian efficient multiple kernel learning
* Bagged random forest
* Multi-modal deep neural networks


## Files and requirements

### Metabolic Modelling 

The metabobolic modelling code is in [Code/metabolic_modelling](Code/metabolic_modelling) and has been tested in Matlab R2015b. 

To calculate the metabolic fluxes for all yeast strains, run RUN.m. This will automatically start the FBA process using the gene expression data in msbdataAltered.mat and the metabolic model in yeastmm.mat. 

The obtained flux levels can then be extracted and used to create a complete dataset (see our experiment example in [Code/learning/Data](Code/learning/Data)).

Approx running time: 24 hours - this code will re-run the modelling process

#### Matlab dependencies

* [The COBRA toolbox](https://opencobra.github.io/cobratoolbox/latest/)


### Machine Learning

All the material for reproducing the machine learning tests is in [Code/learning](Code/learning).

To build the predictive models, three data files included in [Code/learning/Data](Code/learning/Data) first have to be extracted: 

* CompleteDataset.csv.zip - It contains gene expression, metabolic flux and growth rate data.
* Reduced_Dataset.zip - It contains gene expression, metabolic flux and metabolic gene expression data in distinct files.
* features.zip - It contains the features selected by the three methods listed above.

The R script machineLearning.R trains and applies all the feature selection and machine learning approaches, except for the deep neural networks. This script has been tested with R 3.4 and can be run by first putting the data in the same folder. 

Approx running time: 6 hours - this code will run feature selection and re-train the non-deep-learning models

#### R dependencies

The script machineLearning.R requires that the libraries listed at the top are installed to ensure it runs to completion. In particular, implementations of the machine learning methods used are in:

* caret
* nsga2R
* SGL
* iRF
* [bemkl](https://github.com/mehmetgonen/bemkl)


### Deep Learning

A separate Python script is dedicated to training and testing the deep learning models. Once the datasets in [Code/learning/Data](Code/learning/Data) have been extracted, run DeepLearningFullSet.py to build the deep learning models and obtain their set of results. 

Approx running time: 10 mins - for the inference component

#### Python dependencies

The deep learning code has been tested with the following libraries:
* Python 3.5
* Tensorflow 2.0 
* Keras 2.0
* Numpy 1.5
* Pandas 0.25
* Matplotlib 3.1.1
* Seaborn 0.9





