![leafSun](https://user-images.githubusercontent.com/32124230/107999911-9f3f1100-6fe0-11eb-9755-f48dbb82cf65.png)

# _futureCrops model_
Developed by [**Peter Skelsey**](mailto:peter.skelsey@hutton.ac.uk?subject=findOUT), James Hutton Institute, Dundee

## Basic overview
A desktop app for performing climate change risk assessments in real crop locations in Great Britain:
* Upload your own data.
* Train, tune, and test a Random Forest regression model / classifier in a few easy clicks.
* Choose a raster distribution of crop species to apply your model in.
* Select a future time-period and season.
* Hit the run button, visualise your results, and save them to create your own graphics. 

## Example
Data for the proportion of a crop infected with a disease together with observed weather variables is uploaded. A Random Forest regression model is trained, tuned, and tested with a nested k-fold cross validation procedure that utilizes Bayesian optimization for tuning of hyperparameters. This is all achieved with a few button clicks. The results can be saved to your PC.

![regressionTab1](https://user-images.githubusercontent.com/32124230/108000033-e3caac80-6fe0-11eb-9ee5-8357da6abfc0.PNG)

Once your model is defined in the Model Tab, you can move on to the Projections Tab. in this example, barley has been selected as the crop distribution and the results panel shows boxplots of projected values for the the 2030s - 2060s.

![regressionTab2](https://user-images.githubusercontent.com/32124230/108000964-183f6800-6fe3-11eb-9286-72b11232524e.PNG)

## Motivation
This app was developed as a tool to help non-modellers access cutting-edge machine learning algorithms and perform state-of-the-art climate change risk assessments. At your fingertips are real crop / land-use data from [IACS](https://ec.europa.eu/agriculture/direct-support/iacs_en) and [JACS](https://www.gov.scot/collections/june-scottish-agricultural-census/), and [UKCP18](http://https://www.metoffice.gov.uk/research/approach/collaboration/ukcp/index) spatially and temporally coherent probabilistic climate change data (emissions scenario RCP8.5). 

### Installation and loading
NOTE: Due to the many controls and features of the app, the GUI is a fixed size and not 'responsive,' i.e. it will not scale to fit the screen of your device. However, the GUI is small enough that it should be suitable for notebooks etc. with smaller screens. To make sure you get the latest release, click on the 'release' button near the top of the page:

<p align="left">
  <img src="https://github.com/pskelsey/4C/blob/gh-pages/download.PNG">
</p>

Then click on the executable file at the top of the Assets list - _cropConnectivityUnderClimateChange.exe_. This will download a web-based installer to your computer, so you will need an internet connection to install the app. When you run the installer, it will download MATLAB Runtime to your computer (if required). MATLAB Runtime is a royalty-free standalone set of shared libraries that enables the execution of compiled MATLAB applications on computers that do not have MATLAB installed. If you have an up-to-date version of MATLAB then this step will be skipped. MATLAB Runtime requires approximately 1GB of disk space so it may take some time to install. Once installation of Runtime is complete the app will load quickly using the shortcuts provided. When the app is loading a splash screen will appear then disappear, and there will be a short interval before the app loads; there is no need to try and load it again, just be patient. 

### Documentation
A full guide on how to use the app to perform climate change risk assessments is provided in the [Documentation.md](https://github.com/pskelsey/4C/blob/master/docs/Documentation.md)

### References
[Skelsey, P. et al. 2017. Potential impacts of climate change on the threat of potato cyst nematode species in Great Britain. Plant Pathology, DOI: 10.1111/ppa12807.](http://onlinelibrary.wiley.com/doi/10.1111/ppa.12807/full)

[Skelsey, P. et al. 2016. Crop connectivity under climate change: future environmental and geographic risks of potato late blight in Scotland. Global Change Biology 22:3724–3738.](http://onlinelibrary.wiley.com/doi/10.1111/gcb.13368/full)

[Skelsey, P., and Newton, A.C. 2015. Future environmental and geographic risks of Fusarium head blight of wheat in Scotland. European Journal of Plant Pathology 142:133–147.](https://link.springer.com/article/10.1007/s10658-015-0598-7)

### License
The MIT License (MIT) 2017 - Peter Skelsey. For more details, please have a look at the [LICENSE](https://github.com/pskelsey/4C-model/blob/master/LICENSE) for more details.
