# SQL-Problem-Solving

/*Generate the following two result sets:

Query an alphabetically ordered list of all names in OCCUPATIONS, 
immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). 
For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format:

There are a total of [occupation_count] [occupation]s.
where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. 
If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.

--Sample Output
Ashely(P)
Christeen(P)
Jane(A)
Jenny(D)
Julia(A)
Ketty(P)
Maria(A)
Meera(S)
Priya(S)
Samantha(D)
There are a total of 2 doctors.
There are a total of 2 singers.
There are a total of 3 actors.
There are a total of 3 professors.

Explanation:

The results of the first query are formatted to the problem description's specifications.
The results of the second query are ascendingly ordered first by number of names corresponding to each profession (2<=2<=3<=3), 
and then alphabetically by profession (doctor <= singer, and actor <= professor).*/

	Create table OCCUPATIONS (Name varchar(20), Occupation varchar(20));
	INSERT INTO OCCUPATIONS (Name, Occupation) values ('Samantha','Doctor'), ('Julia','Actor'),('Maria','Actor'),('Mera','Singer'),
	('Ashely','Professor'),('Ketty','Professor'),('Christeen','Professor'),('Jane','Actor'),('Jenny','Doctor'),('Priya','Singer')

# I made following solution but I made it complex by using count with case statements although it wasn't even necessary there :D lol...

	Select concat(Name,'(',Left(Occupation,1),')') as solname from OCCUPATIONS 
	UNION
	Select concat('There are a total of ', cast((count(case when occupation='Doctor' then occupation 
				  				when occupation='Singer' then occupation
				  				when occupation='Actor' then occupation
				  				when occupation='Professor' then occupation
				                              end)) as char(1)), ' ', occupation,'s')  as col 
	from OCCUPATIONS
	Group by Occupation 
	order by solname

# ABOVE can be achieved simply by using count and group by bcz group by is already seggregating on occupation count
	SELECT CONCAT(NAME, '(', LEFT(OCCUPATION, 1), ')') AS solname
	FROM OCCUPATIONS
	UNION
	SELECT CONCAT('There are a total of ', COUNT(OCCUPATION),' ', lower(OCCUPATION), 's.') 
	FROM OCCUPATIONS
	GROUP BY OCCUPATION
	ORDER BY solname

# But what if we want to use order by count of occupation ????
# Order by always works after UNION clause 
# bcz contextually it is not possible to order half of col values by sth and other values of same col by some other criteria
# but what if we really want to use order by after union clause????

	with cte1 as (
		SELECT CONCAT(NAME, '(', LEFT(OCCUPATION, 1), ')')
		FROM OCCUPATIONS
		ORDER BY NAME ASC
	)	
	, cte2 as(
		SELECT CONCAT('There are a total of ', COUNT(OCCUPATION),' ', lower(OCCUPATION), 's.') 
		FROM OCCUPATIONS
		GROUP BY OCCUPATION
		ORDER BY COUNT(OCCUPATION) ASC
	)
	select * from cte1 
	union
	select * from cte2


# here is the solution
# In SQL, the UNION operator combines the result sets of two or more SELECT queries into a single result set. 
# When you use UNION, the order of the rows in the final result set is not guaranteed to be the same as the order in the individual queries. 
# In order to maintain the order of the rows based on specific criteria in both queries, 
# you can use a subquery with UNION inside the main query to achieve your desired result.
	WITH cte1 AS (
    		SELECT CONCAT(NAME, '(', LEFT(OCCUPATION, 1), ')') AS result
    		FROM OCCUPATIONS
    		ORDER BY NAME ASC
	),
	cte2 AS (
    		SELECT CONCAT('There are a total of ', COUNT(OCCUPATION), ' ', LOWER(OCCUPATION), 's.') AS result
    		FROM OCCUPATIONS
    		GROUP BY OCCUPATION
    		ORDER BY COUNT(OCCUPATION) ASC
	)
	SELECT result
	FROM (
    		SELECT result, 1 AS sort_order
    		FROM cte1
		UNION
    		SELECT result, 2 AS sort_order
		FROM cte2
	) AS combined
	ORDER BY sort_order, result;





