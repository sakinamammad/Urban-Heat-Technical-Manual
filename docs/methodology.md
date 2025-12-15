# Methodology and Replication Instructions

## Step 0: Extracting and Preparing Input Data

### Extract Data

The first step is to gather all datasets required to support the heat, environmental, and social vulnerability analysis. Analysts should begin by obtaining administrative boundaries and/or creating a tessellation grid, which establishes the spatial framework for the assessment.

- Obtain administrative boundaries to define the study extent.
- If administrative boundaries are unavailable, generate a tessellation grid using the **Generate Tessellation** tool. The size of the grid is dependent on the intervention goals of the user; however, the Nairobi pilot analysis used a size of **2 km²**.
- Acquire heat exposure data. If needed, compute UHI by subtracting rural baseline temperatures from urban LST.
- Download vegetation data such as NDVI.
- Obtain built environment and population data (building footprints, population grids, land cover).
- Collect social vulnerability indicators such as unemployment, youth share, senior share, and disability.
- Download nightlights rasters as a proxy for electrification and density *(not required if social vulnerability data is available)*.

Ensure all datasets use compatible coordinate systems and appropriate spatial resolution before proceeding.

---

### Clip Data

After all datasets are extracted, analysts clip each layer to the study boundary to ensure consistency across inputs. Raster datasets such as nightlights should be clipped to the boundary using the **Clip Raster** tool so that illumination values reflect only the area of interest. The same approach is applied to vegetation rasters such as NDVI, ensuring that greenness metrics capture only the relevant urban extent. Clipping all layers at this stage avoids later discrepancies in zonal calculations, overlays, and joins.

- If using nightlight data, clip the nightlights raster to the administrative boundary using **Clip Raster**.
- Clip the NDVI raster using the same boundary to maintain consistency.
- Clip any additional raster layers (e.g., heat rasters, land cover) to ensure uniform spatial extent.




## Step 1: Measuring Heat – Urban Heat Island Effect

**Note:** This step can be skipped if you already have an alternative method for calculating the UHI effect and a raster ready for analysis.

In order to calculate the Urban Heat Island Effect, a customizable toolkit has been developed. The toolkit utilizes a Python script, and detailed documentation can be found in the following repository:

<https://github.com/nishansrepo/SUHIIAnalysisTool>

Our measure of Urban Heat Island Effect is **Surface Urban Heat Island Intensity (SUHII)**. SUHII quantifies how much hotter urban areas are compared to surrounding rural land, measured as:

**SUHII = μ(LSTurban) − μ(LSTrural)**


This metric is essential for heat vulnerability assessments because it identifies where within a city the built environment amplifies thermal exposure. When combined with social vulnerability indicators (e.g., age demographics, income levels, housing quality, access to cooling), SUHII enables the construction of a composite heat risk score that pinpoints populations facing compounded risk—areas that are both physically hotter and socially less equipped to cope.

SUHII-based risk mapping supports targeted interventions such as urban greening programs, cool roof initiatives, and heat-health early warning systems in the neighborhoods that need them most.

---

### Datasets

**Land Surface Temperature (LST)**  
- **Purpose:** Primary thermal input  
- **Recommended Source:** Landsat-8/9  

**Land Use Land Cover (LULC)**  
- **Purpose:** Urban classification; water pixel masking  
- **Recommended Source:** Dynamic World (10 m) (Brown et al., 2022) via Google Earth Engine  

**Normalized Difference Vegetation Index (NDVI)**  
- **Purpose:** Vegetation filtering  
- **Recommended Source:** Landsat-8/9 or Sentinel-2  

**Digital Elevation Model**  
- **Purpose:** Elevation correction  
- **Recommended Source:** SRTM (30 m) or ASTER GDEM  

**Administrative Boundary**  
- **Purpose:** Study area extent  
- **Sources:** Local government, GADM, OpenStreetMaps  

---

### How It Is Created

#### Urban Area Definition

This step defines which pixels represent the “urban” thermal signal. Three methods are available:

