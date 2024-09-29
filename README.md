# EDA-Analysis-of-Depressive-Cases

# Data Processing 

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






