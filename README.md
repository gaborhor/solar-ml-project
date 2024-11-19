# Solar ML Modeling and Feature, Error Analaysis

## Overview:

This solo project was created with the intent of engineering non-lag time-based data features and identifying "secondary" solar and atmospheric data features meaningful in ML modeling of photovoltaic cell outputs.
Though this data is Time Series in nature, Forecasting is likely a non-starter as models may take weather conditions as input, and weather itself can only be forecast in relatively short terms.
Due to this, I instead performed model optimization and error analysis while comparing SciKitLearn's HistGradientBoosting Regressor and XGBoost Regressor as we expected to be working with NaN values.

## File Glossary:

solar-ml-modeling.ipynb :

  Jupyter Notebook with python code implementing Pandas, Scikit-Learn, and XGBoost in exploring Time-Series photovoltaic energy production (kW) data.

solar-ml-modeling_presentation.pptx :

  STAR-method Powerpoint presentation providing Situation, Task, Actions, and Results for brief breakdown of project.

## Data Glossary:

Time or Static Features:
  * ['start'] – start of 15-minute interval time of measurement (i.e. 12:15:00 to 12:29:59)
  * ['kw'] – power production (in 15-min interval) - TARGET FEATURE FOR MODELING
  * ['capacity_clipped'] – maximum solar farm power capacity
  * ['time_hourly'] – hourly timestamp

Solar Features:
  * ['S_d'] – direct beam solar radiation intensity. Typically measured in Watt/Steradian. NOTE: "Direct solar radiation" is shortwave radiation able to penetrate through the atmosphere without having been affected by constituents of the atmosphere in any way.
  * ['airmass'] - quantifies the reduction in the power of light as it passes through the atmosphere and is absorbed by air and dust
  * ['altitude'] – position of sun relative to the horizon (+65 is high noon Northern hemisphere, 0 is exactly at the horizon, -65 is high noon Southern hemisphere)
  * ['azimuth'] – azimuthal angle (how far off of North the sun is in the sky throughout a full earth-rotation)[somewhere between 0-360 degrees over the course of a full day, squeezing/stretching over the year]
  * ['irradiation'] – power-per-unit-area (surface power density) received from the Sun in the form of electromagnetic radiation in the wavelength range of the measuring instrument. Typically measured in [1] x100MJ/hr/m^2^ or [2] kW/m^2^. Convert [1]->[2] by dividing by 360).
  * ['fold_cos'] – cosine of the sunlight’s incident angle on the ground
  * ['panel_cos'] - cosine of the sunlight’s incident angle on the panel ^In other words, cosine of the angle between source-normal sunlight and normal of the ground or panel.
  * ['sunrise'] – time of sunrise to the microsecond
  * ['sunset'] – time of sunset to the microsecond
  * ['rad_lw_mean'] – long wave radiation

Weather Features:
  * ['precip_total_mean'] - average of total precipitation for a given time period. Typically measured in (mm) or (in).
  * ['cloud_total_mean'] - Percentage of the sky hidden by all visible clouds
  * ['cloud_high_mean'] - Percentage of the sky hidden by high-altitude clouds
  * ['cloud_mid_mean'] - Percentage of the sky hidden by medium-altitude clouds
  * ['cloud_low_mean'] - Percentage of the sky hidden by low-altitude clouds
  * ['temp_total_mean'] - mean Temperature across 15-minute interval
  * ['rad_global_mean'] - "global" radiation refers to the capturing of radiation amounts by a "global" sensor, which is one that receives from 180-degrees of input as opposed to a smaller angle.
  * ['radNetS_lw_mean'] - Surface net long-wave (Earthly) radiation. NOTE: Heat energy absorbed by the Earth's surface, re-emitted toward the atmosphere as radiation, and reflected back down to the surface.
  * ['radNetS_sw_mean'] - Surface net short-wave (solar) radiation. NOTE: Solar radiation (direct or diffuse) that reaches the Earth's surface.

__Engineered Features:__
These features were engineered as "distance transformations" of the Time, allowing models to better understand the different between changes in hour, month, day-of-week, and day-of-year.
  * ['hour_sin']
  * ['hour_cos']
  * ['month_sin']
  * ['month_cos']
  * ['dayofwk_sin']
  * ['dayofwk_cos']
  * ['dayofyr_sin']
  * ['dayofyr_cos']

## Tools and Approaches:

* Pandas, Numpy, Datetime - data handling
* Matplotlib, Seaborn - data visualization
* SciKitLearn - Error metrics, TimeSeriesSplit, TrainTestSplit, GridSearchCV, modeling w/ HistGradientBoosting Regressor
* XGBoost - modeling w/ XGBRegressor

## __Results:__

* Identified sub-optimal handling of NaN values by HistGradientBoosting Regressor, resulting in usage of XGBRegressor.
* Identified 'Temperature' and 'Irradiation' as important regulating features in modeling.
* Created models to both handle and ignore NaN values in varying sets of features, resulting in Mean Absolute Error values of 14.3 kW and below for predictions on a scale of 0 to 468 kW.

## Future Opportunities:

* Data imputation or interpolation for NaN-valued features, employing 'pvlib' and 'pvanalytics' libraries
