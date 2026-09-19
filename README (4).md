# Mental Health in Tech — EDA

Exploratory data analysis of the **2014 OSMI (Open Sourcing Mental Illness) Mental Health in Tech Survey**, examining how demographics, workplace characteristics, and workplace mental-health support relate to whether tech employees seek treatment for a mental health condition.

## Project Type
Individual EDA project.

## Problem Statement

Mental health remains a significantly under-discussed topic in tech workplaces, despite the high-pressure, high-stress nature of the industry. Employers often struggle to know whether the benefits and policies they offer (health coverage, wellness programs, anonymity guarantees, care-option awareness) actually translate into employees seeking treatment, and whether fear of stigma is undermining those efforts.

This project analyzes the survey to answer:
- What proportion of tech employees have sought treatment for a mental health condition, and how does this vary by demographic and workplace factors?
- Which workplace policies/attitudes (benefits, care options, anonymity, leave policy, fear of consequences) are most strongly associated with treatment-seeking behavior?
- Does company size, remote-work status, or employer type (tech vs. non-tech) meaningfully affect the availability of mental health support?
- Are there patterns that could help a company identify where its mental health support is falling short?

## Business Objective

Help HR / People teams at tech companies identify which workplace factors most strongly influence whether employees seek mental health treatment, so they can prioritize investments (e.g., clearer communication of benefits, anonymity guarantees, manager training) most likely to increase treatment-seeking and reduce stigma.

## Dataset

- **Source:** OSMI Mental Health in Tech Survey (2014)
- **Size:** 1,259 responses × 27 columns
- **Type:** Mostly categorical, self-reported survey data; `Age` is the only true numeric column
- **Key outcome variable:** `treatment` — whether the respondent has sought treatment for a mental health condition

Key fields include demographics (`Age`, `Gender`, `Country`), workplace attributes (`no_employees`, `remote_work`, `tech_company`), and mental-health-related workplace policy variables (`benefits`, `care_options`, `wellness_program`, `anonymity`, `leave`, `mental_health_consequence`, `family_history`, `work_interfere`, `coworkers`, `supervisor`).

## Data Cleaning

- Parsed `Timestamp` to datetime, then dropped it (not relevant to this analysis).
- Replaced invalid `Age` values (negative numbers, e.g. `-1726`, and absurd values like `99999999999`) with `NaN`, then imputed with the median age (~31).
- Standardized `Gender` from 40+ free-text variants (`M`, `m`, `Malr`, `Femake`, `queer`, etc.) into three categories: `Male`, `Female`, `Other/Non-binary`.
- Filled `self_employed` missing values with `No` (majority class) and `work_interfere` missing values with `Not applicable` (per survey skip-logic — this field is only shown to respondents with a mental health condition).
- Dropped `comments` (~87% missing, free text) and `state` (~40% missing, only applies to US respondents).

## Approach

The notebook follows a structured **Univariate → Bivariate → Multivariate (UBM)** workflow with 23 charts:

1. **Univariate** — distributions of Age, Gender, Treatment, Family History, Work Interference, Company Size, Remote Work, Country, and Benefits.
2. **Bivariate** — treatment-seeking broken down by Age, Gender, Family History, Work Interference, Care Options, Benefits, Anonymity, Leave Ease, and Company Size.
3. **Multivariate** — combined views of Gender × Family History × Treatment, and Age × Gender × Treatment.
4. **Correlation heatmap** (on label-encoded features) and a **pair plot** to round out relationship analysis.

## Key Findings

- **`family_history`, `work_interfere`, and `care_options`** are the three strongest correlates of treatment-seeking.
- Respondents with a family history of mental illness seek treatment at a much higher rate (~75%) than those without (~35%).
- Treatment-seeking rises sharply with work-interference severity — nearly all respondents reporting "Often" interference have sought treatment.
- Awareness of care options matters more than the benefit itself: employees who know their care options seek treatment at ~65% vs. ~35–40% for "No"/"Not sure."
- A large share of respondents answered **"Don't know"** (not "No") to benefits, anonymity, and care-option questions — pointing to a communication gap rather than a policy gap.
- Male respondents seek treatment less often than female and non-binary respondents.
- Age and remote-work status show little to no relationship with treatment-seeking.
- Larger companies (500+ employees) are far more likely to formally provide mental health benefits than small companies/startups.
- The sample skews heavily male (~79%) and US-based (~60%), which should be considered when generalizing results.

## Recommendations

1. **Prioritize communication over new policy creation** — clarify existing benefits, anonymity, and care options rather than adding new ones.
2. **Train managers** to recognize early work-interference signals and proactively point employees to resources.
3. **Make care-option and benefit information easy to find** — awareness matters more than existence.
4. **Address the gender gap** with messaging aimed at male employees, favoring anonymous/no-stigma channels.
5. **Support small companies differently** — low-cost alternatives (EAPs, stipends, telehealth) instead of full benefit packages.
6. **Simplify medical leave policy** for mental health to reduce fear of disclosure consequences.

⚠️ **Caveat:** Findings are skewed toward US, male respondents from 2014 — validate against your own company's demographic mix before applying.

## Tech Stack

- Python 3
- `pandas`, `numpy` — data wrangling
- `matplotlib`, `seaborn` — visualization
- `scikit-learn` (`LabelEncoder`) — encoding categorical features for the correlation heatmap

## Project Structure

```
.
├── Mental_Health_in_Tech_EDA.ipynb   # Main analysis notebook
├── survey.csv                        # OSMI 2014 survey dataset (not included — see below)
└── README.md
```

## Getting Started

1. Download the OSMI Mental Health in Tech Survey (2014) dataset as `survey.csv` and place it in the project root.
2. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn jupyter
   ```
3. Launch the notebook:
   ```bash
   jupyter notebook Mental_Health_in_Tech_EDA.ipynb
   ```
4. Run all cells to reproduce the cleaning steps, charts, and insights.

## Conclusion

Whether a tech employee seeks treatment for a mental health condition is driven far more by **awareness and communication of workplace support** (care options, benefits clarity, anonymity assurances) and by **personal/behavioral signals** (family history, degree of work interference) than by demographic factors like age or remote-work status. Improving how existing mental-health resources are communicated — rather than simply adding new ones — appears to be the highest-leverage lever available to HR teams.
