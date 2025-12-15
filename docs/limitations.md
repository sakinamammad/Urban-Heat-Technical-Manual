# Limitations and Future Work

## Limitations

While the workflow developed for this project provides a replicable approach to assessing urban heat risk and evaluating city-level performance against the **3-30-300 framework**, several limitations should be acknowledged. These limitations reflect constraints in data availability, methodological assumptions, and the absence of local validation at this stage.

---

### Tree Data Availability

The accuracy of the **“three trees within view”** and canopy metrics depends heavily on the quality and completeness of available tree data. In many cities, including Nairobi, publicly accessible datasets may be incomplete or outdated, which can lead to underestimation or misclassification of green cover.

---

### Park Definitions and Calculations

Park boundaries were digitized manually for this pilot analysis. As park quality, accessibility, and official boundaries vary across cities, results may differ depending on how parks are defined or which areas are included. These methodological differences can affect the **300-meter access** metric.

---

### Placement of Interventions

The analysis identifies priority tessellations but does not determine the optimal placement of specific interventions (e.g., where exactly to plant trees within a hexagon). More advanced siting tools or optimization models would be required to translate priority areas into project-ready intervention plans.

---

### QGIS Integration

Although the workflow was executed in **ArcGIS Pro**, full integration into **QGIS** has not yet been completed. Some processes, such as suitability analysis or near-table generation, may require alternative tools or manual adaptations in QGIS environments.

## Future Work

Several opportunities exist to expand and strengthen this analytical framework as additional data, tools, and local expertise become available. The following areas outline future work that can improve the accuracy, usability, and policy relevance of the heat-risk and **3-30-300 assessment**.

---

### Deep Learning for Tree Crown Identification

Future iterations of this project could incorporate deep learning methods to automatically detect individual tree crowns from high-resolution imagery. This would improve the accuracy of the **“three trees within view”** metric and reduce reliance on external or manually curated datasets.

---

### Optimization of Intervention Placement

While this analysis identifies priority tessellations, further work is needed to determine the optimal locations for specific interventions (e.g., where exactly to plant trees or expand canopy). Linear programming or other optimization approaches could support site-level decision-making and translate priority zones into actionable project plans.

---

### QGIS Plugin Development

Developing a **QGIS plugin** to automate key components of the workflow—including tessellation creation, zonal statistics, and SUHII filtering—would make the methodology more accessible for cities and organizations without access to ArcGIS licenses.

---

### Validation with Local Planners

Collaboration with Nairobi city planners and technical teams is essential to refine assumptions, verify feasibility, and ensure results align with local land-use contexts. Field validation and stakeholder feedback would strengthen the credibility and policy relevance of the final outputs.

---

### Integration with Broader Climate-Risk Tools

Future versions of this workflow could integrate urban heat metrics and **3-30-300** results with the World Bank’s broader climate-risk and urban resilience platforms. This integration would enable a more comprehensive, hyper-local understanding of vulnerability across multiple climate hazards.


