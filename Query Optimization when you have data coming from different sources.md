### write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, 
### total number of managers, and total number of employees. 
### Order your output by ascending company_code.

  	SELECT * FROM Company
  	SELECT * FROM Lead_Manager
  	SELECT * FROM Senior_Manager
 	SELECT * FROM Manager
  	SELECT * FROM EmployeeN

  	Create table Company (company_code varchar(10), founder varchar(10));
 	Create table Lead_Manager (lead_manager_code varchar(10), company_code varchar(10));
  	Create table Senior_Manager (senior_manager_code varchar(10), lead_manager_code varchar(10), company_code varchar(10));
  	Create table Manager (manager_code varchar(10), senior_manager_code varchar(10), lead_manager_code varchar(10), company_code varchar(10));
  	Create table EmployeeN (employee_code varchar(10), manager_code varchar(10), senior_manager_code varchar(10), lead_manager_code varchar(10), company_code 	varchar(10))
					   
  	Insert into Company (company_code, founder) 
  	values ('C1','Monika'), ('C2','Samantha');	

  	Insert into Lead_Manager (lead_manager_code, company_code)
  	values ('LM1','C1'), ('LM2','C2');

  	Insert into Senior_Manager (senior_manager_code, lead_manager_code, company_code)
  	values ('SM1','LM1','C1'), ('SM2','LM1','C1'), ('SM3','LM2','C2');

  	INsert into Manager (manager_code, senior_manager_code, lead_manager_code, company_code)
  	values ('M1','SM1','LM1','C1'), ('M2','SM3','LM2','C2'), ('M3','SM3','LM2','C2'), ('M3','SM3','LM2','C2');

  	INSERT into EmployeeN (employee_code, manager_code, senior_manager_code, lead_manager_code, company_code)
  	values ('E1','M1','SM1','LM1','C1'),
  	('E2', 'M1','SM1','LM1','C1'), ('E3','M2','SM3','LM2','C2'),
  	('E4','M3','SM3','LM2','C2')

### so, as you can see that in following query there is alot of joins and repeating columns in join conditions as well.
### complex joins and aggregations in real-time can make query result performance really very bad
### then, how can we optimize this query into a more precise manner??
  	Select Company.company_code, founder, count(DISTINCT Lead_Manager.lead_manager_code) as "'total number of lead managers'",
  	COUNT(DISTINCT Senior_Manager.senior_manager_code) AS "'total number of senior managers'",
  	COUNT(DISTINCT Manager.manager_code) AS "'total number of managers'",
  	COUNT(DISTINCT employee_code) AS "'total number of employees'"
  	from Company join Lead_Manager ON Company.company_code =  Lead_Manager.company_code
  	JOIN Senior_Manager ON Senior_Manager.company_code = Company.company_code 
  	join Manager ON Manager.company_code = Company.company_code 
  	JOIN EmployeeN ON EmployeeN.company_code = Company.company_code 
  	group by Company.company_code, founder

To optimize and potentially make it more precise, you can consider the following approaches:

- **Indexing:** Ensure that the relevant columns used in join conditions and the grouping columns have appropriate indexes. Indexing can significantly improve query performance, especially when dealing with large datasets.

- **Use of CTEs (Common Table Expressions):** You can break down the query into smaller, more manageable parts using CTEs. This can make the query more readable and maintainable. For example, you can calculate the counts for lead managers, senior managers, managers, and employees separately in CTEs before joining them together in the final query.

'''sql

	WITH LeadManagerCounts AS (
    		SELECT company_code, COUNT(DISTINCT lead_manager_code) AS lead_manager_count
    		FROM Lead_Manager
    		GROUP BY company_code
	),

	SeniorManagerCounts AS (
    		SELECT company_code, COUNT(DISTINCT senior_manager_code) AS senior_manager_count
    		FROM Senior_Manager
    		GROUP BY company_code
	),

	ManagerCounts AS (
    		SELECT company_code, COUNT(DISTINCT manager_code) AS manager_count
    		FROM Manager
    		GROUP BY company_code
	),

	EmployeeCounts AS (
    		SELECT company_code, COUNT(DISTINCT employee_code) AS employee_count
   	 	FROM EmployeeN
    		GROUP BY company_code
	)

	SELECT C.company_code, C.founder,
    	COALESCE(LM.lead_manager_count, 0) AS 'total number of lead managers',
    	COALESCE(SM.senior_manager_count, 0) AS 'total number of senior managers',
    	COALESCE(MG.manager_count, 0) AS 'total number of managers',
    	COALESCE(EC.employee_count, 0) AS 'total number of employees'
	FROM Company AS C
	LEFT JOIN LeadManagerCounts AS LM ON C.company_code = LM.company_code
	LEFT JOIN SeniorManagerCounts AS SM ON C.company_code = SM.company_code
	LEFT JOIN ManagerCounts AS MG ON C.company_code = MG.company_code
	LEFT JOIN EmployeeCounts AS EC ON C.company_code = EC.company_code;

- **Consider Data Redundancy:** If this query is frequently used and performance is a critical concern, consider maintaining redundant summary tables that store the counts for each company. You can update these summary tables periodically and query them instead of performing complex joins and aggregations in real-time.

- **Hardware and Database Optimization:** Ensure that your database server is properly configured and optimized for query performance. This includes having enough memory, CPU resources, and tuning database settings.

- **Use of Analytical Functions:** Depending on your database system, you can also explore the use of analytical functions like PARTITION BY and SUM OVER to calculate counts more efficiently.

- **Data Partitioning and Sharding:** If your dataset is very large, consider partitioning or sharding your data to distribute the load across multiple servers, which can significantly improve performance.

- **Benchmarking and Profiling:** Continuously monitor and profile your query's performance using database profiling tools to identify bottlenecks and areas for improvement.

- **Data Cleaning:** Ensure that your data is clean and well-maintained, as inconsistencies or duplicate records can negatively impact query performance.












