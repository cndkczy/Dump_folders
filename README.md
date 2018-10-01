##Toolbox for Inflorescence Measurements (TIM)

#Introduction

This text describes the methods used for the Toolbox for Inflorescence Measurements (TIM). TIM consists of two functions: 

	i) app for semi-automated image processing. The installation file of the app and instruction video are stored at "App_for_Image_Processing" folder
	ii) automated trait extraction pipelines. The scripts related to this function could be found at "Phenotype_Extraction" folder

In order to reach high accuracy, panicle images need first be cropped from background using "SorghumPanicle" app. The app will generate independent folder for each image, thus the user himself need move the images and corresponding data to a customized folder for next phenotype extraction. 

If the user need later transform data from pixel into cm. A function that could measure the pixels of reference is also provided by the app.

The automated trait extraction involves different packages and custom-built algorithms mainly in MATLAB and all the codes could be incorporated into one bash-script.

The scripts and app require preinstalled MATLAB with version 2017b or later version.

For further details please check supplemental methods of "Semi-Automated Feature Extraction from RGB Images for sorghum panicle architecture GWAS".

For questions please contact schnable lab: "schnable@iastate.edu"
