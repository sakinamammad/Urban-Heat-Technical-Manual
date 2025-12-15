## Ideal & Minimum Standard Datasets

This section outlines the minimum datasets required to run the heat risk assessment, along with ideal datasets that can strengthen the analysis when available. Because the methodology is designed to be replicable across diverse cities with varying data environments, each dataset is categorized as either **Required (Minimum)** or **Optional (Ideal)**.

The datasets fall into these analytical domains:

- Administrative Boundaries and Spatial Units  
- Heat Exposure  
- Vegetation and Green Space (3–30–300 indicators)  
- Built Environment  
- Social Vulnerability  
- Health and Environmental Risk Indicators *(Optional)*  

These domains reflect the project’s analytical priorities and the core components of the 3–30–300 framework.

### Required and Optional Datasets

| Category | Purpose | Ideal Datasets | Minimum Required Datasets | Potential Resources |
|--------|--------|---------------|---------------------------|---------------------|
| Administrative Boundaries | Defines the analysis extent and demographic boundaries | High-resolution administrative micro-units | City boundary; basic administrative units (wards, districts) | National stats portals / Generate Spatial Units |
| Urban Heat Exposure | Measures heat intensity | Urban Heat Island (UHI) index computed from rural–urban temperature differences | Land Surface Temperature (LST) | Landsat 8 |
| Vegetation and Tree Canopy | Measures compliance with 30-number trees and tree canopy area | Tree-level canopy data or LiDAR vegetation structure | NDVI raster, [tree counts / survey / GSV] | NDVI |
| Built Environment | Captures urban form and heat absorption | Building height, materials, albedo, and impervious surface data | Building footprints | Google Open Buildings |
| Social Vulnerability Indicators | Proxy for density and electrification* | Local Population Survey (age, unemployment, electrification) | VIIRS mean brightness | NASA / Local Population and Housing Survey or census |
| Optional Environmental Enhancements | Improves exposure characterization | Humidity, heat index, PM2.5, or surface imperviousness | None required | ERA5, WorldCover |

\*The indicators listed above are intended to capture social and demographic characteristics that increase individual and community-level risk under extreme heat conditions, as defined in the World Bank Group Heat Risk framework.

**Note:** Optional datasets mentioned above are provided only as suggestions to enable more advanced analyses and are not included in the technical manual instructions for implementing these elements.

