# Students Mental Health Analysis 
## Project Overview

This project analyzes how the **length of stay** impacts the **mental health diagnostic scores** of international students. Using SQL-based data analysis, we explore the relationship between acculturation duration and three key mental health indicators: depression, social connectedness, and acculturative stress.

 Objectives

- Analyze the correlation between length of stay and mental health outcomes
- Identify patterns in depression scores (PHQ-9) across different stay durations
- Examine social connectedness (SCS) trends among international students
- Evaluate acculturative stress (ASISS) levels based on years abroad
- Provide data-driven insights for university support services

---
 Understanding Mental Health Assessment

### How We Identify Mental Health Issues

This study uses **three validated psychological assessment instruments** commonly used in clinical and research settings to measure mental health:

---

1. **PHQ-9 (Patient Health Questionnaire-9)** - Depression Assessment

**Column:** `todep` (Total Depression Score)

**Purpose:** Measures depression severity over the past 2 weeks

**Score Range:** 0-27 points

**Clinical Interpretation:**
| Score Range | Severity Level | Meaning |
|-------------|----------------|---------|
| 0-4 | Minimal/None | No depression |
| 5-9 | Mild | Mild depressive symptoms |
| 10-14 | Moderate | Moderate depression requiring attention |
| 15-19 | Moderately Severe | Significant depression requiring treatment |
| 20-27 | Severe | Severe depression requiring immediate intervention |

**Assessment Areas:**
- Feeling down, depressed, or hopeless
- Little interest or pleasure in activities
- Sleep disturbances
- Fatigue or low energy
- Appetite changes
- Low self-esteem
- Concentration difficulties
- Psychomotor changes
- Thoughts of self-harm

**Why it matters for international students:**
Depression among international students can stem from academic pressure, cultural isolation, language barriers, and separation from family support systems.

---

2. **SCS (Social Connectedness Scale)** - Social Connection Assessment

**Column:** `tosc` (Total Social Connectedness Score)

**Purpose:** Measures feelings of belonging and connection to social environment

**Score Interpretation:**
- **Higher scores** = Greater social connectedness ✅ (POSITIVE indicator)
- **Lower scores** = Social isolation, loneliness ❌ (NEGATIVE indicator)

**Assessment Areas:**
- Sense of belonging to a community
- Feeling close to others
- Quality of meaningful relationships
- Feeling understood by peers
- Integration into social groups

**Why it matters for international students:**
International students face unique social challenges:
- Language barriers reducing communication
- Cultural differences creating distance
- Far from family and established support networks
- Difficulty making friends in new cultural context
- Limited participation in social activities

**Impact on mental health:**
Low social connectedness is strongly correlated with depression, anxiety, and poor academic performance.

---

 3. **ASISS (Acculturative Stress Scale for International Students)** - Cultural Adaptation Stress

**Column:** `toas` (Total Acculturative Stress Score)

**Purpose:** Measures stress specifically related to adapting to a new culture

**Score Interpretation:**
- **Higher scores** = Greater acculturative stress ❌ (NEGATIVE indicator)
- **Lower scores** = Better cultural adaptation ✅ (POSITIVE indicator)

**Stress Components Measured:**
| Component | Column | Description |
|-----------|--------|-------------|
| Discrimination | `apd` | Perceived discrimination or prejudice |
| Homesickness | `ahome` | Missing home country, family, culture |
| Fear/Anxiety | `aphaf` | Fear and anxiety in new environment |
| Change Stress | `acsag` | Stress from life changes and adaptation |
| Guilt | `aguilt` | Guilt about leaving family behind |
| Miscellaneous | `amiscell` | Other cultural stressors |

**Why it matters for international students:**
Acculturative stress is unique to international students and includes:
- Language barriers in academic and social settings
- Unfamiliar educational systems and expectations
- Immigration and visa concerns
- Financial pressures and limited work opportunities
- Different food, climate, and social norms
- Discrimination or feeling like an outsider
- Identity conflicts between home and host cultures

**Acculturation Timeline:**
Students often experience different stress patterns based on length of stay:
- **Year 1-2:** Initial culture shock, high stress
- **Year 3-4:** Adjustment period, mixed experiences
- **Year 5+:** Integration or chronic stress patterns

---

 Combined Mental Health Picture

**Example Analysis from Dataset:**

**Student Profile A:**
Length of Stay: 2 years (Medium)
Depression (PHQ-9): 9 (Mild) ❌
Social Connectedness: 41 ⚠️
Acculturative Stress: 140 (Very High) ❌❌
Suicide Ideation: Yes ❌❌❌


**Interpretation:** This student shows clear signs of mental health distress, likely due to being in the difficult adjustment period with high cultural stress and limited social support.

**Student Profile B:**
Length of Stay: 5 years (Long)
Depression (PHQ-9): 0 (None) ✅
Social Connectedness: 34 ⚠️
Acculturative Stress: 79 (Moderate) ⚠️
Suicide Ideation: No ✅


**Interpretation:** Despite lower social connectedness, this student shows no depression, suggesting successful long-term adaptation despite ongoing cultural challenges.

---

 Dataset Description

The dataset contains mental health data from **international and domestic students** with the following fields:

