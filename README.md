# Night-Time-Lights
//Night time light monitoring to visualize developmental chnges in recent years.  
// Visual parameter - red to blue means old to new developments.  
//Code developed by Somdeep Kundu, PGD NHDRM 2022-23, Indian IInstitute of Remote Sensing, Dehradun 
//Google Earth Engine



// Load the three nighttime images
var dataset2014 = ee.ImageCollection('NOAA/VIIRS/DNB/MONTHLY_V1/VCMCFG')
                  .filter(ee.Filter.date('2014-04-01', '2015-04-30'));
                  //.filterBounds(geometry);
                  
var dataset2017 = ee.ImageCollection('NOAA/VIIRS/DNB/MONTHLY_V1/VCMCFG')
                  .filter(ee.Filter.date('2017-04-01', '2018-04-30'));
                  //.filterBounds(geometry);
                  
var dataset2021 = ee.ImageCollection('NOAA/VIIRS/DNB/MONTHLY_V1/VCMCFG')
                  .filter(ee.Filter.date('2021-04-01', '2022-04-30'));
                  //.filterBounds(geometry);

var nighttime2014 = dataset2014.select('avg_rad').median() ;//.clip(geometry) ;
var nighttime2017 = dataset2017.select('avg_rad').median() ;//.clip(geometry) ;
var nighttime2021 = dataset2021.select('avg_rad').median() ;//.clip(geometry) ;

// Specify the visualization parameters for the RGB composite
var visParams = {
  bands: ['avg_rad_median_2014', 'avg_rad_median_2017', 'avg_rad_median_2021'],
  min: 0,
  max: 5
};

// Create the RGB composite image by adding the three images as bands to a new image
var rgbImage = ee.Image().addBands(nighttime2021.rename('avg_rad_median_2021'))
                         .addBands(nighttime2017.rename('avg_rad_median_2017'))
                         .addBands(nighttime2014.rename('avg_rad_median_2014'))
                         .visualize(visParams);

// Display the RGB composite image
Map.addLayer(rgbImage, {}, 'RGB Composite Image');