- **LULC-Based Method:**  
  Urban pixels are identified using land use/land cover classification products (e.g., Dynamic World Class 6 = Built Area). This method leverages validated classification products and provides consistent definitions across studies (Raj & Yun, 2024).

- **NDVI-Based Method:**  
  Urban pixels are identified using a vegetation index threshold (NDVI < 0.3), based on the strong inverse correlation between LST and NDVI (Ahmad et al., 2024).  
  **Warning:** This method cannot distinguish urban areas from bare soil or water and should be used only when LULC data is unavailable.

- **Combined LULC + NDVI Method:**  
  Uses LULC classification to identify built-up areas, then applies an NDVI filter to exclude vegetated pixels (parks, urban forests). This isolates impervious surfaces that drive the UHI effect, preventing urban green spaces from biasing the urban mean downward (Zhou et al., 2014; Naidu & Chundeli, 2023).

**Water Body Exclusion:**  
All methods optionally exclude water bodies from the urban mask. Water has distinct thermal properties (high heat capacity and evaporative cooling) that would bias urban LST estimates (Mentaschi et al., 2022).

---

#### Rural Reference Selection

This step defines the baseline “rural” temperature against which urban temperatures are compared. Four methods are available:

- **Fixed Buffer Method:**  
  Creates a fixed-width annular buffer (default: 10 km) around the urban boundary. This is the most common approach and is easy to replicate across studies (Raj & Yun, 2024; Zhou et al., 2014).

- **Urban Halo Method:**  
  Skips an inner exclusion zone before the buffer begins, avoiding thermal contamination from the urban heat island “footprint” that extends beyond administrative boundaries through heat advection (Mentaschi et al., 2022). Recommended for large cities with a 1–3 km inner exclusion.

- **Three-Ring Method:**  
  Creates dynamically sized buffer zones scaled to city area using formulas from Fernandes et al. (2024). Three concentric zones are defined: Urban Adjacent (Ua), Future Urban Adjacent (FUa), and Peri-Urban (PUa). This enables consistent cross-city comparisons regardless of city size.

- **In-City Method:**  
  Selects rural reference pixels from within the administrative boundary itself (e.g., parks, urban forests, undeveloped land). This method is useful for intra-urban analysis or isolated cities where surrounding landscapes are climatically dissimilar. It additionally calculates **Normalized UHI** following Ahmad et al. (2024):  

## Step 2: Social Vulnerability

To identify populations most at risk under extreme heat, create a **Social Vulnerability Score** for each spatial unit. The goal of this step is to capture demographic and socioeconomic characteristics that influence how different communities experience and recover from exposure to heat. Because Nairobi’s population census data is published using relatively large enumeration boundaries, the pilot analysis combined census indicators with nightlight and building-density data to approximate sub-neighborhood variation (Andries et al., 2023). The efficacy of this approach has not been fully confirmed at the local level, so cities are strongly encouraged to use whatever local data may be available.

Census indicators are assigned to tessellations based on the sub-county they overlap the most. Together, these layers provide a more complete picture of vulnerability across the city.

---

### How to Create It

Generate the Social Vulnerability Score using a **Suitability Analysis Tool**, where each field represents a variable associated with social vulnerability. For this analysis, the following variables were included:

- **Unemployment** (positive relationship to vulnerability)  
- **Percentage of youth** (positive)  
- **Percentage of seniors** (positive)  
- **Percentage of people with disabilities** (positive)  
- **Mean electrification** (inverse relationship — lower electrification corresponds to higher vulnerability)

**Note:** If planners are using QGIS instead of ArcGIS Pro, this process can be replicated through a multi-criteria overlay or by manually standardizing inputs using a calculated field.

---

### Standardizing and Weighting Inputs

All measures are standardized so they can be compared on the same scale.

Weights can be set at the discretion of planners and may vary depending on local priorities. However, unless there is a clear rationale for prioritizing specific demographic groups, equal weighting across indicators is recommended.

For this analysis:

- **Building density** is weighted lower at **8%**
- All other vulnerability variables are weighted equally at **18.4%** each

