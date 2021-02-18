![leafSun](https://user-images.githubusercontent.com/32124230/107999911-9f3f1100-6fe0-11eb-9755-f48dbb82cf65.png)

# _futureCrop_
Developed by [**Peter Skelsey**](mailto:peter.skelsey@hutton.ac.uk?subject=futureCrop), James Hutton Institute, Dundee

## Basic overview
A desktop app for performing climate change risk assessments in real crop locations across Scotland:
* Train, tune, and test predictive regression- or classification-tree ensembles on your own data in a few easy button clicks.
* Select the required crop species to implement the fitted model across real crop locations.
* Explore possible future changes using a range of high resolution, fully coherent data for 12 future climates.
* Visualise your selections and results using maps, boxplots and bar charts.
* Save your model fitting and projection results to create your own statistics and graphics. 

## Example
The example regression dataset contains a response variable and values for 6 different weather variables. Regression tree ensembles are trained, tuned and tested using a nested cross validation procedure with Bayesian optimization for tuning of hyperparameters. Parallel computing is selected to speed up execution on multi-core devices. The final model is an ensemble of 129 regression trees fitted using the Least-Squares Boosting Algorithm.

![regModelTab](https://user-images.githubusercontent.com/32124230/108418695-82494e80-7229-11eb-9fac-f954d1f61b09.PNG)

The model is applied across real barley locations using climate change projection data for summer in 2030-2060. The ensemble of results for each decade reveal that the predicted response variable is expected to increase by 67% in 2060 relative to baseline conditions.

![regProjectionsTab](https://user-images.githubusercontent.com/32124230/108418731-8aa18980-7229-11eb-9183-85a244fba312.PNG)

## Motivation
This app was developed as a tool to help non-modellers access cutting-edge machine learning algorithms and perform state-of-the-art climate change risk assessments. It provides easy acces to real crop / land-use data from [IACS](https://ec.europa.eu/agriculture/direct-support/iacs_en) and [JACS](https://www.gov.scot/collections/june-scottish-agricultural-census/), and [UKCP18](https://www.metoffice.gov.uk/research/approach/collaboration/ukcp/index) fully coherent climate change data for 12 future climates (emissions scenario RCP8.5) and a range of time-periods and seasons. 

## Installation and loading
The GUI is a fixed size and not 'responsive,' i.e., it will not scale to fit the screen of your device. However, the GUI is small enough to run on notebooks and laptops. To download the files, click on the green 'Code' button near the top of the screen and then on 'Download ZIP'. Extract the files and double click 'futureCrop.exe'. This will open a web-based installer that will download 'MATLAB Runtime' to your computer (if required). MATLAB Runtime is a free software provided by MathWorks that enables users who do not have a MATLAB license to run compiled MATLAB applications (e.g., this app) on their computers. Once it is installed you will have access to thousands of MATLAB apps at no cost. If you already have the correct version of MATLAB Runtime then this step will be skipped. After installation the app will load using the shortcuts provided. When the app is loading a splash screen will appear then disappear and there will be a short interval (~5 seconds) before the app loads - there is no need to try and load it again. If you do not want to download the example data files and documentation that come bundled with the app, you can alternatively click on 'futureCrop.exe' then download and install as described.

## Documentation
A full guide on how to use the app to perform climate change risk assessments is provided in the [Documentation.md](https://github.com/pskelsey/futureCrop/blob/master/docs/Documentation.md)

## License
The MIT License (MIT) 2021 - Peter Skelsey. For more details, please have a look at the [LICENSE](https://github.com/pskelsey/futureCrop/blob/master/LICENSE) for more details.
