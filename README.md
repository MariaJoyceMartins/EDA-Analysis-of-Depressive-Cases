# EDA-Analysis-of-Depressive-Cases

# Data Processing 


```
-- Checking for inconsistencies in the data --

SELECT RIAGENDR FROM demo_phq
WHERE RIAGENDR NOT IN ('1', '2')

-- variavel ok -- 

SELECT DISTINCT RIDAGEYR FROM demo_phq;

-- variavel ok --

-- Joining the demo_phq and pag_hei tables --

SELECT *
FROM demo_phq AS d LEFT JOIN pag_hei AS p
ON d.SEQN = p.SEQN;

-- counting the number of lines
SELECT COUNT(SEQN) 
FROM d_w_p;

-- change the security mode
SET SQL_SAFE_UPDATES = 0;

-- Assigning zero to ''don't know'' and ''refused to answer'' responses
UPDATE d_w_p
SET DPQ010= 'NA'
WHERE DPQ010 = 9 AND 7;

UPDATE d_w_p
SET DPQ020= 'NA'
WHERE DPQ010 = 9 AND 7;

UPDATE d_w_p
SET DPQ030= 'NA'
WHERE DPQ020 = 9 AND 7;

UPDATE d_w_p
SET DPQ040= 'NA'
WHERE DPQ040 = 9 AND 7;

UPDATE d_w_p
SET DPQ050= 'NA'
WHERE DPQ050 = 9 AND 7;

UPDATE d_w_p
SET DPQ060= 'NA'
WHERE DPQ060 = 9 AND 7;


UPDATE d_w_p
SET DPQ070 = 'NA'
WHERE DPQ070 = 9 AND 7;

UPDATE d_w_p
SET DPQ080 = 'NA'
WHERE DPQ080 = 9 AND 7;

UPDATE d_w_p
SET DPQ090 = 'NA'
WHERE DPQ090 = 9 AND 7;

UPDATE d_w_p
SET DMDEDUC = 'NA'
WHERE DMDEDUC = 9 AND 7;

SELECT HEI2015C1_TOTALVEG
FROM d_w_p

-- calculating quantity of each category --
CREATE TABLE categorias_somadas AS
SELECT
    SUM(CASE WHEN DPQ010 = 0 THEN 1 ELSE 0 END +
        CASE WHEN DPQ020 = 0 THEN 1 ELSE 0 END +
        CASE WHEN DPQ030 = 0 THEN 1 ELSE 0 END +
        CASE WHEN DPQ040 = 0 THEN 1 ELSE 0 END +
        CASE WHEN DPQ050 = 0 THEN 1 ELSE 0 END +
        CASE WHEN DPQ060 = 0 THEN 1 ELSE 0 END +
        CASE WHEN DPQ070 = 0 THEN 1 ELSE 0 END +
        CASE WHEN DPQ080 = 0 THEN 1 ELSE 0 END +
        CASE WHEN DPQ090 = 0 THEN 1 ELSE 0 END) AS categoria_0, 
        
    SUM(CASE WHEN DPQ010 = 1 THEN 1 ELSE 0 END +
        CASE WHEN DPQ020 = 1 THEN 1 ELSE 0 END +
        CASE WHEN DPQ030 = 1 THEN 1 ELSE 0 END +
        CASE WHEN DPQ040 = 1 THEN 1 ELSE 0 END +
        CASE WHEN DPQ050 = 1 THEN 1 ELSE 0 END +
        CASE WHEN DPQ060 = 1 THEN 1 ELSE 0 END +
        CASE WHEN DPQ070 = 1 THEN 1 ELSE 0 END +
        CASE WHEN DPQ080 = 1 THEN 1 ELSE 0 END +
        CASE WHEN DPQ090 = 1 THEN 1 ELSE 0 END) AS categoria_1,

    SUM(CASE WHEN DPQ010 = 2 THEN 1 ELSE 0 END +
        CASE WHEN DPQ020 = 2 THEN 1 ELSE 0 END +
        CASE WHEN DPQ030 = 2 THEN 1 ELSE 0 END +
        CASE WHEN DPQ040 = 2 THEN 1 ELSE 0 END +
        CASE WHEN DPQ050 = 2 THEN 1 ELSE 0 END +
        CASE WHEN DPQ060 = 2 THEN 1 ELSE 0 END +
        CASE WHEN DPQ070 = 2 THEN 1 ELSE 0 END +
        CASE WHEN DPQ080 = 2 THEN 1 ELSE 0 END +
        CASE WHEN DPQ090 = 2 THEN 1 ELSE 0 END) AS categoria_2,

    SUM(CASE WHEN DPQ010 = 3 THEN 1 ELSE 0 END +
        CASE WHEN DPQ020 = 3 THEN 1 ELSE 0 END +
        CASE WHEN DPQ030 = 3 THEN 1 ELSE 0 END +
        CASE WHEN DPQ040 = 3 THEN 1 ELSE 0 END +
        CASE WHEN DPQ050 = 3 THEN 1 ELSE 0 END +
        CASE WHEN DPQ060 = 3 THEN 1 ELSE 0 END +
        CASE WHEN DPQ070 = 3 THEN 1 ELSE 0 END +
        CASE WHEN DPQ080 = 3 THEN 1 ELSE 0 END +
        CASE WHEN DPQ090 = 3 THEN 1 ELSE 0 END) AS categoria_3

FROM 
    d_w_p;
    

-- Create a column mild symptons --
ALTER TABLE categorias_somadas ADD COLUMN mild_symptoms INT;

-- Combining category 0 and 1 (mild symptoms) --
UPDATE categorias_somadas
SET mild_symptoms = categoria_0 + categoria_1;

-- Alter the name of colums of categorias somadas --

ALTER TABLE categorias_somadas CHANGE categoria_2 moderate_symptoms INT;
ALTER TABLE categorias_somadas CHANGE categoria_3 severe_symptoms INT;

-- checking the results --

SELECT 	*
FROM categorias_somadas


-- checking the values null --


SELECT 
    SUM(CASE WHEN RIAGENDR IS NULL THEN 1 ELSE 0 END) AS RIAGENDR_null_count,
    SUM(CASE WHEN RIDAGEYR IS NULL THEN 1 ELSE 0 END) AS RIDAGEYR_null_count,
    SUM(CASE WHEN RIDRETH1 IS NULL THEN 1 ELSE 0 END) AS RIDRETH1_null_count,
    SUM(CASE WHEN DMDEDUC IS NULL THEN 1 ELSE 0 END) AS DMDEDUC_null_count,
    SUM(CASE WHEN INDFMINC IS NULL THEN 1 ELSE 0 END) AS INDFMINC_null_count,
    SUM(CASE WHEN HEI2015_TOTAL_SCORE IS NULL THEN 1 ELSE 0 END) AS HEI2015_TOTAL_SCORE_null_count,
    SUM(CASE WHEN PAG_MINW IS NULL THEN 1 ELSE 0 END) AS PAG_MINW_null_count,
    SUM(CASE WHEN ADHERENCE IS NULL THEN 1 ELSE 0 END) AS ADHERENCE_null_count,
    
    (SUM(CASE WHEN RIAGENDR IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS RIAGENDR_null_percentage,
    (SUM(CASE WHEN RIDAGEYR IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS RIDAGEYR_null_percentage,
    (SUM(CASE WHEN RIDRETH1 IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS RIDRETH1_null_percentage,
    (SUM(CASE WHEN DMDEDUC IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS DMDEDUC_null_percentage,
    (SUM(CASE WHEN INDFMINC IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS INDFMINC_null_percentage,
    (SUM(CASE WHEN HEI2015_TOTAL_SCORE IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS HEI2015_TOTAL_SCORE_percentage,
    (SUM(CASE WHEN PAG_MINW IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS PAG_MINW_null_percentage,
    (SUM(CASE WHEN ADHERENCE IS NULL THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS ADHERENCE_null_percentage

FROM 
    d_w_p;


SELECT mild_symptoms, severe_symptoms, moderate_symptoms
FROM categorias_somadas
WHERE mild_symptoms IS NULL
AND severe_symptoms IS NULL
AND moderate_symptoms IS NULL;

-- there are no null columns -- 

```

