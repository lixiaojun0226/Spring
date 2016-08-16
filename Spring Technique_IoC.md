
## IoC: 反向控制, 是一种思想，类似于Java的设计模式中的一大原则（依赖倒置原则）------实现依赖于抽象, 抽象不应依赖于实现. 与Java的抽象工厂模式设计理念相同.
### For Example:
   * 从SQL Server中取数据
   * 从DB2中取数据
   * 从My SQL中取数据
  
     设计数据库时，考虑数据会存储在不同的数据库中，在获取数据即数据访问时不应只针对特定的数据库的实现特定的数据访问方式，这种方式不利于扩展、移植. 若更换数据库时要变动的不仅是访问数据库的代码，业务逻辑的代码因此也受影响，需要重改，代码量较多，效率低下。 
   
    #### low style:
    从SQL Server中取数据

    *** 数据访问层 ***
```java
    public Class SQLServerDataBase{
          /****从SQL Server中取数据******/
           public list getDataFromSQLServer(){ }
    }
```
    *** 业务逻辑层 ***
    
```java
    public Class Business{
          SQLServerDataBase db = new SQLServerDataBase();
          public void gevatData(){
             List list = db.getDataFromSQLServer()
          }
    }
```
   从DB2中取数据
   
    *** 数据访问层 ***
```java
    public Class DB2DataBase{
          /****从DB2中取数据******/
           public list getDataFromDB2(){ }
    }
 ```
    *** 业务逻辑层 ***
    
```java
    public Class Business{
          DB2DataBase db = new DB2DataBase();
          public void getData(){
             List list = db.getDataFromDB2()
          }
    }
```
    
    上述方法中业务逻辑层的Business类依赖于数据访问层的类***DataBase,类依赖于类，实现依赖于实现，不利于扩展, 不灵活.
    
    高效的方式应该将业务逻辑与数据访问分开，在本例中，即使数据库更换，也不会影响业务逻辑层.
    
    #### high style：
