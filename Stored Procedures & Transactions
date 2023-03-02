-- #USING JOINS 

--Write and execute a SQL query to list the school names, community names and average attendance for communities with a hardship index of 98.

select cs.name_of_school, cs.community_area_name, cs.average_student_attendance
from chicago_public_schools cs left join census_data cd 
on cs.community_area_number = cd.community_area_number
where cd.hardship_index = 98;

--Write and execute a SQL query to list all crimes that took place at a school. Include case number, crime type and community name.

select cc.case_number, cc.primary_type,cd.community_area_name
from chicago_crime_data cc left join census_data cd 
on cc.community_area_number = cd.community_area_number
where lower(cc.location_description) like '%school%';

--Create a View 

create view chicago_schools 
as select name_of_school as school_name, safety_icon as safety_rating, family_involvement_icon as family_rating,
		  environment_icon as environment_rating, instruction_icon as instruction_rating, leaders_icon as leaders_rating,
		  teachers_icon as teachers_rating 
from chicago_public_schools;

--Creating a Stored Procedure 
--#SET TERMINATOR @
CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
IN SCHOOL_ID INTEGER, IN LEADERS_SCORE INTEGER)
LANGUAGE SQL 
READS SQL DATA                      
DYNAMIC RESULT SETS 1              
BEGIN 
 DECLARE C1 CURSOR               -- CURSOR C1 will handle the result-set by retrieving records row by row from the table
    WITH RETURN FOR                 -- This routine will return retrieved records as a result-set to the caller query
    
    SELECT SCHOOL_ID, LEADERS_SCORE FROM CHICAGO_PUBLIC_SCHOOLS;          -- Query to retrieve all the records from the table
    
    OPEN C1;                        -- Keeping the CURSOR C1 open so that result-set can be returned to the caller query

END 
@

 --#SET TERMINATOR @ 
CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
	IN IN_SCHOOL_ID INTEGER, IN IN_LEADERS_SCORE INTEGER)
LANGUAGE SQL
MODIFIES SQL DATA 

BEGIN
	 
		UPDATE CHICAGO_PUBLIC_SCHOOLS
		SET LEADERS_SCORE = IN_LEADERS_SCORE
		where IN_SCHOOL_ID = SCHOOL_ID;
	

END
@

--turn the stored procedure into a transaction

--#SET TERMINATOR @ 
CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
	IN IN_SCHOOL_ID INTEGER, IN IN_LEADERS_SCORE INTEGER)
	LANGUAGE SQL
	MODIFIES SQL DATA 

BEGIN
	UPDATE Chicago_Public_Schools
	SET Leaders_Score=in_leaders_score
	WHERE School_ID=in_school_id;
	 
	if IN_LEADERS_SCORE > 0 AND IN_LEADERS_SCORE < 20 THEN
	UPDATE CHICAGO_PUBLIC_SCHOOLS
	SET LEADERS_ICON = 'Very weak'
	where SCHOOL_ID = IN_SCHOOL_ID;
	
	ELSEIF in_Leaders_Score < 40 THEN
	UPDATE CHICAGO_PUBLIC_SCHOOLS
	SET LEADERS_ICON = 'Weak'
	where SCHOOL_ID = IN_SCHOOL_ID;
	
	ELSEIF in_Leaders_Score < 60 THEN
	UPDATE CHICAGO_PUBLIC_SCHOOLS
	SET LEADERS_ICON = 'Average'
	where SCHOOL_ID = IN_SCHOOL_ID;
	
	ELSEIF in_Leaders_Score < 80 THEN
	UPDATE CHICAGO_PUBLIC_SCHOOLS
	SET LEADERS_ICON = 'Strong'
	where SCHOOL_ID = IN_SCHOOL_ID;
	
	ELSEIF in_Leaders_Score < 100 THEN
	UPDATE CHICAGO_PUBLIC_SCHOOLS
	SET LEADERS_ICON = 'Very strong'
	where SCHOOL_ID = IN_SCHOOL_ID;
	
 	ELSE ROLLBACK WORK;
	END IF;
    COMMIT WORK;
END
@
