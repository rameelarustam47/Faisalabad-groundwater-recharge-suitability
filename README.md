# A Machine Learning and SAR-Based Geospatial Framework for Groundwater Recharge Suitability Mapping in Faisalabad District, Punjab

## Overview

This project develops an integrated **GIS, Remote Sensing, Sentinel-1 SAR, and Machine Learning framework** for assessing groundwater recharge suitability and identifying potential locations for groundwater recharge wells in **Faisalabad District, Punjab, Pakistan**.

The framework combines terrain, hydrological, soil, vegetation, land-use, rainfall, and flood-related information to support **flood-informed groundwater recharge planning**.

Faisalabad is experiencing increasing pressure on groundwater resources due to urbanization, intensive agriculture, and growing dependence on groundwater. At the same time, flood and rainfall-related water accumulation creates an opportunity to investigate whether excess surface water can be captured and used for groundwater recharge.

## Study Area

The study covers **Faisalabad District**, with an area of approximately **5,952.6 km²**.

The district includes:

* Faisalabad City
* Faisalabad Sadar
* Chak Jhumra
* Jaranwala
* Samundri
* Tandlianwala

The area is part of the **Lower Chenab Canal Command Area** and is characterized by a largely flat alluvial landscape with extensive irrigation and drainage networks.

## Objectives

1. Identify important environmental, topographical, hydrological, and land-use factors affecting groundwater recharge.
2. Integrate GIS and Remote Sensing datasets to produce a groundwater recharge suitability map.
3. Develop and compare Machine Learning models for flood-related classification.
4. Identify potential locations for rainfall- and flood-informed groundwater recharge wells.

## Data

The project uses multiple geospatial and remote-sensing datasets:

| Dataset                         | Application                   |
| ------------------------------- | ----------------------------- |
| SRTM DEM                        | Elevation and slope           |
| MERIT Hydro                     | HAND and flow accumulation    |
| Derived terrain indices         | TWI, curvature and STI        |
| ESA WorldCover                  | Land cover                    |
| OpenLandMap                     | Soil clay and sand            |
| Sentinel-2                      | NDVI / vegetation             |
| Sentinel-1 SAR                  | Surface-water/flood detection |
| ERA5-Land                       | Rainfall                      |
| GEOGloWS                        | River-discharge validation    |
| Drainage and settlement vectors | Spatial constraints           |

All major datasets were processed through **Google Earth Engine**.

## Methodology

The framework consists of several major stages:

### 1. Geospatial data preparation

Terrain, hydrological, soil, vegetation, land-cover, rainfall, drainage, and settlement datasets were prepared at the project grid scale.

### 2. Sentinel-1 flood detection

Sentinel-1 SAR imagery from **2021–2025** was used to identify flood signals based on changes in SAR backscatter.

### 3. Machine Learning

Terrain-only flood classifiers were developed using:

* Random Forest
* XGBoost
* LightGBM

Five-fold spatial cross-validation was used to evaluate model performance.

### 4. Groundwater recharge suitability

Recharge suitability was assessed by combining relevant terrain, soil, hydrological, vegetation, rainfall, and land-use factors.

Three suitability variants were developed:

* **Balanced**
* **Rainfall-based**
* **Flood-avoidance-based**

### 5. Recharge-well selection

Candidate recharge-well locations were selected while applying spatial constraints, including a minimum reported distance of **200 m from drainage and settlements**.

### 6. Flood validation

SAR-derived flood signals were compared with historical river-discharge information from the **GEOGloWS River Forecast System** to investigate whether detected flooding was associated with major Ravi River discharge events.

### 7. Flood recurrence

Flood maps from 2021–2025 were aggregated to distinguish:

* **Recurring flood:** flooding in 3 or more years
* **Unusual flood:** flooding in 1–2 years

## Results

### Groundwater Recharge Suitability

| Suitability zone | Share of district |
| ---------------- | ----------------: |
| Low              |              2.1% |
| Moderate         |             73.2% |
| High             |             20.9% |
| Very High        |              3.9% |

The majority of Faisalabad District falls within the **moderate suitability** class.

Jaranwala, Tandlianwala, and Chak Jhumra showed comparatively higher suitability areas.

### Recharge-Well Selection

| Well-selection strategy | Wells selected | Mean suitability score |
| ----------------------- | -------------: | ---------------------: |
| Balanced                |            100 |                   0.90 |
| Rainfall-based          |            100 |                   0.91 |
| Flood-avoidance-based   |            100 |                   0.97 |

