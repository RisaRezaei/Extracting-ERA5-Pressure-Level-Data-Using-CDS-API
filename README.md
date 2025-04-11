# Extracting ERA5 Pressure Level Data Using CDS API

## Overview
This script downloads ERA5 reanalysis pressure-level monthly mean data from the Climate Data Store (CDS) API. The data includes specific humidity, U and V wind components at multiple pressure levels (1000 hPa, 850 hPa, 700 hPa, 500 hPa, 300 hPa) for the years 2022, 2023, and 2024. The data is retrieved for months November through April and for specific times of the day. The results are saved as individual .nc (NetCDF) files.

---
## Code Explanation
### **Step 1: Set the API Key and Initialize CDS Client**
The CDS API key is specified in the .cdsapirc file. This key is necessary for authenticating requests to the CDS API.
```python
os.environ['CDSAPI_RC'] = r'C:\Users\Risa\.cdsapirc'
c = cdsapi.Client()  # Initialize the CDS API client
```
### **Step 2: Define Years and Pressure Levels**
The script defines the list of years (2022, 2023, 2024) and pressure levels (1000, 850, 700, 500, 300) to loop over for data retrieval.
```python
years = ["2022", "2023", "2024"]
pressure_levels = ["1000", "850", "700", "500", "300"]  # Separate request for each level
```
### **Step 3: Loop Through Years and Pressure Levels**
The script iterates over each year and pressure level. For each combination, it requests data for specific variables (specific humidity, U and V wind components) at multiple time intervals throughout the day (00:00, 06:00, 12:00, 18:00).
```python
for year in years:
    for level in pressure_levels:
        print(f"Requesting data for {year}, pressure level {level} hPa...")
        c.retrieve(
            'reanalysis-era5-pressure-levels-monthly-means',
            {
                "variable": ["specific_humidity", "u_component_of_wind", "v_component_of_wind"],
                "pressure_level": level,  # Request one pressure level at a time
                "year": year,
                "month": ["11", "12", "01", "02", "03", "04"],  # November to April
                "day": [str(i).zfill(2) for i in range(1, 31)],  # Days 01-30
                "time": ["00:00", "06:00", "12:00", "18:00"],
                "format": "netcdf"
            },
            f'ERA5_{year}_{level}.nc'  # Save each file separately
        )
        print(f"Download complete for {year}, pressure level {level} hPa!\n")
```
### **Step 4: Save Files for Each Request**
Each dataset is saved as a .nc (NetCDF) file with the name format ERA5_<year>_<level>.nc, allowing easy tracking of the different datasets.

## Key Features

✔ Multiple Pressure Levels: Data is retrieved for a range of pressure levels (1000 hPa to 300 hPa), suitable for various atmospheric studies.

✔ Time-Specific Data: Data is retrieved for multiple time intervals each day (00:00, 06:00, 12:00, 18:00) to capture variations in atmospheric conditions.

✔ Automated Download and Saving: The script automatically saves the data for each year and pressure level combination as a .nc file.

## Potential Applications
📍 Weather Forecasting: Useful for studying atmospheric conditions and their impact on weather forecasts.

📍 Climate Research: Helps in understanding pressure-level variations in the atmosphere over multiple years.

📍 Wind and Humidity Analysis: Wind and humidity data at different atmospheric levels are crucial for meteorological modeling and prediction.
