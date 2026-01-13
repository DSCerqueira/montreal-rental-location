**Data pipeline, exploratory phase, and KPIs**

**Data Pipeline Architecture and Transformation Layers**

In my project, the data pipeline is organized into four transformation layers: **Bronze, Silver, Gold, and Presentation**. Each layer has a specific role in ensuring **data quality, scalability, and usability** for analytics and BI consumption.

**Data Ingestion - Raw and Bronze Layers**

- **Copy activity** to ingest data into the raw folder.
- **Copy activity** to move and create tables in the Bronze layer.
- **Notebook activity** to web scrape rental data, store it in the raw folder, and generate the Bronze table.

**Data Transformation - Silver and Gold Layers**

- **Spark SQL notebooks** for data transformations.
- **Python notebooks** for complex transformations, such as linking infractions data to postal codes.
- **Spark SQL notebooks** to summarize, aggregate, and categorize data.

**Data Presentation - Presentation Layer and Semantic Model**

- **Spark SQL notebooks** to create tables in the Presentation layer.
- **Semantic model** connected to the Presentation layer using **Direct Lake mode**.

I am using the semantic model in Direct Lake to leverage VertiPaq engine for fast in-memory analytics while taking advantage of Direct Lake's optimized querying capabilities for large-scale data.

**Bronze Layer (Raw Ingestion)**

The Bronze layer ingests data directly from the raw source folders. At this stage, **only the necessary columns are selected and loaded**, without applying any transformations. The goal of this layer is to preserve the original data structure while limiting the dataset to relevant fields. No data cleansing or business logic is applied in this layer.

Target users for the gold layer:

- Data engineers
- IT department

**Silver Layer (Data Cleansing and Business Rules)**

The Silver layer is where the **first and most significant transformations occur**. In this layer:

- Duplicate records (such as repeated advertisements) are removed
- Null values are either eliminated or replaced with meaningful defaults
- Data inconsistencies and ambiguities are identified and treated
- Business rules are applied, such as **linking each advertisement to its corresponding neighborhood**

These enrichments are essential for enabling accurate aggregations and analyses in the downstream layers.

Target users for the gold layer:

- Data engineers

**Gold Layer (Analytical Data Model)**

The Gold layer serves as the **foundation for the analytical data model**. Here:

- **Fact and Dimension tables** are clearly defined
- Aggregations and summarizations are created
- Statistical analyses are applied to **identify and remove outliers**, reducing bias in the results

At this stage, the data is largely ready for consumption by BI tools, following analytical best practices.

Target users for the gold layer:

- Data scientists
- Data analysts
- Data engineers

**Presentation Layer (BI-Optimized Model)**

Based on practical experience, I identified the need for an additional layer to better support BI tools such as **Power BI**. The **Presentation layer**, built on top of the Gold layer, is designed to:

- Create a data model optimized for specific project and reporting needs
- Apply additional aggregations and filters
- Build tailored tables to address project-specific requirements

This layer represents the **final, refined data model**, fully aligned with the project's analytical goals and optimized for performance and usability in BI tools.

Target users for the presentation layer:

- Data analysts
- Business analysts

**Rental Data Statistical Analysis and Outlier Treatment**

Statistical analysis of rental data, including **outlier detection**, is an integral part of the pipeline. The outlier removal process runs **weekly during the data refresh cycle**. Listings with values **below the 1st percentile or above the 99th percentile** are excluded, as well as advertisements missing the number of bedrooms. This approach helps ensure reliable statistics and minimizes distortion caused by extreme or incomplete values.
<img width="682" height="359" alt="statistic_1" src="https://github.com/user-attachments/assets/107db70b-4603-41ed-b110-d25333a46ad3" />

<img width="680" height="357" alt="statistic_2" src="https://github.com/user-attachments/assets/efde0482-9045-48c3-81d2-015cc4651fb1" />



**KPIs and method used**

The objective of the dashboard is to support users in selecting the most suitable location in Montreal based on their individual priorities. The following KPIs enable data-driven decision-making by evaluating each borough against key criteria related to affordability, safety, accessibility, and public services:

- Average of Price
- Infraction ratio per 1,000 habitants
- Average of walk score
- Average of transit score
- Average of commute time score\*
- School place ratio per 5 to 19 years old habitants aged
- Secondary school grade
- Day care ratio per 0 to 4 years old habitants aged

**KPI methodology**

The Island of Montreal is divided by locations. Each location is ranked from 1 to number of locations for each evaluation criterion. To standardize comparisons across locations, the ranks are normalized by dividing them by the total number of locations. The normalized rank represents the score of a location for a given criterion and is calculated as:

The scores are calculated dynamically in Power BI using DAX expressions, allowing results to automatically adapt to user selections and filter context. Users can assign a relevance level to each criterion, ranging from zero (not relevant) to high relevance. These relevance levels are used as weights in the calculation of the final location score, enabling the ranking of locations based on individual user priorities.

At the end of the calculation, the dashboard will highlight the **top three locations**, ranked from 1 to 3. The **number 1 location** represents the best choice for the user, based on the weighted relevance of the selected criteria. The ranking is fully dynamic and updates automatically according to the user's preferences.

**Data pipeline summary**

Raw Data

| **Data** | **Description** | **Data Source** | **Link** |
| --- | --- | --- | --- |
| rental Data | A spark notebook run a web scrape collecting data from Kijiji and saving into raw folder | Kijiji | <https://www.kijiji.ca/> |
| Montreal infractions | number of infractions registered in Montreal | Montreal open data | <https://donnees.montreal.ca/> |
| Census Data | Population by Montreal postal code with age group. | Statistic Canada | [https://www12.statcan.gc.ca](https://www12.statcan.gc.ca/) |
| Walk and transit score | Walk and transit score data are gathering | Walkscore | <https://www.walkscore.com/> |
| Commute time | HeiGit - Open geodata used to calculate time travel | HeiGit | <https://heigit.org/> |
| Day care | Data about the number of places by day care | Quebec open data | <https://www.donneesquebec.ca/> |
| Schools | Number of enrolled students by pre, primary, and secondary school.  <br>Secondary school evaluation (grade) | Quebec open data  <br>Fraser Institute | <https://www.donneesquebec.ca/>  <br><https://www.fraserinstitute.org/> |

**Medallion architecture and presentation layer**

**Semantic Model - Star schema**

Dimension table:

- mtl_locations

Fact tables:

- mtl_rent
- mtl_infraction_year
- mtl_age_group
- mtl_walkscore_commute_time
- mtl_daycare
- mtl_schools

The dimension table is linked to the fact tables via postal codes, which are grouped into locations.
