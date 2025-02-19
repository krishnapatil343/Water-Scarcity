# 🌏 Historical Water Scarcity Analysis using Google Earth Engine


This project analyses historical water scarcity trends using the **NASA GLDAS-2.1 NOAH Land Surface Model** dataset from **Google Earth Engine**. It visualizes three critical indicators:
- 📉 **Soil Moisture**
- 🌤 **Evapotranspiration**
- 🌡 **Surface Temperature**

The project focuses on **Any Location** and can be easily adapted worldwide by changing the coordinates. 
<img width="1024" alt="Image" src="https://github.com/user-attachments/assets/b81a49ab-1e24-41ff-b28d-484bd3e44a4b" />
---

## 🚀 Features
- Extracts historical water scarcity trend data from the **GLDAS dataset**.
- Visualizes time-series charts for:
  - Soil Moisture (m³/m³)
  - Evapotranspiration (mm/day)
  - Surface Temperature (K)
- Highlights the location with a **red spot** on the map for clear spatial reference.
- Focuses on the surrounding area of Shanghai Central Plaza, providing regional context.

---

## ⚙️ How to Use

**Set Up Google Earth Engine:**
   - Make sure you have access to [Google Earth Engine](https://earthengine.google.com/). 
   - Use the **Code Editor** to paste and run the script.

**Change Location:**
   - To adapt this analysis for any location worldwide:
     - Replace the coordinates in the following line with your desired location:
       ```js
       var shanghaiPoint = ee.Geometry.Point([longitude, latitude]).buffer(5000);
       ```
     - Example for New York:
       ```js
       var newyorkPoint = ee.Geometry.Point([-74.006, 40.7128]).buffer(5000);
       ```

**Adjust Time Range:**
   - You can modify the time range to your desired years:
     ```js
     var startYear = 2002;
     var endYear = 2024;
     var years = ee.List.sequence(startYear, endYear);
     ```

---

## 📊 Graphs and Visualizations

- **Time-Series Charts**:
  - Soil Moisture (2002-2024)
  - Evapotranspiration (2002-2024)
  - Surface Temperature (2002-2024)

- **Observations**:
  - **Soil Moisture**: High variability with significant peaks and troughs. The fluctuations are likely linked to precipitation patterns and evapotranspiration rates.
  - **Evapotranspiration**: Shows significant fluctuations with no clear increasing or decreasing trend, highlighting the dynamic nature of water transfer between the land and atmosphere.
  - **Surface Temperature**: Gradual increase over the years, indicating warming trends in the region.

---

## 🌐 Dataset and Reference

- **Dataset Used**: [NASA GLDAS-2.1 NOAH Land Surface Model](https://developers.google.com/earth-engine/datasets/catalog/NASA_GLDAS_V021_NOAH_G025_T3H)
- **Source**: Google Earth Engine

---

## 🛠 Customization and Contribution

1. **Fork the Repository**:
   - Click on **Fork** at the top right of this repository.

2. **Clone the Repository**:
   ```sh
   git clone https://github.com/YOUR-USERNAME/historical-water-scarcity.git
   cd historical-water-scarcity
   ```

3. **Change Location and Time Span**:
   - Update the location coordinates and time span directly in the script.

4. **Push Changes**:
   ```sh
   git add .
   git commit -m "Changed location to XYZ and updated time span"
   git push origin main
   ```
### Not For Use ###
// Define the location of interest (Shanghai, Central Plaza) with a buffer
var Location = ee.Geometry.Point([21.4508844, 3.21345185]).buffer(5000);  // 5km buffer for regional context

// Define the time range
var startYear = 2002;
var endYear = 2024;
var years = ee.List.sequence(startYear, endYear);

// Load GLDAS dataset for water scarcity analysis
var gldasDataset = ee.ImageCollection('NASA/GLDAS/V021/NOAH/G025/T3H');

// Function to extract key water scarcity indicators
var extractWaterScarcityData = function(year) {
  var startDate = ee.Date.fromYMD(year, 1, 1);
  var endDate = ee.Date.fromYMD(year, 12, 31);
  
  var yearlyData = gldasDataset.filterDate(startDate, endDate).mean();
  
  var soilMoisture = yearlyData.select('SoilMoi0_10cm_inst').reduceRegion({
    reducer: ee.Reducer.mean(),
    geometry: shanghaiPoint,
    scale: 5000,
    maxPixels: 1e9
  }).get('SoilMoi0_10cm_inst');
  
  var evapotranspiration = yearlyData.select('Evap_tavg').reduceRegion({
    reducer: ee.Reducer.mean(),
    geometry: shanghaiPoint,
    scale: 5000,
    maxPixels: 1e9
  }).get('Evap_tavg');
  
  var avgTemp = yearlyData.select('AvgSurfT_inst').reduceRegion({
    reducer: ee.Reducer.mean(),
    geometry: shanghaiPoint,
    scale: 5000,
    maxPixels: 1e9
  }).get('AvgSurfT_inst');
  
  return ee.Feature(null, {
    'year': ee.Number(year).format('%d'), 
    'soilMoisture': soilMoisture,
    'evapotranspiration': evapotranspiration,
    'avgTemp': avgTemp
  });
};

// Generate historical water scarcity data
var waterScarcityData = ee.FeatureCollection(years.map(extractWaterScarcityData));

// Print the historical data for verification
print("Historical Water Scarcity Data for Shanghai Central Plaza:", waterScarcityData);

// Highlight Shanghai, Central Plaza location on the map
var pointLayer = ee.FeatureCollection([ee.Feature(shanghaiPoint)]);
Map.addLayer(pointLayer, {color: 'FF0000'}, 'Shanghai, Central Plaza');

// Center the map on the location
Map.setCenter(121.4508844, 31.21345185, 10);

// Change Basemap for Better Contrast
Map.setOptions('TERRAIN');

// Time-series Charts for Historical Water Scarcity Analysis
var soilMoistureChart = ui.Chart.feature.byFeature(waterScarcityData, 'year', 'soilMoisture')
  .setChartType('LineChart')
  .setOptions({
    title: 'Historical Soil Moisture (Shanghai)',
    hAxis: {title: 'Year', format: '####'},
    vAxis: {title: 'Soil Moisture (m³/m³)'},
    lineWidth: 2,
    pointSize: 4,
    colors: ['#3182bd']
  });

var evapChart = ui.Chart.feature.byFeature(waterScarcityData, 'year', 'evapotranspiration')
  .setChartType('LineChart')
  .setOptions({
    title: 'Historical Evapotranspiration (Shanghai)',
    hAxis: {title: 'Year', format: '####'},
    vAxis: {title: 'Evapotranspiration (mm/day)'},
    lineWidth: 2,
    pointSize: 4,
    colors: ['#31a354']
  });

var tempChart = ui.Chart.feature.byFeature(waterScarcityData, 'year', 'avgTemp')
  .setChartType('LineChart')
  .setOptions({
    title: 'Historical Surface Temperature (Shanghai)',
    hAxis: {title: 'Year', format: '####'},
    vAxis: {title: 'Avg Surface Temperature (K)'},
    lineWidth: 2,
    pointSize: 4,
    colors: ['#e34a33']
  });

// Display Charts
print(soilMoistureChart);
print(evapChart);
print(tempChart);






**Pull Requests**:
   - Create a pull request for review and merge.
**Give a star 🌟 if you find this useful! Happy Analyzing!**
