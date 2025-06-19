# EDA-Analysis-of-Depressive-Cases

# Questions to be answered:

- What is the profile of individuals (adults over 18 years old) with depressive symptoms in the USA in the period 2005-2006?

- Are healthy eating and physical activity habits associated with lower rates of depression in this population?


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

```



# UNIVARIATE EXPLORATORY ANALYSIS 
Since one of the questions we want to answer is: ''What is the profile of individuals (adults over 18 years old) with depressive symptoms in the USA in the period 2005-2006?''

We will analyze the variables of this problem tree below, filtering the results by ''moderate and severe symptoms''

👇problem tree to identify the main variables to be worked on in the univariate analysis and during this process understand if and how they answer our question

![image](https://github.com/user-attachments/assets/059227d0-df8d-4ae4-93eb-ac7d3f2e8657)



---
# Categorizing and Grouping Depressive Symptom Levels

To assign a final classification to each individual's results from the DPQ questionnaires (DPQ10, DPQ20, ... DPQ9) and group them by depressive symptom level, I employed the following methodology:

---
# 1.1 Initial Calculation Exploration and Data Challenges

Initially, I experimented with various calculation methods (mean, median, mode, and weighted average) to determine the most representative final score. A significant challenge encountered was the **high frequency of zero responses** within the questionnaires.

For instance, consider a participant with the following scores:
* Two scores of '3' (indicating severe depressive symptoms)
* Four scores of '2' (indicating moderate depressive symptoms)
* Four scores of '0' (indicating no symptoms)

While there were responses indicating high depressive symptom levels (2 for moderate and 3 for severe), the four zeros, despite not representing the majority of scores, skewed the simple mean towards 0 or 1. This would incorrectly suggest low depressive symptom levels, which was not an accurate representation of the individual's overall state.

---
# 1.2 Solution: Implementing a Weighted Average

To address the data distortion caused by the numerous zero responses and to emphasize the severity of higher symptom levels, I implemented a **weighted average** calculation. The weighting was applied as follows:

* **Score 0 = Weight 0**
* **Score 1 = Weight 1**
* **Score 2 = Weight 2**
* **Score 3 = Weight 3**

This approach provides greater consideration and impact to higher depressive symptom levels, aligning the final score more accurately with the presence of more severe symptoms.

---
# 1.3 Calculation and Classification Logic (Excel Implementation)

The weighted average was calculated using the following formula in Excel:

```excel
=SUM(ARRAYFORMULA(Notas_da_pessoa * VLOOKUP(Notas_da_pessoa, Peso_de_Cada_Nota, 2, FALSE))) / SUM(ARRAYFORMULA(VLOOKUP(Notas_da_pessoa, Peso_de_Cada_Nota, 2, FALSE)))
```

The resulting weighted average was then rounded to the nearest whole number:

```excel
=ROUND(Média_ponderada)
```

Finally, the rounded value was used to classify the individual's depressive symptom level:

```excel
=IF(Valor_arredondado <=1, "mild symptoms", IF(Valor_arredondado = 2, "moderate_symptoms", "severe_symptoms"))
```

This classification allows for the final visualization of the data through the distribution shown below:

![depressive_symtons_distribution](https://github.com/user-attachments/assets/4d5918c6-06a3-49c9-91e1-6eeedf4855e3)



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

# 3.1 0–1,24 hours per week: Poor
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

# 3.5 More than 20 hours per week: Overload
Based on studies: Research suggests that this level of exercise is typical of competitive athletes or those involved in high-performance physical activities, such as training for competitions. The Frontiers in Psychology study notes that excessive exercise can have negative impacts on mental health, such as burnout and increased risk of injury.

Rationale: At this range, intense exercise may begin to have mixed effects on mental health, including risk of physical and psychological stress.

Source: Frontiers in Psychology, “Overtraining and Mental Health in Athletes: A Systematic Review,” 2019.

# 4. I counted the quantity of each category:

using the =COUNT function:

=COUNT(I:I)

doing this with each column of the categories

getting the following result:
![amout of exercise](https://github.com/user-attachments/assets/badaeb55-c37b-416b-8aad-92e0618c65a4)


# 5. I created a graph to analyze the participation of each category

and in the future analyze the relationship with other variables: 
![Chart Exercise Distribution](https://github.com/user-attachments/assets/2bab2938-4259-44fd-8b83-b16dfdc144b6)


# Filtering variables for people with depression, to understand their profile:

## Gender

### 📊 Key Findings
- **53% of the women** in the dataset are classified as experiencing depression.
- While women represent the majority of the sample, the **percentage of men with depression is very close** to that of women.
- This narrow difference suggests that **gender is not a major influencing factor** in identifying individuals with depression within this dataset.
![chart](https://github.com/user-attachments/assets/e5d1e8d2-162a-45e4-8d02-ea4a7b0df718)
