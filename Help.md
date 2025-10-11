SQL Problems - Chicago Schools Dataset
# Problem 1
How many Elementary Schools are in the dataseSELECT COUNT(*) AS elementary_school_count
FROM schools
WHERE "Elementary, Middle, or High School" = 'ES';

# Problem 2
What is the highest Safety ScorSELECT MAX(Safety_Score) AS highest_safety_score
FROM schools;

# Problem 3
Which schools have highest Safety Score?
sqlSELECT Name_of_School, Safety_Score
FROM schools
WHERE Safety_Score = (SELECT MAX(Safety_Score) FROM schools);

# Problem 4
What are the top 10 schools with the highest "Average Student AttendaSELECT Name_of_School, Average_Student_Attendance
FROM schools
ORDER BY Average_Student_Attendance DESC
LIMIT 10;

# Problem 5
Retrieve the list of 5 Schools with the lowest Average Student Attendance sorted in ascending order based on attendance
SELECT Name_of_School, Average_Student_Attendance
FROM schools
ORDER BY Average_Student_Attendance ASC
LIMIT 5;

# Problem 6
Now remove the '%' sign from the above result set for Average Student Attendance column
SELECT Name_of_School, 
       REPLACE(Average_Student_Attendance, '%', '') AS Average_Student_Attendance
FROM schools
ORDER BY Average_Student_Attendance ASC
LIMIT 5;
Alternative using CAST for numeric operations:
SELECT Name_of_School, 
       CAST(REPLACE(Average_Student_Attendance, '%', '') AS DECIMAL(5,2)) AS Average_Student_Attendance
FROM schools
ORDER BY CAST(REPLACE(Average_Student_Attendance, '%', '') AS DECIMAL(5,2)) ASC
LIMIT 5;

# Problem 7
Which Schools have Average Student Attendance lower than 70%?
SELECT Name_of_School, Average_Student_Attendance
FROM schools
WHERE CAST(REPLACE(Average_Student_Attendance, '%', '') AS DECIMAL(5,2)) < 70
ORDER BY Average_Student_Attendance ASC;

# Problem 8
Get the total College Enrollment for each Community Area
SELECT Community_Area_Name, 
       SUM(College_Enrollment) AS total_college_enrollment
FROM schools
GROUP BY Community_Area_Name;

# Problem 9
Get the 5 Community Areas with the least total College Enrollment sorted in ascending order
SELECT Community_Area_Name, 
       SUM(College_Enrollment) AS total_college_enrollment
FROM schools
GROUP BY Community_Area_Name
ORDER BY total_college_enrollment ASC
LIMIT 5;

# Problem 10
List 5 schools with lowest safety score
SELECT Name_of_School, Safety_Score
FROM schools
ORDER BY Safety_Score ASC
LIMIT 5;

# Problem 11
Get the hardship index for the community area which has College Enrollment of 4368
SELECT s.Community_Area_Name, 
       c.hardship_index
FROM schools s
JOIN census_data c ON s.Community_Area_Number = c.community_area_number
WHERE s.College_Enrollment = 4368;
Alternative without JOIN (using subquery):
SELECT hardship_index
FROM census_data
WHERE community_area_number = (
    SELECT Community_Area_Number 
    FROM schools 
    WHERE College_Enrollment = 4368
);

# Problem 12
Get the hardship index for the community area which has the school with the highest enrollment
SELECT s.Community_Area_Name, 
       c.hardship_index,
       s.College_Enrollment
FROM schools s
JOIN census_data c ON s.Community_Area_Number = c.community_area_number
WHERE s.College_Enrollment = (SELECT MAX(College_Enrollment) FROM schools);
Alternative approach:
SELECT hardship_index
FROM census_data
WHERE community_area_number = (
    SELECT Community_Area_Number
    FROM schools
    ORDER BY College_Enrollment DESC
    LIMIT 1
);