---

### Combining Indicators

When using a suitability analysis approach, the Suitability Analysis Tool standardizes the input variables and applies the user-defined weights to produce a final Social Vulnerability Score for each tessellation.

---

### Output: Social Vulnerability Index

The resulting score is used as a **Social Vulnerability Index (SVI)** that enables comparison of relative vulnerability across spatial units within the city. The SVI can be classified into multiple levels (e.g., low to high vulnerability) to support visualization, grouping of areas by risk, and integration with other analytical components such as heat exposure metrics.


## Step 3: Calculating Holistic Heat Risk

After generating the SUHII layer (heat exposure) and the Social Vulnerability Index (SVI), the next step is to combine these two components into a single **Holistic Heat Risk Score**. This score captures both the physical intensity of heat and the degree to which local populations are socially or economically more sensitive to heat impacts. Combining these layers allows planners to identify locations where extreme temperatures intersect with high vulnerability, ensuring interventions are directed where they are most needed.

This approach follows the logic used in tree equity and heat-justice frameworks, where exposure and vulnerability together define relative risk. Because both layers were already normalized and summarized to the same **2 km² tessellations**, they can be combined directly.

---

### How It Is Created

The following formula is used to calculate Holistic Heat Risk for each spatial unit:
**Urban Heat Risk = SUHII × Social Vulnerability Index × 100**

This multiplicative approach ensures that areas with both high heat exposure and high vulnerability score the highest on the risk scale. If either exposure or vulnerability is low, the overall risk is moderated, aligning with the goal of identifying neighborhoods where both environmental and social burdens are elevated.

---

### Output: Holistic Heat Risk Map

The final result is a heat risk map that highlights priority zones for intervention. This layer serves as the bridge between **Steps 1 and 2** and the subsequent **3-30-300 assessment**, ensuring that greening and cooling strategies can be compared with underlying heat risk.

---

## Step 4: Identifying Areas Needing 3-30-300 Improvement

The final component of the analysis involves assessing each spatial unit against the **3-30-300 framework**, which evaluates access to trees and green space through three indicators:

- Visibility of at least **3 trees** from buildings  
- **30% tree canopy cover**  
- **300 meters** access to a public park  

The goal of this step is to understand where green-infrastructure gaps align with high heat exposure and high social vulnerability. By calculating each indicator at the tessellation level, it is possible to generate a consistent greenness profile for the entire city and identify areas that fall short of meeting the 3-30-300 standards (Browning et al., 2024).

---

### Three Trees Within View

This indicator evaluates tree visibility from buildings by assessing whether a minimum number of trees are present within a defined viewshed around each structure (Liang et al., 2023). The metric is intended to capture residents’ visual exposure to trees rather than overall canopy coverage.

Ideally, cities will have access to point-level locations of individual trees. In this case, tree visibility can be assessed directly by counting how many individual trees fall within each building’s viewshed buffer. If individual tree locations are not available, tree visibility can be approximated using buffer-based methods derived from vegetation or proxy datasets.

---

#### How It Is Created

- Generate building-level viewshed buffers to approximate the area from which trees may be visible.
- **If individual tree point data are available:**
  - Perform a spatial join to count the number of trees within each building’s viewshed buffer.
- **If individual tree point data are not available:**
  - Use buffered vegetation features or proxy tree layers to approximate tree presence within the viewshed area.
- Classify buildings based on tree visibility:
  - no data  
  - fewer than three trees  
  - at least three trees  
- Aggregate building-level results to tessellations by calculating the percentage of buildings meeting the three-tree threshold within each spatial unit.

---

### Thirty Percent Tree Canopy Cover

To assess canopy coverage, use NDVI from the summer season with less than **10% cloud cover**. Applying an NDVI threshold allows identification of areas of tree canopy within each tessellation.

---

#### How It Is Created

- Filter NDVI pixels using a **> 0.5** threshold for tree canopy.
- Calculate the canopy area within each tessellation.
- Compute percent canopy cover per unit.
- Classify each tessellation as either meeting or not meeting the **30% canopy goal**.

