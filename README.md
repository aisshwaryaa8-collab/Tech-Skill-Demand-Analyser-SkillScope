## SkillScope

A data science project analyzing tech job market trends using live job posting data from the Adzuna API, built as part of the Thiranex internship.

## Overview

SkillScope collects and analyzes technical job postings across the US, India, and the UK to answer two questions:

Which tech skills are most in demand, and which tend to be requested together?
Can required skills predict whether a posting pays above or below its market's median salary?

## Task 1: Tech Skill Demand Analysis

## SkillScope_Task1_DataCollection.ipynb

Data collection: Pulled job postings via the Adzuna API across 3 countries (US, India, GB) using 10 role-based search keywords (e.g. Python Developer, Data Scientist, DevOps Engineer), deduplicated by job ID.
Skill extraction: Used a whitelist-based regex approach (~40 tech skills — languages, frameworks, cloud platforms, databases, tools) matched against each posting's title and description, chosen for precision and interpretability over free-text NLP.
Analysis: Computed skill frequency and skill co-occurrence (pairs), then visualized results.

## Key results (2,959 postings analyzed):

78.9% of postings mentioned at least one whitelisted skill
Most in-demand skill: Python (451 postings)
Top 3 skills overall: Python, AWS, Java
Most common skill pairing: JavaScript + React (227 postings)

**Files**: adzuna_jobs_with_skills.csv, skill_frequency.csv, skill_pairs.csv, top_skills.png, skill_heatmap.png, salary_by_skill.png, key_findings.txt

## Task 2: Predictive Modeling

## SkillScope_Task2_PredictiveModeling.ipynb

Goal: Predict whether a job posting pays above or below its country's median salary, using required skills, skill count, and country as features.
Target design: Initially explored predicting individual skill presence, but skill frequencies were too imbalanced (~15–20%) for a meaningful classification target. Also discovered salary_max mixed raw local currencies (USD/GBP/INR) across countries, making a single global salary threshold misleading. Fixed by computing the median within each country and classifying postings as above/below their own country's median — yielding a clean ~50/50 class balance.
Features: One-hot encoded skills (from skills_str), one-hot encoded country, and skill count — 47 features total across 2,479 postings with valid salary data.
Models compared: Logistic Regression, Decision Tree, Random Forest (scikit-learn defaults).

## Results:

Model	Accuracy	ROC-AUC
Logistic Regression	59.3%	0.63
Decision Tree	59.5%	0.62
Random Forest	61.1%	0.65

Random Forest performed best across both metrics, likely due to its ability to capture non-linear interactions between co-occurring skills. Skills and country provide moderate predictive signal for salary tier, but likely miss key drivers not present in this dataset — seniority, years of experience, and company size.

**Files**: confusion_matrices_task2.png, roc_curves_task2.png, key_findings_task2.txt

## Tech Stack

Python, pandas, scikit-learn, matplotlib, seaborn, Adzuna API

## Author

Aisshwarya
