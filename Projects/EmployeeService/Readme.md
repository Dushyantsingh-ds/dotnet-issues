This is an Web api projects, created with the use of Entity framework and Class libaray.  <br/>

<details>
  <summary>Create Project </summary>

The Sql Qurey ex:
Execute the following SQL Script using SQL Server Management studio. This script creates <br/>
1. EmployeeDB database <br/>
2. Creates the Employees table and populate it with sample data <br/>
```
Create Database EmployeeDB
Go

Use EmployeeDB
Go

Create table Employees
(
     ID int primary key identity,
     FirstName nvarchar(50),
     LastName nvarchar(50),
     Gender nvarchar(50),
     Salary int
)
Go

Insert into Employees values ('Mark', 'Hastings', 'Male', 60000)
Insert into Employees values ('Steve', 'Pound', 'Male', 45000)
Insert into Employees values ('Ben', 'Hoskins', 'Male', 70000)
Insert into Employees values ('Philip', 'Hastings', 'Male', 45000)
Insert into Employees values ('Mary', 'Lambeth', 'Female', 30000)
Insert into Employees values ('Valarie', 'Vikings', 'Female', 35000)
Insert into Employees values ('John', 'Stanmore', 'Male', 80000)
Go

```

Creating a new ASP.NET Web API Project

1. Open Visual Studio and select File - New - Project
 

2. In the "New Project" window

    Select "Visual C#" under "Installed - Templates"

From the middle pane select, ASP.NET Web Application

Name the project "EmployeeService" and click "OK"
    
![Alt Text](https://1.bp.blogspot.com/-_o6MTksitR8/V8x9DX8G93I/AAAAAAAAjW8/NmDqxlbi7mE5C9eWWNyVtVdHrBLAdqVmQCLcB/s1600/asp.net%2Bweb%2Bapi%2Bentity%2Bframework%2Bexample.png)
    

3. On the next window, select "Web API" and click "OK"



At this point you should have the Web API project created.



Adding ADO.NET Entity Data Model to retrieve data

1. We will have the Entity Model in a separate project.



2. Right click on EmployeeService solution in the Solution Explorer and select Add - New Project



3. In the Add New Project window

Select Visual C# from the left pane

Class Library Project from the Middle pane

Name the project EmployeeDataAccess and click OK    

![alt txt](https://4.bp.blogspot.com/-yDvz55GfOCY/V8x9fkh6YMI/AAAAAAAAjXA/a04SmN6AXZcKq5XGNxWiwquLlAE1OTBBwCLcB/s1600/connect%2Bweb%2Bapi%2Bto%2Bsql%2Bserver%2Bdatabase.png)

4. Right click on EmployeeDataAccess project and select Add - New Item



5. In the "Add New Item" window

    Select "Data" from the left pane

    Select ADO.NET Entity Data Model from the middle pane

    In the Name text box, type EmployeeDataModel and click Add

![alt txt](https://2.bp.blogspot.com/-ORMTPka-4UA/V8x90jSJUQI/AAAAAAAAjXE/7WPVfj5SduIzON-KaQ50QjDOZPrH_ILGgCLcB/s1600/using%2Bentity%2Bframework%2Bwith%2Bweb%2Bapi.png)


6. On the Entity Data Model Wizard, select "EF Designer from database" option and click next

![alt txt](https://1.bp.blogspot.com/-JgZuShvgc0E/V8x-LSivl2I/AAAAAAAAjXI/bjO8gZdZVI4VQ_x8O1RuONKjU3tK2kkVQCLcB/s1600/asp.net%2Bweb%2Bapi%2Bentity%2Bframework%2Bdatabase%2Bfirst.png)

7. On the next screen, click "New Connection" button



8. On "Connection Properties" window, set

Server Name = (local)

Authentication = Windows Authentication

Select or enter a database name = EmployeeDB

Click OK and then click Next



9. On the nex screen, select Entity Framework 6.x

![alt txt](https://4.bp.blogspot.com/-18quEDxgk2U/V8x-gK2T4kI/AAAAAAAAjXM/GT4aVRxu-C8uwP9JEpDwHIQkZT-4JPV5QCLcB/s1600/adding%2Bentity%2Bframework%2Bto%2Bweb%2Bapi.png)

10. On the nex screen, select "Employees" table and click Finish.

![alt txt](https://2.bp.blogspot.com/-MKliQ9Z4b-E/V8x-2Bkhr9I/AAAAAAAAjXQ/l_3cP7nvdxYA0-CJtBMgKkJV9_642E7UACLcB/s1600/web%2Bapi%2Band%2Bentity%2Bframework%2B6.png)

Using the Entity Data Model in EmployeeService project

1. Right click on the references folder in the EmployeeService project and select "Add Reference"



2. On the "Reference Manager" screen select "EmployeeDataAccess" project and click OK. 

![alt txt](https://1.bp.blogspot.com/-blhAqQjdgho/V8x_HGI7ziI/AAAAAAAAjXU/KmZaTgf6evIVxrNug_Ma8TcBWB-vm6FcgCLcB/s1600/adding%2Bproject%2Breference%2Bvisual%2Bstudio.png)

Using the Entity Data Model in EmployeeService project

1. Right click on the references folder in the EmployeeService project and select "Add Reference"



2. On the "Reference Manager" screen select "EmployeeDataAccess" project and click OK. 

adding project reference visual studio



Adding Web API Controller

1. Right click on the Controllers folder in EmployeeService project and select Add - Controller



2. Select "Web API 2 Controller - Empty" and click "Add"



3. On the next screen set the Controller Name = EmployeesController and click Add



4. Copy and paste the following code in EmployeesController.cs

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Web.Http;
using EmployeeDataAccess;

namespace EmployeeService.Controllers
{
    public class EmployeesController : ApiController
    {
        public IEnumerable<Employee> Get()
        {
            using(EmployeeDBEntities entities = new EmployeeDBEntities())
            {
                return entities.Employees.ToList();
            }
        }

        public Employee Get(int id)
        {
            using (EmployeeDBEntities entities = new EmployeeDBEntities())
            {
                return entities.Employees.FirstOrDefault(e => e.ID == id);
            }
        }
    }
}
```
5. At this point build the solution and navigate to /api/employees. You will get the following error.
No connection string named 'EmployeeDBEntities' could be found in the application config file.

6. This is because "Entity Framework" is looking for EmployeeDBEntities connection string in the web.config file of EmployeeService project. EmployeeDBEntities connection string is actually in App.config file of EmployeeDataAccess class library project. Include a copy of this connection string in web.config file.

At this point when you navigate to /api/employees you should see all employees and when you navigate to /api/employees/1 you should see all the details of the employee whose Id=1   
  
  
  
  </details>
  
  
<details>
  <summary>Error Project </summary>
  
 Get: //Working <br/> 
  Get/ <br/>
  Get/ID <br/> <br/>
  
Post: //Working <br/>
  Get/ <br/><br/>
  
 Put: //Not wokring <br/>
  Get/ID + [BodyForm] Employee <br/><br/>
  
 Delete: //Not working <br/><br/>
  Get/ID<br/><br/>
  
  error: HTTP/1.1 405 Method Not Allowed

![alt txt](https://raw.githubusercontent.com/Dushyantsingh-ds/dotnet-issues/main/Projects/EmployeeService/ss1.png)
  </details>