| Field Name | Description |
|------------|-------------|
| `inter_dom` | Student type (international or domestic) |
| `region` | Geographic region of origin |
| `gender` | Gender identity |
| `academic` | Academic level (undergraduate or graduate) |
| `age` | Current age of student |
| `stay` | **Current length of stay in years** (KEY VARIABLE) |
| `todep` | **Total PHQ-9 depression score** (0-27) |
| `tosc` | **Total SCS social connectedness score** |
| `toas` | **Total ASISS acculturative stress score** |
| `suicide` | Suicidal ideation indicator (Yes/No) |
| `dep` | Self-reported depression (Yes/No) |
| `deptype` | Depression type |
| `depsev` | Depression severity category |

**Additional stress subscales:** `apd`, `ahome`, `aphaf`, `acsag`, `aguilt`, `amiscell`

---

 Analysis Approach

### Research Question:
**Does the length of stay impact mental health diagnostic scores among international students?**

### Key Metrics Calculated:
1. **Count of International Students** per length of stay
2. **Average PHQ-9 Score** (Depression - rounded to 2 decimals)
3. **Average SCS Score** (Social Connectedness - rounded to 2 decimals)
4. **Average ASISS Score** (Acculturative Stress - rounded to 2 decimals)

### SQL Query

SELECT
stay, -- Length of stay in years
COUNT(*) AS count_int, -- Number of international students
ROUND(AVG(todep), 2) AS average_phq, -- Average PHQ-9 depression score
ROUND(AVG(tosc), 2) AS average_scs, -- Average SCS social connectedness
ROUND(AVG(toas), 2) AS average_as -- Average ASISS stress score
FROM
students
WHERE
inter_dom = 'Inter' -- Filter for international students
GROUP BY
stay -- Group by length of stay
ORDER BY
stay DESC -- Sort by stay duration (longest first)
LIMIT 9; -- Return top 9 results

text

---

## 📈 Key Findings & Hypotheses

### Possible Patterns to Explore:

**1. Early Arrival Culture Shock (Years 1-2):**
- Hypothesis: New international students experience highest depression and stress
- Expected: High `todep`, high `toas`, low `tosc`

**2. Mid-Stay Adjustment (Years 3-5):**
- Hypothesis: Students in middle years show improvement as they adapt
- Expected: Decreasing `todep` and `toas`, increasing `tosc`

**3. Long-Term Integration (Years 6+):**
- Hypothesis: Students with longer stays show better mental health outcomes
- Expected: Low `todep`, high `tosc`, moderate `toas`

**4. Chronic Stress Pattern:**
- Alternative hypothesis: Stress may increase over time without proper support
- Expected: Increasing `toas` even with longer stays

---

 Output Structure

The final result table contains:

| Column | Description | Data Type |
|--------|-------------|-----------|
| `stay` | Length of stay in years | Integer |
| `count_int` | Number of international students | Integer |
| `average_phq` | Average depression score (PHQ-9) | Decimal (2 places) |
| `average_scs` | Average social connectedness score | Decimal (2 places) |
| `average_as` | Average acculturative stress score | Decimal (2 places) |

**Rows:** 9 (sorted by stay duration in descending order)

---

 Insights & Applications

This analysis can help:

### For Universities:
- **Student Support Services:** Identify critical periods when students need mental health resources
- **International Student Offices:** Design targeted intervention programs based on length of stay
- **Orientation Programs:** Prepare incoming students for expected challenges
- **Counseling Services:** Allocate resources based on high-risk periods

### For Researchers:
- Understand acculturation patterns and mental health trajectories
- Identify protective factors for long-term student wellbeing
- Compare mental health outcomes across different stay durations
- Develop evidence-based intervention strategies

### For Policy Makers:
- Develop culturally-sensitive support systems
- Create early intervention programs for new arrivals
- Design long-term mental health monitoring systems
- Improve international student retention rates

---

 Technologies Used

- **SQL** - Data querying, aggregation, and filtering
- **Database Management** - Structured data analysis
- **Statistical Analysis** - Mean calculations and grouping
- **Data Modeling** - Mental health metrics interpretation

---

 How to Run

1. Ensure you have access to the students dataset
2. Execute the SQL query in your database environment (PostgreSQL, MySQL, SQL Server, etc.)
3. Results will display 9 rows with 5 columns as specified

-- Run this query in your SQL environment
SELECT
stay,
COUNT(*) AS count_int,
ROUND(AVG(todep), 2) AS average_phq,
ROUND(AVG(tosc), 2) AS average_scs,
ROUND(AVG(toas), 2) AS average_as
FROM
students
WHERE
inter_dom = 'Inter'
GROUP BY
stay
ORDER BY
stay DESC
LIMIT 9;

text

---

 About me

**Data Analyst & Business Intelligence Professional**

Passionate about using data analytics to solve real-world problems in education, mental health, and cross-cultural studies. Experienced in SQL, Power BI, Python, and data visualization.

**Skills:** SQL | Power BI | Python | Data Analysis | Statistical Analysis |

Connect with me:
- 📝 [Medium Blog](https://medium.com/@somranal2799)
- 💼 [LinkedIn](https://www.linkedin.com/in/mranal-sonare/)
- 🐙 [GitHub](https://github.com/somranal2799)

---

## 📄 License

This project is open source.

---


---

## 🔮 Future Enhancements

- [ ] Visualize trends using Power BI or Python (matplotlib/seaborn)
- [ ] Perform statistical significance testing (t-tests, ANOVA)
- [ ] Compare international vs. domestic students
- [ ] Analyze by region, gender, and academic level
- [ ] Create predictive models for mental health risk
- [ ] Develop interactive dashboard for universities

---

 **If you found this project helpful, please consider giving it a star!**

---

##  Contact

For questions, collaborations, or feedback, feel free to reach out!

**Project Status:** Completed |  Open to Contributions
