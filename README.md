# 🌏 Does Studying Abroad Affect Mental Health?
### A PostgreSQL analysis of international students in Japan

## 📌 The Problem

Studying in a new country can be exciting and isolating at the same time. In 2018, a Japanese international university surveyed its students, and the study published the following year (approved by several ethical and regulatory boards) reported that:

- International students have a **higher risk of mental health difficulties** than the general population.
- **Social connectedness** and **acculturative stress** (stress from joining a new culture) are **predictive of depression**.

**This project asks:** does the survey data support a similar conclusion for international students, and is **length of stay** a contributing factor?

## 🗂️ The Data

The `students` table holds 286 survey responses. Key columns:

| Column | Description |
|---|---|
| `inter_dom` | International or domestic student |
| `stay` | Length of stay in years |
| `todep` | Depression score (PHQ-9) |
| `tosc` | Social connectedness score (SCS) |
| `toas` | Acculturative stress score (ASISS) |

Other columns cover language proficiency, academic level, and age.

## 🛠️ Tools & Technology

- **PostgreSQL / SQL**: filtering, `GROUP BY`, and aggregates (`COUNT`, `AVG`, `ROUND`)
- **Git & GitHub**: version control and documentation

## 💡 The Solution

One query filters to international students and groups them by length of stay, returning the student count and the average depression, social connectedness, and acculturative stress scores for each group.

```sql
SELECT
    stay,
    COUNT(*)              AS count_int,
    ROUND(AVG(todep), 2)  AS average_phq,
    ROUND(AVG(tosc), 2)   AS average_scs,
    ROUND(AVG(toas), 2)   AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC
LIMIT 9;
```

## 📊 Key Findings

201 international students, longest stays first:

| Stay (years) | Students | Depression (PHQ-9) | Social connectedness (SCS) | Acculturative stress (ASISS) |
|---|---|---|---|---|
| 10 | 1 | 13 | 32 | 50 |
| 8 | 1 | 10 | 44 | 65 |
| 7 | 1 | 4 | 48 | 45 |
| 6 | 3 | 6 | 38 | 58.67 |
| 5 | 1 | 0 | 34 | 91 |
| 4 | 14 | 8.57 | 33.93 | 87.71 |
| 3 | 46 | 9.09 | 37.13 | 78 |
| 2 | 39 | 8.28 | 37.08 | 77.67 |
| 1 | 95 | 7.48 | 38.11 | 72.8 |

- **Reliable range:** stays of 5+ years have only 1 to 3 students each, so focus on the 1 to 4 year groups (14 to 95 students).
- **Depression:** lowest in year 1 (7.48), higher in years 2 to 4 (8.28, 9.09, 8.57). The rise is modest and stays in the mild PHQ-9 range.
- **Acculturative stress:** the clearest trend, rising from 72.8 in year 1 to 87.71 in year 4.
- **Social connectedness:** stays between about 34 and 38, lowest in year 4 (33.93), the same group with the highest stress.

**Conclusion:** the pattern points the same way as the original study. Students who have stayed longer than a year show somewhat higher depression and acculturative stress and lower social connectedness, so length of stay looks like a contributing factor. The differences are small, and testing whether connectedness and stress *predict* depression would need a direct check, such as correlations between `todep`, `tosc`, and `toas`.

## 🎯 The Value

- **Evidence-based:** tests published research against the underlying data.
- **Early-warning signals:** shows when in a student's stay support may matter most.
- **Actionable:** can guide orientation programs, peer communities, and counselling outreach.
- **Reproducible:** plain SQL that anyone can rerun and verify.

## ⚠️ Limitations

- Single university, single year, so results may not generalise.
- Each stay length compares different students rather than following the same students over time.
- The results show association, not causation.

## 🚀 How to Run

1. Load the dataset into PostgreSQL as a table named `students`.
2. Run the query above, or the files in `queries/`, in psql, pgAdmin, or DBeaver.

```
├── README.md
├── queries/length_of_stay_analysis.sql
└── data/students.csv
```

## 👩🏽‍💻 Author

**Susan**, Nairobi, Kenya. Building skills in Data Science and AI through hands-on projects.

*Data source: international university student survey (2018), published 2019.*
