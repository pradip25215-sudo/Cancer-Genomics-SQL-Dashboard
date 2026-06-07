#Cancer Genomics SQL Dashboard
#End-to-end data science pipeline analyzing 2,000 cancer patients across 12 cancer types. Demonstrates the complete data scientist workflow: ETL → SQL → Statistics → Survival Analysis → Interactive Dashboard.
What This Project Demonstrates
Skill Implementation SQL Database Design 2-table SQLite with indexes. Advanced SQL JOINs, CTEs, aggregates, subqueries, ETL Pipeline, Raw data → cleaned → SQL database, Statistical Analysis, Chi-square, log-rank testsSurvival AnalysisKaplan-Meier curvesMultivariate StatisticsCox Proportional Hazards model, Data Visualization, Matplotlib + Seaborn + Plotly, Interactive Dashboard, Streamlit multi-page app, Domain Knowledge, Cancer genomics, clinical outcomes
Dataset

2,000 simulated TCGA-style cancer patients
12 cancer types (BRCA, LUAD, COAD, PRAD, GBM, OV, STAD, LIHC, KIRC, HNSC, PAAD, BLCA)
7,571 mutations across 37 cancer-specific genes
Real biology: TP53, KRAS, EGFR, BRCA1, APC, PIK3CA, PTEN, etc.

Key Findings

TP53 mutations associated with worse survival (log-rank test)
Stage IV patients have substantially higher mortality than Stage I
Top co-occurring mutations identified via self-join SQL queries
Cox model quantifies survival risk factors (stage, age, mutations)

Visualizations
Mutation Frequency Analysis
Show Image
Gene x Cancer Heatmap
Show Image
Kaplan-Meier Survival Curves
Show Image
Cox Proportional Hazards Model
Show Image
SQL Skills Demonstrated
Example query — top mutations per cancer type using JOIN + subquery + aggregates:
sqlSELECT
    p.cancer_type,
    m.gene,
    COUNT(*) as mutation_count,
    ROUND(100.0 * COUNT(DISTINCT p.patient_id) /
          (SELECT COUNT(*) FROM patients WHERE cancer_type = p.cancer_type), 2) as pct_patients
FROM patients p
JOIN mutations m ON p.patient_id = m.patient_id
GROUP BY p.cancer_type, m.gene
HAVING mutation_count > 20
ORDER BY p.cancer_type, mutation_count DESC;
Example query — survival statistics using Common Table Expression (CTE):
sqlWITH stage_stats AS (
    SELECT
        stage,
        COUNT(*) as n_patients,
        AVG(survival_months) as avg_survival,
        AVG(CASE WHEN vital_status = 'Dead' THEN 1.0 ELSE 0.0 END) as death_rate
    FROM patients
    GROUP BY stage
)
SELECT stage, n_patients,
       ROUND(avg_survival, 1) as avg_survival_months,
       ROUND(death_rate * 100, 1) as mortality_pct
FROM stage_stats
ORDER BY stage;
How to Run
bashpip install pandas numpy matplotlib seaborn plotly scipy lifelines sqlalchemy
Open Cancer_Genomics_SQL_Dashboard.ipynb in Google Colab (no GPU needed). Takes ~15 minutes.
For the Streamlit dashboard:
bashpip install streamlit
streamlit run dashboard.py
Industry Relevance
This is the exact skillset clinical genomics companies need:

MedGenome Labs — Clinical bioinformatics analyst
Strand Life Sciences — Cancer genomics data scientist
Tata Memorial Hospital — Clinical data analyst
Apollo Genomics — Healthcare informatics
AstraZeneca / Novartis — Real-World Evidence (RWE) analyst

Tools
Python · Pandas · NumPy · SQLite · SQL · Matplotlib · Seaborn · Plotly · Lifelines · scipy · Streamlit
Honest Limitations

Patient data is simulated (uses realistic TCGA mutation frequencies but not real patients)
Survival times include controlled randomness for demonstration
Production use would require real TCGA/cBioPortal API integration
2,000 patients is small; real clinical databases have 100,000+

Future Improvements

Connect to live cBioPortal API for real patient data
Add gene expression integration (RNA-seq)
Deep learning survival models (DeepSurv)
Multi-omics integration (mutation + expression + clinical)
Deploy Streamlit app to cloud (Streamlit Cloud / HuggingFace Spaces)

Project Structure
Cancer-Genomics-SQL-Dashboard/
├── README.md
├── Cancer_Genomics_SQL_Dashboard.ipynb   # main notebook
├── dashboard.py                           # Streamlit app
├── cancer_genomics.db                     # SQLite database
├── mutation_frequencies.png
├── gene_cancer_heatmap.png
├── survival_analysis.png
└── cox_model.png

Pradip Palekar | MT25215 | M.Tech Computational Biology, IIIT Delhi
pradip25215@iiitd.ac.in