The flood-avoidance strategy produced the highest reported mean suitability score.

### Machine Learning Performance

Spatial cross-validation results for terrain-only flood classification:

| Model         | F1-score |
| ------------- | -------: |
| Random Forest |    0.739 |
| LightGBM      |    0.720 |
| XGBoost       |    0.702 |

Under holdout evaluation with threshold tuning:

| Model         | Best threshold | Best F1 |
| ------------- | -------------: | ------: |
| Random Forest |            0.8 |   0.429 |
| XGBoost       |            0.9 |   0.421 |
| LightGBM      |            0.8 |   0.433 |

The terrain-only models performed substantially worse under the realistic holdout class imbalance.

## Feature Importance

The reported Random Forest feature importance included:

| Feature                                  |     Importance |
| ---------------------------------------- | -------------: |
| Soil clay content                        |          19.3% |
| NDVI                                     |          15.4% |
| Soil sand content                        |          12.2% |
| TWI                                      |           9.7% |
| STI                                      |           9.0% |
| Slope, curvature and HAND                | 22.3% combined |
| Infiltration, curve number and roughness | 12.0% combined |

Soil clay content was the most important individual feature, followed by NDVI and soil sand content.

## Flood Validation

Five Sentinel-1 flood-signal dates from 2021–2025 were compared with Ravi River discharge information.

The analysis reported temporal separation between the detected SAR flood signals and major river-discharge events. The project therefore suggests that the detected flooding may involve **local pluvial and/or canal-related processes** rather than direct overbank flooding from the Ravi River.

## Key Findings

* Most of Faisalabad District was classified as moderately suitable for groundwater recharge.
* Jaranwala, Tandlianwala, and Chak Jhumra contained comparatively higher suitability areas.
* Flood information can be incorporated into recharge-well planning.
* The flood-avoidance suitability strategy produced the highest reported mean suitability score.
* Random Forest produced the highest spatial cross-validation F1-score among the terrain-only models.
* Soil and vegetation variables were important predictors in the terrain-only flood classification.
* SAR-derived flood signals did not consistently coincide with major Ravi River discharge events during the examined period.

## Limitations

The project has several limitations:

1. The flood label was derived from a Sentinel-1 backscatter threshold and was not independently evaluated against ground-truth observations.
2. Terrain-only model performance decreased under realistic class imbalance.
3. The drainage dataset may omit smaller irrigation canals that are relevant to flooding in Faisalabad.
4. The settlement dataset has a coarser resolution than the 30 m predictor grid.
5. Rainfall data were used at approximately 1 km resolution and as multi-year averages.
6. The Sentinel-1 record should be extended beyond five years to strengthen flood-recurrence analysis.
7. Local infiltration testing should be carried out before finalizing recharge-well locations.

## Code and Reproducibility

### Google Earth Engine

**Data acquisition**

https://code.earthengine.google.com/db50d21e4cb42769e918a08334692b31

**Drainage**

https://code.earthengine.google.com/604212905058ab5a8196e153015bb39a

**Settlement**

https://code.earthengine.google.com/8fd4227ad0a83235d98a35923c5a2be9

### Google Colab

**Recharge-well locations**

https://colab.research.google.com/drive/1Iklx0HkCFddG5rC6jsNIwFbCTWypAayp

**Data validation**

https://colab.research.google.com/drive/1kSm_zV2zX6gmvrXCzu80K6e7QDmKsUD

**Maps**

https://colab.research.google.com/drive/1XzT9_unu8dFwEDtYeB6_CCUtTJgFpNUD

**Recurring vs. unusual flood classification**

https://colab.research.google.com/drive/1CsxhIVp8nRSdqXCxTBFnM2dQDLgplVK3

## Project Focus

**Floods + GIS + Remote Sensing + Sentinel-1 SAR + Machine Learning + Groundwater Recharge**

This project demonstrates how remotely sensed flood information and geospatial machine learning can be integrated into groundwater recharge planning for a flood-prone, canal-irrigated agricultural region.

## Author

**Rameela Rustam**

Water / Agricultural Engineer
Specialization: Irrigation and Drainage
Research interests: Flood Mapping, GIS, Remote Sensing, Machine Learning, Deep Learning, Groundwater Recharge and Climate-Related Water Risks

## Citation

If you use this project, please cite the associated research/report:

**A Machine Learning and SAR-Based Geospatial Framework for Groundwater Recharge Suitability Mapping in Faisalabad District, Punjab.**
