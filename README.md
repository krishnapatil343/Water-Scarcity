# Water-Scarcity
🌏 Historical Water Scarcity Analysis using Google Earth Engine  This project provides a detailed analysis of historical water scarcity trends using the NASA GLDAS-2.1 NOAH Land Surface Model dataset from Google Earth Engine. It visualizes three critical indicators:  📉 Soil Moisture 🌤 Evapotranspiration 🌡 Surface Temperature

🚀 Features

Extracts historical water scarcity trend data from the GLDAS dataset.
Visualizes time-series charts for:
Soil Moisture (m³/m³)
Evapotranspiration (mm/day)
Surface Temperature (K)
Highlights the location with a red spot on the map for clear spatial reference.
Focuses on the surrounding area


⚙️ How to Use

Set Up Google Earth Engine:
Make sure you have access to Google Earth Engine.
Use the Code Editor to paste and run the script.
Change Location:
To adapt this analysis for any location worldwide:
Replace the coordinates in the following line with your desired location:
var Location = ee.Geometry.Point([longitude, latitude]).buffer(5000);
Example for New York:
var newyorkPoint = ee.Geometry.Point([-74.006, 40.7128]).buffer(5000);
Adjust Time Range:
You can modify the time range to your desired years:
var startYear = 2002;
var endYear = 2024;
var years = ee.List.sequence(startYear, endYear);
📊 Graphs and Visualizations

Time-Series Charts:
Soil Moisture (2002-2024)
Evapotranspiration (2002-2024)
Surface Temperature (2002-2024)
Observations:
Soil Moisture: High variability with significant peaks and troughs. The fluctuations are likely linked to precipitation patterns and evapotranspiration rates.
Evapotranspiration: Shows significant fluctuations with no clear increasing or decreasing trend, highlighting the dynamic nature of water transfer between the land and atmosphere.
Surface Temperature: Gradual increase over the years, indicating warming trends in the region.
🌐 Dataset and Reference

Dataset Used: NASA GLDAS-2.1 NOAH Land Surface Model
Source: Google Earth Engine