---

### Three Hundred Meters to a Park

To evaluate park access, digitize public parks within the city limits and measure building-level access to these green spaces.

---

#### How It Is Created

- Digitize parks inside the study area.
- Perform a **300 m near-table analysis** to determine which buildings are within 300 meters of a park.
- Classify buildings as inside or outside the 300 m boundary.
- Summarize results to tessellations by calculating the percentage of buildings within 300 m.

\* QGIS users can use the NNJoin Plugin: <https://plugins.qgis.org/plugins/NNJoin/>

---

### Greenness Index

Once the three indicators are calculated, they are combined into a single greenness score to summarize overall compliance with the 3-30-300 framework.

---

#### How It Is Created

- Equally weight the three components:
  - tree visibility  
  - canopy cover  
  - park access  
- Produce a **0–1 normalized greenness index** for each tessellation.

This greenness index allows direct comparison of green-infrastructure gaps across the city and serves as an essential input to the final priority mapping.

---

### Priority Areas for Intervention

Finally, the greenness index is combined with the Holistic Heat Risk Score to identify areas where **high social vulnerability** and **low greenness** overlap. These locations represent the highest-priority sites for heat-mitigating interventions such as tree planting, canopy restoration, and expanded park access.

---

#### How It Is Created

- Combine the Social Vulnerability Index and greenness results.
- Classify tessellations where SVI is high and the greenness score is low.
- Flag these as **priority intervention zones**.

---

## Dashboard Development (Optional for Analysis)

The dashboard is intended as a communication tool to help advocates and practitioners present results to policymakers and other non-technical audiences who may not have a detailed understanding of SUHI/SUHII or the 3-30-300 framework. While the dashboard is not required to complete the analysis, it provides a clear and accessible way to visualize results and support decision-making.

The dashboard produces a series of map layouts showing where high-priority areas overlap with locations targeted for tree planting and park construction.

---

### Foundational Analysis in a Web Map

To build the dashboard, users must first create a web map, either directly in **ArcGIS Online** or by uploading a map constructed in **ArcGIS Pro**. The web map should include the following layers:

- The **SUHII × Social Vulnerability** analysis, visualized using quantiles and a red color ramp.
- A visualization of the **Greenness Score**.
- A layer highlighting areas of noncompliance with the **“300”** element of the framework.
- A layer highlighting areas of noncompliance with the **“3–30”** elements of the framework.
- A priority sketch layer highlighting areas of overlap, with pop-ups edited to include findings and potential interventions.

This web map serves as the foundational element of the dashboard.

---

### Expanding the Dashboard

Once the layers have been uploaded into a web map, create a new dashboard draft in ArcGIS Online and add the web map. Add a title bar, a legend, and two sidebars. Text from the Nairobi Demonstration document may be copied and edited as needed to support visualization and interpretation.

---

### Filters

To enable interactive exploration, filters can be added to the dashboard.

#### Heat Risk Filter

- Add a **Number Selector** element to the title bar.
- Set the display type to **Combination** with a **Range** input type.
- Enable the **Show Reset** option.
- Under **Limits from**, select **Statistic**.
- Choose the **Heat Risk** layer and select the combined Social Vulnerability × SUHII field.
- Set **Precision** to 1 and **Increment** to 1.
- Under **Actions**, configure the filter so the Heat Risk layer is active only when filtered.

#### Plant Trees Filter

- Add a second Number Selector element to the title bar.
- Use a Combination display with Range inputs and enable the reset option.
- Under Limits from, select Statistic.
- Choose the Where to Plant Trees layer.
- For the filter condition, use “Plant Trees” is equal to “Plant Trees in this Area,” as defined in the previous analyses.
- For the Field, select the tree canopy cover percentage variable.
- Set Precision to 1 and Increment to 1.
- Under Actions, confirm that the filter configuration matches the intended screen layout.

#### Using the Dashboard
These elements, in combination, allow you to identify areas ripe for intervention.

