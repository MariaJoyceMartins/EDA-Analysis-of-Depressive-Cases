# EDA-Analysis-of-Depressive-Cases

#Data processing

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

-- Data Analytics --






