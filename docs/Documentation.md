![leafSun](https://user-images.githubusercontent.com/32124230/107999911-9f3f1100-6fe0-11eb-9755-f48dbb82cf65.png)

# DOCUMENTATION

# Table of Contents
* [Background](#background)
  * [Climate data](#climate-data)
  * [Crop data](#crop-data)
* [Basic operation](#basic-operation)
* [Model Tab](#model-tab)
  * [Algorithms](#algorithms)
  * [Data format](#data-format-and-upload)
  * [Sampling strategy](#sampling-strategy)
  * [Results](#results)
  * [Final model version](#final-model-version)
* [Projections tab](#projections-tab)
  * [Emissions panel](#emissions-panel)
  * [Landscapes panel](#landscapes-panel)
    * [How to incorporate cell areas](#how-to-incorporate-cell-areas)
    * [Select a distribution of locations](#select-a-distribution-of-locations)
    * [Artificial landscape generation](#artificial-landscape-generation)
  * [Dispersal panel](#dispersal-panel)
    * [Choose the type of dispersal](#choose-the-type-of-dispersal)
    * [Create the dispersal function](#create-the-dispersal-function)
  * [Plots panel](#plots-panel)
    * [Making maps](#making-maps)
    * [Plotting projected values](#plotting-projected-values)
    * [Saving results](#saving-results)
  

# Background
The app provides a very simple front-end to cutting edge machine learning algorithms, and is bundled with gridded crop distribution and climate change data. With a few button clicks you can fit a weather-dependent regression- or classification-tree ensemble to your own data, apply your model across real crop locations, and explore possible future changes under various climate change scenarios. 

### Climate data
The app uses seasonal 12 km gridded (65 x 112 cells) climate data from the UK Met Office Climate Projections database ([UKCP18](https://www.metoffice.gov.uk/research/approach/collaboration/ukcp/index)) 12-member ensemble of Regional projections. The following variables are provided: temperature (deg. C), relative humidity (%), wind speed (m s<sup>-1</sup>), net surface shortwave radiation flux (W m<sup>-2</sup>), precipitation (mm d<sup>-1</sup>), total cloud (%). The data are at presented at seasonal timescales (spring, summer, autumn, and winter) for each decade spanning 2020s-2060s. UKCP18 data for 1981-2000 are included as baseline values for comparison. All the climate data have full spatial and temporal coherence, enabling the assessment of multiple climatic drivers of changing hazards across multiple geographic locations. The data provide 12 equally plausible snapshots of climate change for emissions scenario RCP8.5, meaning that fitted models will produce an ensemble of 12 projected values for each grid cell, and a super-ensemble of results for each crop species distribution.

### Crop data
Polygon data defining the spatial coverage of crops and land-use types were derived from [IACS](https://ec.europa.eu/agriculture/direct-support/iacs_en) and [JACS](https://www.gov.scot/collections/june-scottish-agricultural-census/). These data cover Scotland only. The vector data were rasterised to 12 km grids matching the resolution of the climate change data. Information on the abundance (area) of crop in each grid cell is used for visualisation purposes only, and the presence-absence of selected crop species is used to narrow the climate change risk assessment to relevant grid cells.  

# Basic operation
*Tabs*: The app has two 'tabs' - 'Model' and 'Projections.' Click on the tab name to switch from one to the other.  
*Switches*: Drag the circluar switch to the left or right, or click in the empty space.  
*Lists*: Click on the desired option in the list.  
*Buttons*: Click them.  
*Numeric fields*: Click in the white box to change the numerical value, then hit enter or click outside the box.  
*State buttons*: Click them to toggle between two states - dark grey indicates 'on' and light grey is 'off'.  
*Knobs*: Drag the circular control around to your selection, or click on your selection.  
Note: some controles may be unavailable (greyed out) until certain operations have been performed, for example, model results cannot be saved until a model has been fitted to your data.  

# Model Tab
The risk models you create here must be a function of one or more of the weather variables included in the climate change projection data. Inclusion of other weather variables will produce meaningless results. Your 'response variable' (dependent variable) can be any weather-dependent process involving / affecting / occurring in or around crops, e.g., crop diseases, pests, crop growth etc.    

### Algorithms
The app uses the MATLAB commands ``fitrensemble`` to fit regression tree ensembles, and ``fitcensemble`` to fit classification tree ensembles, depending on the required modelling task. This is selected using 'Task' switch. For regression tasks your Y variable should be continuous and for classification tasks it must be categorical. If an attempt is made to upload data that do not match the selected task, a warning will be issued and the data will not be loaded. 

### Data format and upload
To upload your data click the 'Load' button. This will open up a dialogue box that allows you to search for your file. Data files must be .xls or .xlsx. Data should be arranged in rows with columns for the different variables. The column header for the response (dependent) variable should be an upper case 'Y', and the column headers for your weather variables should be all lower case: temp, rh, rain, wind, rad, cloud. It doesn't matter what order the columns are in, and you can include as many of the set of 6 weather variables as required. Missing data should be left as blank cells and these will be ignored during model fitting; do not use numerical missing data identifiers (e.g., -9999) as these will be treated as numbers. Text entries will be treated as missing data. Data for classification tasks require a little more consideration. Your categorical response variable must be coded as integers and not any form of character or string array (text, alphanumeric, symbols etc.) e.g., low, medium, and high should be converted to 1, 2, 3. Integers will be treated as discrete classes by the algorithms. This is required to eliminate the risk of typos, use of different date formats etc. You ideally want >25 values of Y for each discrete class. An acceptable performance may be achieved with fewer samples per class, but <5 per class on average will result in a warning and the data will not load. Consider aggregating the classes into fewer categories if the data per class is sparse, and fitting an algorithm on the new data to see if performance is improved. Unbalanced data, where the class proportions are skewed, is not a serious concern as the algorithms are ensembles of decision trees, which handle imbalanced data quite well, However, if the performance of the fitted model is poor, you may want to consider addressing this via, e.g., upsampling or downsampling. Two synthetic datasets are included for regression and classification, and you can refer to these if in doubt about the required data format. 

### Sampling strategy
Machine learning algorithms have parameters that are learned from the data (training) and hyperparameters that affect the learning process, whose value must be set before learning begins (tuning). It is common practice in machine learning to train, tune, and test your algorithm on different samples of your data, in order to provide an estimate of how well the entire model-building process will perform when confronted with new (unseen) data. A number of sampling strategies are provided in the 'Sampling' list. One of these strategies will be automatically selected based on the size of your dataset, but this can be overridden. If 'none' is selected then all the data will be used for learning, default values for the hyperparameters will be used (no tuning), and the model will be 'tested' using all the data it has already learned from. This is the least desirable option as it can lead to 'overfitting', where the algorithm learns the detail and noise in the training data to the extent that it negatively impacts performance when confronted with new data. It is, however, the fastest option. The next 3 options are prefixed by 'NCV' which stands for nested cross-validation. Cross-validation, or *k*-fold cross-validation, is a useful procedure to estimate the skill of an algorithm on unseen data and reduce overfitting when the size of the dataset is limited. Cross-validation has a single hyperparameter *k* that controls the number of subsets that a dataset is split into. Once split, each subset is given the opportunity to be used as a test set while all other subsets are combined as the training dataset. This means that *k*-fold cross-validation involves fitting and evaluating *k* versions of an algorithm. This, in turn, provides *k* estimates of an algorithm's performance on the dataset. In nested CV there are two loops of CV: an inner and an outer loop. Each training set of the outer cross-validation is further subdivided into folds in an inner loop of CV to find the best hyperparameter values for that training set. This is achieved using 'Bayesian optimization', via MATLAB's ``bayesopt`` command with *k* = 5. Once the optimal hyperparameter values are found, the algorithm is retrained and tested in the outer loop. This is considered to be the 'gold-standard' approach in machine learning, but it is computationally expensive and may be slow to run depending on the size of your dataset. The outer loop can be *k* = 5, *k* = 10, or 'loocv', which stands for leave-one-out cross-validation. This is an extreme version of *k*-fold cross-validation where *k* is set to the number of examples in the dataset. It requires one model to be created and evaluated for each example in your dataset, and therefore has the maximum computational cost and is not appropriate for very large datasets. Finally, there is the 'holdout' option, in which 80% of your data is used for training and tuning in a 5-fold Bayesian optimization, and 20% is 'held out' for testing the optimized algorithm. Automatic selection of the sampling strategy follows a commonly used approach based on the number of examples  *n* in your dataset: *n* <= 20 = 'none', 20 < *n* <= 100 = loocv, 100 < *n* <= 1000 = 10-fold, 1000 < *n* <= 10,000 = 5-fold, and *n* > 10,000 = 'holdout'. 

### Results

TTT results.

### Final model version

outer loop of cross-validation for training and testing, and an inner loop of cross-validation for tuning and testing. The inner loop provides the optimal hyperparameter values, which are then used to retrain and test the performance of the algorithm in the outer loop.       perfom to Select 'Use the list' using the switch. Select a function from the 'Model list' dropdown list. The equation will be displayed in the 'Model description' pane. Adjust the value of the parameters (a, b, c, d) using the numeric fields. The 'lower' and 'upper' fields will be greyed out as these are used for fitting models to data. Click the 'Run' button to visualise your model.

### Fit a model to your own data
Select 'Fit model to data' using the switch. This will open up a file selection dialog box. Your datafile must be an ASCII delimited text file or CSV file with the data stored in columns. The first column should contain the independent- or x-variable (i.e. the weather variable) and the second column the dependent variable. Replicate values of the dependent variable should be stored in subsequent columns. If your file does not meet these requirements then a warning will occur and the app will default back to 'Create a model.' A data file ('testData.txt') has been provided in the 'Assets' list of the release (see download instructions in the README.md) to help you get up and running with the model fitting features. Your data will be plotted in the 'Data / Predictions' plot pane once it has loaded successfully, and the x-axis limits of the plot pane will adjust according to the lower and upper values in your data. 

To fit a model, select one from the dropdown 'Model list.' The selected model will be displayed in the 'Model description' pane. The grid of 'Parameter' text fields in the top-right corner will adjust according to the model, with unnecessary fields greyed out. Default initial values for the parameters to be fitted are selected uniformly at random from the interval (0,1), or you can set these yourself using the 'initial' text field. Lower and upper bounds on the parameters to be fitted can be set using the 'lower' and 'upper' text fields. Click the 'Run' button to fit the model. The numerical fit results will be displayed in the 'Model description' pane and goodness of fit statistics in the 'GoF' pane. If the model fails to fit a warning will occur, with some suggestions to help improve the fit. The method of least squares (linear or nonlinear) is used when fitting data, or there is an option to fit a smoothing spline in the 'Model list'. Spline is a good option if your data is noisy or you are having trouble fitting other models, although this is considered a non-parametric fit and the display of numerical fit results will be limited. The value of the smoothing parameter for splines is fixed at 0.1. 

### Plot a model
Click the 'Run' button to display your user-defined or fitted model in the 'Data / Predictions' plot pane. Use the 'x-range' numeric field below the plot pane to visualise or extrapolate your model across a different range of values on the x-axis. The format required is min:step:max, e.g. to plot from 0 to 10 in steps of 0.1 you would enter 0:0.1:10. Then hit Return on your keyboard or click your mouse outside of the numeric field. Note that this is for visualisation purposes only and does not affect model fit.

### Final model selection
To use a model for projections you must select 'Use this model' with the switch in the bottom right corner of the tab, changing the lamp from red to green. You can now proceed to the Projections Tab.

# Projections Tab
<p align="left">
  <img src="https://github.com/pskelsey/4C-model/blob/gh-pages/projectionsTabLarge.png">
</p>

## Emissions panel
Select your climate variable, future time-period, and CO2 emissions scenario using the knobs. Select the month of the year or the season using the list box. For seasonal averages of the selected climate variable, Spr = spring (MAM), Sum = summmer (JJA), Aut = autumn (SON), and Win = winter (DJF). The climate change data are probabilistic, therefore a range of climate values will be provided for each grid cell (see [Climate data](#climate-data)).

## Landscapes panel
Here you define the distribution of locations (grid cells) that you want to use in your risk assessment. 

### How to incorporate cell areas
The 'Areas' radio buttons (No / Yes) are used to determine whether or not you want the area crop / land use type within each 25 km grid cell to be incoporated into risk model projections. 'No' = projections are made for every grid cell containing the crop / land use of interest. 'Yes' = projected values incorporate the area of crop / land use within each grid cell, and if required, also a cell density parameter that is set using the numeric field below the 'Yes' radio button: projected value = risk model value x area x density. The following examples demonstrate how these radio buttons and the density numeric field can be combined for a wide variety of different outcomes:
1. Your risk model describes the impact of a climate variable on any response variable (e.g. disease severity per observational unit,  crop growth rate, etc.). To project future values of the response variable in grid cells containing any amount of the land use type of interest, select 'No': projected value = risk model value.
2. Your risk model describes the impact of climate on the density of a variable per square meter of crop cover (e.g. pests per square meter of crop cover). To project the total number in each grid cell, select 'Yes' and leave the density field equal to 1 (i.e. square meters of crop per square meter of crop): projected number per cell (pests) = risk model value (pests m<sup>-2</sup>) x area of crop per cell (m<sup>2</sup>) x 1 (m<sup>2</sup> m<sup>-2</sup>).
3. Your risk model describes the impact of climate on the density of a variable per square meter of crop canopy (e.g. lesions per square meter of foliage). To project the total number in each grid cell, select 'Yes' and set the density field equal to the ratio of canopy area to ground area (i.e. the leaf area index, LAI): projected number per cell (lesions) = risk model value (lesions m<sup>-2</sup>) x area of crop per cell (m<sup>2</sup>) x LAI (m<sup>2</sup> m<sup>-2</sup>).
4. Your risk model describes the impact of climate on the density of a variable per observational unit (e.g. spores per lesion). To project the total number of spores in each grid cell, select 'Yes' and set the density field equal to the ratio of the observational unit to crop area (e.g. lesions per square meter of crop cover): projected number per cell (spores) = risk model value (spores lesion<sup>-1</sup>) x area of crop per cell (m<sup>2</sup>) x density of lesions (lesions m<sup>-2</sup>).
5. Your risk model describes the impact of climate on the per capita growth rate of a population of organisms. To obtain the future population size within the habitat / host contained in each grid cell, select 'Yes' and set the density field equal to the initial number of organisms per square meter of crop: projected number per cell (individuals) = risk model value (-) x area of crop per cell (m<sup>2</sup> crop) x initial density (individuals m<sup>-2</sup>). Note that the growth rate in your risk model should be a monthly or seasonal per capita growth rate to match the temporal resolution of the climate change data (see [Climate data](#climate-data)). 

The density parameter can be used in many other ways; just make sure you do a dimensional analysis, as above, so that the units on the left hand side of the equality operator match the units on the right hand side.

### Select a distribution of locations
Select a region of the Scotland or England using the 'Region' knob. Sco_N, Sco_E, Sco_W = Northern, Eastern and Western Scotland, respectively. Eng_N, Eng_M, Eng_SE, Eng_SW = North, Midlands, South East, and South West England, respectively. Once a region is selected, choose a spatial distribution of crop / land use locations using the  'Crop / land use' list box: P = potato, SB = spring barley, WB = winter barley, SO = spring oats, WO = winter oats, SW = spring wheat, WW = winter wheat, F = forestry, W = water bodies (ponds, lakes, rivers).

### Artifical landscape generation
The 4C model also allows you to generate your own distribution of locations. This is useful if the crop distribution desired is not available, and for adaptation scenarios involving a change in the amount and spatial distribution of crop locations. Artifical landscapes are generated using fractal geometry (the ‘inverse Fourier transform' method) to create binary landscape patterns of occupied / unoccupied cells (e.g. habitat / non-habitat). Select 'Artificial' using the switch. Set 'Seed' using the numeric field - this controls the random generation of patterns, allowing you to replicate a pattern by using the same value for seed. The 'f' numeric field (0,1) sets the fraction of GB land area that will be occupied. The 'H' numeric field (0,1) controls the degree of aggregation of occupied cells via a parameter known as the Hurst exponent. The 'r' numeric field (0.01,1) controls the texture of the pattern, or the size distribution of gaps among occupied cells, via a parameter known as the lacunarity.

## Dispersal panel
The features in this panel are used to define spatial relationships among grid cells, i.e. physical dispersal among cells, or degree of connectivity among cells (on a scale of 0 to 1). These spatial relationshps are used to modify the projected values from your risk model. For those who are interested, spatial phenomena are solved by performing a spatial convolution between the distribution of risk model values in the selected cells and a 2D radially-symmetric probability density function (dispersal kernel), implemented via fast Fourier transforms (see [Skelsey et al. 2013](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0075892)). 

### Choose the type of dispersal
The 'Dispersal' knob is used to define three different types of spatial relationship among cells: 
1. No dispersal - set 'Dispersal' to 'Off.' This means that projected values from your risk model are not modified to incorporate a spatial relationship among cells.
2. Dispersal is a physical process involving absolute numbers of dispersing agents - set 'Dispersal' to 'Std.' or 'No self.' In both cases, the projected values from your risk model are treated as a 'source' of material (an amount per cell) to be dispersed among other grid cells. When 'Std.' (standard) is selected, source values are dispersed among all grid cells *including* the cell of origin. An example would be passive dispersal of spores, whereby a proportion deposit close to the infection source (in the same grid cell). When 'No self.' (no selfing) is selected then all source material is dispersed outwith the boundaries of the source cell. An example would be active dispersal of pests away from a depleted habitat resource. When 'No self.' is selected the centre of the dispersal kernel is set to 0.   
3. Dispersal is a weighting factor, ranging from 0 to 1, that describes the degree of connectivity of each cell to all other cells - set 'Dispersal' to 'Conn.' In this case the dispersal kernel is normalized to a maximum value of unity. An example would be a risk model that projects the likelihood of pest occurrence (on a scale of 0 to 1), which is then weighted according to the connectivity of host cells (on a scale of 0 to 1) to provide projections of the risk of pest occurrence and spread (on a scale of 0 to 1). 

### Create the dispersal function
To change the dispersal function use the 'Kernel' knob:  fat = fat-tailed (square-root negative exponential kernel), thin = thin-tailed (negative exponential kernel), Gauss = Gaussian kernel. These three kernels are all derived from the exponential-power distribution and all have a different shape (see [Skelsey et al. 2013](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0075892) for a thorough explanation). Fat-tailed kernels are often used for processes that operate at broad spatial scales, such as long-distance dispersal by wind, whereas thin-tailed kernels are often used for processes that operate at fine spatial scales, such as splash dispersal. The kernel is parameterized using the 'mdd' numeric field = mean dispersal distance. Enter a number for the average distance that an individual can disperse. A 1D slice through your dispersal kernel (i.e. a dispersal gradient) is automatically displayed in the 'Dispersal gradient' plot pane.

## Plots panel

### Making maps
Click the 'Map' button to the left of the 'Map of climate variable / land use' plot pane. Switch between a map of the selected climate variable and a map of the selected crop / land use distribution using the 'Plot' radio buttons immediately beneath. 

### Plotting projected values
Once your future scenario has been defined, click the 'Project' button to the left of the 'Boxplots of projected values' plot pane. This will produce a boxplot of projected values for that scenario. Each boxplot is a 'super-ensemble' of projected values (11 SCPs x *n* selected grid cells). Boxes extend from the first to the third quartile, medians are marked in each box, and whiskers extend to 1.5 times the interquartile range. You can display multiple boxplots (i.e. multiple scenarios, such as a different month of the year / decade / emissions level / risk model / crop distribution, etc.) together in the plot pane. Click the 'Clear' button to clear the plot pane. The 'Plot' radio button 'Abs' will plot the absolute projected values from your risk model. The 'Chg' radio button will plot the projected values relative to those for the 1961-1991 baseline climatology to show the proportional response; i.e. the percentage change in the response variable relative to baseline conditions. The 'Scale' radio buttons 'Lin.' and 'Log.' will change the scale of the y-axis from linear to logarithmic.   

### Saving results
Click the 'Save' button to save your results. This will open up a file save dialog box, where you can name your results file and save to any location. Note that only the results for the boxplots displayed in the plot pane will be saved - if you click the 'Clear' button then these results will be lost. Your results will be saved to an ASCII file, and the projected values for each scenario you define (each boxplot) will be stored in a separate column. Each column will contain 22308 values (52 x 39 cells x 11 SCPs). Only a small proporition of these values will be numerical, pertaining to the grid cells you selected for your projections. The rest will be NaN (not a number), representing the grid cells (land and sea) not selected. 



