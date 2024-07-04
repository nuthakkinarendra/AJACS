### RDS creation:

* Deploy RDS/ Azure SQL
* Map DNS record in Route53/private DNS(private endpoint)
* Install mysql workbench
* Connect to the RDS via Mysql Workbench

### Steps: 

1. Create database for load data to rds.
     
    * Here we have two ways

       * Connect through EC2 Inastance
       * Connect through MYSQL Workbench (install mysql workbench in local system)
       
2. Map DNS record in Route53/private DNS(private endpoint)

    ![preview](images/task68.jpg)
    ![preview](images/task69.jpg)
    ![preview](images/task70.jpg)
    ![preview](images/task71.jpg)
    ![alt text](image.png)
    ![preview](images/task72.jpg)
    ![preview](images/task73.jpg)

3. Install MYSQL Workbench, Currently we are using **RDS Endpoint** connect to **MYSQL Workbench**

    ![preview](images/task43.jpg)
    ![preview](images/task44.jpg)   
    ![preview](images/task45.jpg)
    ![preview](images/task46.jpg)
    ![preview](images/task47.jpg)
    ![preview](images/task48.jpg)
    ![preview](images/task49.jpg)
    ![preview](images/task50.jpg)
    ![preview](images/task57.jpg)

    * Till now created database in aws, now connect to mysql workbench using rds endpoint, username, password which is give to created above database in aws. 

    ![preview](images/task56.jpg)
    ![preview](images/task55.jpg)
    ![preview](images/task52.jpg)
    ![preview](images/task53.jpg)


    