# UNIVARIATE EXPLORATORY ANALYSIS 

👇problem tree to identify the main variables to be worked on in the univariate analysis and during this process understand if and how they answer our question

![image](https://github.com/user-attachments/assets/f1049bfd-32f6-46dc-9733-9ee893334550)


Quantitative variables

```
SELECT
    -- Estatísticas para RIDAGEYR
    AVG(RIDAGEYR) AS RIDAGEYR_avg,
    STDDEV(RIDAGEYR) AS RIDAGEYR_stddev,
    COUNT(RIDAGEYR) AS RIDAGEYR_count,
    MIN(RIDAGEYR) AS RIDAGEYR_min,
    MAX(RIDAGEYR) AS RIDAGEYR_max,

FROM 
    d_w_p;

SELECT
    -- Estatísticas para INDFMINC
    AVG(INDFMINC) AS INDFMINC_avg,
    STDDEV(INDFMINC) AS INDFMINC_stddev,
    COUNT(INDFMINC) AS INDFMINC_count,
    MIN(INDFMINC) AS INDFMINC_min,
    MAX(INDFMINC) AS INDFMINC_max,
    

FROM 
    d_w_p;

SELECT 
    -- Estatísticas para PAG_MINW
    AVG(PAG_MINW) AS PAG_MINW_avg,
    STDDEV(PAG_MINW) AS PAG_MINW_stddev,
    COUNT(PAG_MINW) AS PAG_MINW_count,
    MIN(PAG_MINW) AS PAG_MINW_min,
    MAX(PAG_MINW) AS PAG_MINW_max,
    
FROM 
    d_w_p;

´´'

```
 # Analysis of exercise minutes per week (pag_min)

# 1. I rounded the value 
to terms so that we only have the minutes, without the seconds and make it easier to convert from minutes to hours

for this, I used the =ROUND function from Google Sheets: 

=ROUND(Table1[PAG_MINW])

getting the following before vs after (sample of 14 of 1,000 columns)

![image](https://github.com/user-attachments/assets/3c5583e0-cc80-49c5-8ce9-0cccbfde321c)

# 2. I converted minutes to hours 
to make analysis easier

To do this, I divided by 60 in Google Sheets:

=(B2/60)

getting the following before vs after (sample of 14 of 1,000 columns):

![image](https://github.com/user-attachments/assets/54695630-411c-4aae-83a5-1aacdfde7a3c)


# 3. I categorized and grouped hourly intervals
 based on scientific research

Follow the categories and the reasoning for each:

# 3.1 0–124 hours per week: Poor
Based on WHO guidelines: WHO recommends at least 150 minutes of moderate physical activity or 75 minutes of vigorous physical activity per week for adults. Those who do not achieve these levels are at risk for health problems, including chronic diseases and poor mental health.

Rationale: This range is considered insufficient to achieve significant health benefits and is associated with higher risks of depression and other mental health conditions.

Source: WHO, "Global Recommendations on Physical Activity for Health," 2010.

# 3.2. 1,25–5 hours per week: Good
Based on WHO guidelines: This level of activity meets the minimum recommended level for health. WHO suggests that engaging in at least 75 minutes of vigorous activity or 150 minutes of moderate activity per week helps improve overall health and reduce the risk of several diseases.

Rationale: Exercising during this interval can help improve mental health, reduce symptoms of depression and anxiety, and promote overall well-being.

Source: WHO, "Global Recommendations on Physical Activity for Health," 2010.

# 3.3 5 to 10 hours per week: Very good
Based on studies: Research published in the Journal of the American Medical Association (JAMA) indicated that individuals who exercise between 7 and 10 hours per week have significant cardiovascular benefits, a lower risk of depression, and improved overall mental health.

Rationale: This range represents a more intense frequency, but still in line with recommendations for maintaining good mental and physical health. It is above the minimum recommendation, but not excessive.

Source: JAMA, "Leisure Time Physical Activity and Mortality: A Detailed Pooled Analysis," 2015.

# 3.4 10 to 20 hours per week: Exceptional
Based on studies: According to a study in Lancet Psychiatry, people who engage in moderate or intense exercise for more than 10 hours per week have a significantly reduced risk of developing mental disorders, including depression, but the additional benefit begins to diminish after 15 hours.

Rationale: Above 10 hours, there is an incremental reduction in benefits, and this range may indicate individuals with above-average exercise habits, possibly athletes or highly active individuals.

Source: Lancet Psychiatry, “Associations between physical exercise and mental health: cross-sectional survey,” 2018.

# 3.5 20 to 40 hours per week: High performance
Based on studies: Research suggests that this level of exercise is typical of competitive athletes or those involved in high-performance physical activities, such as training for competitions. The Frontiers in Psychology study notes that excessive exercise can have negative impacts on mental health, such as burnout and increased risk of injury.

Rationale: At this range, intense exercise may begin to have mixed effects on mental health, including risk of physical and psychological stress.

Source: Frontiers in Psychology, “Overtraining and Mental Health in Athletes: A Systematic Review,” 2019.

# 3.6 More than 40 hours per week: Overload
Based on studies: Training more than 40 hours per week can be characterized as overload or excessive exercise, according to the American Journal of Sports Medicine study. Although some elite athletes train in this range, the risk of mental and physical burnout is high. In non-athletes, this may be a sign of dysfunctional exercise-related behaviors.

Rationale: Excessive exercise has been associated with negative mental health effects, including chronic fatigue, burnout, and a potential increased risk of depression.

Source: American Journal of Sports Medicine, "Exercise dependence and overtraining syndrome in endurance athletes," 2020.

# 4. I counted the quantity of each category:

using the =COUNT function:

=COUNT(I:I)

doing this with each column of the categories

getting the following result:

![image](https://github.com/user-attachments/assets/b3af7e70-161c-44ae-b5b5-967e28927f01)

# 5. I created a graph to analyze the participation of each category

and in the future analyze the relationship with other variables: 
![image](https://github.com/user-attachments/assets/e64cb003-73fe-47d2-97ca-5408e47d21a1)

# Analysis of Healthy eating score

# 1. I created a histogram graph of the healthy eating score
![image](https://github.com/user-attachments/assets/2210ff86-56fd-4198-a522-cd79847a2e39)

Pattern: bell-shaped histogram, normal distribution

# Analysis of depression symptom
# 1. I created a bar chart
 to understand the distribution of depression symptom categories
![image](https://github.com/user-attachments/assets/9fb993a3-4b88-43d8-8a3c-11db63249636)
