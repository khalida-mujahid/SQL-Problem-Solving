# write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, 
# total number of managers, and total number of employees. 
# Order your output by ascending company_code.

  SELECT * FROM Company
  SELECT * FROM Lead_Manager
  SELECT * FROM Senior_Manager
  SELECT * FROM Manager
  SELECT * FROM EmployeeN

  Create table Company (company_code varchar(10), founder varchar(10));
  Create table Lead_Manager (lead_manager_code varchar(10), company_code varchar(10));
  Create table Senior_Manager (senior_manager_code varchar(10), lead_manager_code varchar(10), company_code varchar(10));
  Create table Manager (manager_code varchar(10), senior_manager_code varchar(10), lead_manager_code varchar(10), company_code varchar(10));
  Create table EmployeeN (employee_code varchar(10), manager_code varchar(10), senior_manager_code varchar(10), lead_manager_code varchar(10), company_code varchar(10))
					   
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

  Select Company.company_code, founder, count(DISTINCT Lead_Manager.lead_manager_code) as "'total number of lead managers'",
  COUNT(DISTINCT Senior_Manager.senior_manager_code) AS "'total number of senior managers'",
  COUNT(DISTINCT Manager.manager_code) AS "'total number of managers'",
  COUNT(DISTINCT employee_code) AS "'total number of employees'"
  from Company join Lead_Manager ON Company.company_code =  Lead_Manager.company_code
  JOIN Senior_Manager ON Senior_Manager.company_code = Company.company_code 
  join Manager ON Manager.company_code = Company.company_code 
  JOIN EmployeeN ON EmployeeN.company_code = Company.company_code 
  group by Company.company_code, founder











