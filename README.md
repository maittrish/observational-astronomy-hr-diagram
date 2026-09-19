# Observational Astronomy Data Pipeline: The Hertzsprung-Russell (H-R) Diagram

## Project Overview
This project is an automated data engineering and visualization pipeline that reconstructs the Hertzsprung-Russell (H-R) Diagram—a foundational scatter plot in stellar astrophysics that maps the evolutionary life cycle of stars. 

By ingesting raw observational satellite data, handling missing/invalid sensor readings, and applying vectorized trigonometric and logarithmic transformations, this pipeline converts apparent magnitudes into true physical luminosities. 

This project bridges the gap between Data Science and Astronomy, demonstrating how programmatic feature engineering can be used to extract physical reality from raw survey datasets.

## Data Source
* **Dataset:** Hipparcos Star Catalog (European Space Agency)
* **Features Used:** * `Vmag`: Visual Apparent Magnitude (Raw brightness sensor data)
  * `Plx`: Parallax angle (milliarcseconds)
  * `B-V`: Color Index (Proxy for surface temperature)

## Pipeline Architecture
The pipeline is built purely in Python and optimized for computational efficiency over large astronomical datasets (~120,000 rows). 

1. **Data Cleaning (`pandas`):**
   * Filtered out missing `B-V` color index values.
   * Eliminated impossible physical states by dropping records with a Parallax (`Plx`) $\le 0$, which would result in division-by-zero errors or negative distances.
2. **Feature Engineering (`numpy`):**
   * **Distance Calculation:** Converted parallax from milliarcseconds to arcseconds to calculate the true distance to each star in parsecs ($d = 1/p$).
   * **Distance Modulus:** Applied vectorized logarithmic functions to calculate the Absolute Magnitude ($M$) from the Apparent Magnitude ($m$):
     $$M = m - 5 \log_{10}(d) + 5$$
3. **Data Visualization (`matplotlib`):**
   * Generated a high-density scatter plot mapping Color Index (Temperature) against Absolute Magnitude (Luminosity).
   * Applied a temperature-accurate colormap (`RdYlBu_r`) and inverted the Y-axis to align with standard astrophysical conventions (brighter stars at the top).

## Scientific Results
The executed code successfully clusters the raw data into the three primary evolutionary stages of stellar bodies:
* **The Main Sequence:** The dominant diagonal band representing stable stars burning hydrogen (including our Sun).
* **The Red Giant Branch:** The upper-right cluster of massive, cool, but highly luminous dying stars.
* **White Dwarfs:** The lower-left scattering of hot, dim, exposed stellar cores.

## How to Run
1. Ensure the `hipparcos-voidmain.csv` dataset is located in the same directory as the script.
2. Install dependencies: `pip install pandas numpy matplotlib`
3. Execute the Python script to clean the data, engineer the features, and render the plot.
