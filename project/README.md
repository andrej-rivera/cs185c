# Philippines Salinity during Monsoon Seasons
In the Philipines and many Southeast Asian countries, the climate is influenced by El Niño and La Niña years which cycle every few years and have different effects. The effects of these El Niño and La Niña on surface temperature and precipitation is heavily documented, but little documentation exists on their effects on salinity. To provide a new point of view on the effects of these years, I will be observing how these affect salinity during Monsoon seasons, a season marked by heavy rains and a higher frequency of tropic cyclones.

## Project Description

In this project, I will be observing how El Niño and La Niña affect salinity in the Philippines during the monsoon season.


As a general topic, I will be answering the following question:


*Are salinity levels during monsoon seasons (in the Philippines) different in El Niño years when compared to La Niña years?*

To answer this question, I will be creating a model that encompasses the Philippines region and the bodies of water that surround it, the South China Sea and the Pacific Ocean. My model will be observing salinity during two years with notable El Niño and La Niña intensities: 1997 (Strong El Niño year) and 1998 (Strong La Niña year). These are based on the [Oceanic Niño Index (ONI)](https://ggweather.com/enso/oni.htm). For my project, I will be primarily using data from the ECCO Version 5 model in the years provided. For both years, I will run my model for a full year. The range between May and October represents the Monsoon seasons in the Philippines. To analyze results, I'll create two timeseries and look at differences in salinity throughout time for both years and focus on the May-October range. I'll also be creating a movie of salinity differences comparing both years.


Before running this experiment, my initial hypothesis on the question is that La Niña years will experience lower salinity levels than El Niño years. The basis in this hypothesis lies in the characteristics of El Niño and La Niña years. For El Niño years, locations near the equator (like the Philippines) are much hotter and so rainfall would be decreased as a result. Meanwhile, the situation is reversed for La Niña years and so rainfall would increase, leading to a lower salinity level because of all the rainfall.


## Reproducing Model Results

The following steps outline how to construct the model files, configure and run the model, and assess the model results.

### Step 1: Create the Model Files
Several input files need to be created to run the model. Generate the following list of files using the notebooks indicated in paratheses:

**For 1997 (El Niño)**
- Model Grid (notebook/1997/Generate Model Grid.ipynb)
- Bathymetry (notebook/1997/Generate Model Bathymetry.ipynb)
- Initial Conditions (notebook/1997/Create Initial Conditions.ipynb)
- External Forcing Conditions (notebooks/1997/Create the External Forcing Conditions.ipynb)
- Boundary Conditions (notebook/1997/Create Boundary Conditions.ipynb)

**For 1998 (La Niña)**
- Model Grid (notebook/1998/Generate Model Grid.ipynb)
- Bathymetry (notebook/1998/Generate Model Bathymetry.ipynb)
- Initial Conditions (notebook/1998/Create Initial Conditions.ipynb)
- External Forcing Conditions (notebooks/1998/Create the External Forcing Conditions.ipynb)
- Boundary Conditions (notebook/1998/Create Boundary Conditions.ipynb)

The model files should be placed into the `input/1997` and `input/1998` directories respectively.

### Step 2: Add files to the computing cluster
Once the input files have been created, the model files can be transferred to the computing cluster. Begin by cloning a copy of [MITgcm](https://github.com/MITgcm/MITgcm) into your scratch directory and make a folder for the configuration, .e.g.
```
mkdir MITgcm/configurations/Philippines_1997
mkdir MITgcm/configurations/Philippines_1998
```
Then, use the `scp` command to send the `code`, `input`, and `namelist` 
directories to your configuration directory. The namelist files for 1997 and 1998 are different and can be found at the `namelist/1997` and `namelist/1998` directories.

Make sure to send the files for 1997 to Philippines_1997 and the files for 1998 to Philippines_1998.


### Step 3: Compile the model
Once all of the files are on the computing cluster, the model can be compiled. Make a `build` directory in the configuration directory and run the following lines:
```
../../../tools/genmake2 -of ../../../tools/build_options/darwin_amd64_gfortran -mods ../code -mpi
make depend
make
```
Follow this step for both 1997 and 1998 configurations.

### Step 4: Run the model for 1997 and 1998
After the compilation is complete, run the model for 1997. Move to the run directory, link everything from `input` and `code`, and the submit the job script:
```
sbatch cs185c15.slm
```

Next, run the model for 1998 to complete the experiment. 
Note that the .slm file for both 1997 and 1998 are the same so all you need to do is run the same command.

### Step 5: Analyze the Results
There are two notebooks provided for analysis:
1. Assessing Model Results (located in `notebook/1997/Analyze the Results.ipynb` and `notebook/1998/Analyze the Results.ipynb`

   This notebook is provided to have a quick look at spatial and temporal variations in the temperature field in the models. It also generates the visualization provided in the figures directory. After running the notebooks for both years, you should have two .mp4 files displaying surface salinity for both 1997 and 1998. The notebooks can also be slightly modified to display surface temperature.
   
2. Answering the Science Question
   
   This notebooks provides an analysis plot to address the science question posed above. It generates a line graph of salinity for both 1997 and 1998, allowing for a direct comparison.

----

## Model Findings
These findings can also be found in the "Answering the Science Question" notebook.


During the Monsoon season (May-October) in the Philippines, we see that El Niño years (1997) have lower salinity levels on average compared to La Niña years. This is different from my initial hypothesis which assumed that the increased rainfall and temperatures during La Niña years would decrease significantly decrease salinity over El Niño years.

### Further Investigation
One possible explanation for this is how water currents move water during El Niño years. During El Niño years, warm water from Southeast Asia is pushed westward towards South America. This could cause the high-salinity waters to be replaced by lower-salinity waters from the western Pacific. An example of this occurring can be seen in this [video](https://www.youtube.com/watch?v=gaFjlZxM7S4). Here, we see in a more global-snapshot of the ocean and that during 1997 (El Niño) warm water moves eastward and cold water moves westward into the Phiippines. 

Another notable fact mentioned in the video is that it that the El Niño doesn't actually seem to 'start' until July of 1997; reaching its peak in December and ending around the March of 1998. For La Niña, the similar pattern of beginning in the middle of the year and peaking in December of 1998 occurs. This fact could've potentially skewed my outputs as well because for majority of 1997, El Niño wasn't actually significantly affecting the salinity at all. Perhaps the movement of water from east to west offsets salinity when the El Niño is weak, and its only when the El Niño starts to get stronger that the effects of evaporation and higher temperatures kick in (eg: high salinty in december 1997 when El Niño peaks, very low salinity in december 1998 when La Niña peaks).

### Future Notes
If I were to repeat this project again, I would like to run my model for longer periods of time (1.5-2 years long) so that I can see the fully extent to which El Niño and La Niña events shape surface temperatures and salinity. I'd also run the model with different years. Rather than going with years with the strongest El Niño and La Niña events, it would've been better to pick years where the [peaks of the events](https://ggweather.com/enso/oni.htm) coincides with the months of the monsoon season. Finally, I would've liked to run the model with a few parameters off like precipitation and evaporation to see if they significantly change my model's results. 

