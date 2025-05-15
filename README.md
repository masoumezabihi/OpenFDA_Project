##  Insights from Data Exploration

This analysis focuses on a single JSON file from the **OpenFDA Drug Adverse Events** dataset, part of a larger collection of FDA records. The file was selected as a representative sample to examine the structure, completeness, and analytical readiness of the data.

---

###  Data Structure

The dataset is delivered in a deeply nested JSON format, containing multiple lists and objects, including:

- A top-level report structure
- Patient information
- Nested lists of `drug` and `reaction` entries
- `openfda` metadata fields nested within the drug section

The data was successfully **flattened** using `pandas.json_normalize()` and then further normalized by **exploding list-type columns** where necessary.

---

###  Column Homogeneity & Constant Fields

During exploration, several columns were found to contain the **same constant value** across all records. These fields provide no useful variance and can be safely removed or deprioritized in downstream analysis.

The following fields were found to be constant in this file:

- `transmissiondateformat`: `102`
- `receivedateformat`: `102`
- `receiptdateformat`: `102`
- `receiver`: `NaN`
- `sender.senderorganization`: `"FDA-Public Use"`
- `patient.patientdeath.patientdeathdateformat`: `NaN`
- `patient.patientdeath.patientdeathdate`: `NaN`

Additional constant fields may exist in the `drug` table. A general-purpose function was used to programmatically detect such columns across all relevant sections.

---

###  Missing Data and Data Quality

Several fields were found to contain **significant missing values**, often exceeding 50%. This guided early decisions about which fields to retain or drop depending on the goals of future analysis or modeling.

---

###  Exploding List-Based Fields

Many fields, such as `drug.openfda.application_number`, `route`, `substance_name`, and `brand_name`, were stored as lists.

These were individually exploded to convert the data into a normalized, analysis-friendly tabular format.

Exploding was done **column by column** to handle cases where list lengths were inconsistent between fields.

---

###  Conclusion

The exploratory data analysis revealed that while the OpenFDA adverse event data is rich in detail, it requires **careful flattening, cleaning, and feature selection**. Identifying constant fields and handling nested structures were key steps in preparing the dataset for downstream analysis, such as detecting drug-reaction signals or analyzing reporting trends.
