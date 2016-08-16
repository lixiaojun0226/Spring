
## IoC: 反向控制, 是一种思想，类似于Java的设计模式中的一大原则（依赖倒置原则）--实现依赖于抽象, 抽象不应依赖于实现.      与Java的抽象工厂模式设计理念相同.
### For Example:
   * 从SQL Server中取数据
   * 从DB2中取数据
   * 从My SQL中取数据
   
   设计数据库时，考虑数据会存储在不同的数据库中，在获取数据即数据访问时不应只针对特定的数据库的实现特定的数据访问方式，这种方式不利于扩展、移植. 若更换数据库时要变动的不仅是访问数据库的代码，业务逻辑的代码因此也受影响，需要重改，代码量较多，效率低下。 
   
### low style:
####从SQL Server中取数据
```java
    /*** 数据访问层 ***/
    public Class SQLServerDataBase{
          /****从SQL Server中取数据******/
           public list getDataFromSQLServer(){ }
    }

    /*** 业务逻辑层 ***/
    public Class Business{
          SQLServerDataBase db = new SQLServerDataBase();
          public void gevatData(){
             List list = db.getDataFromSQLServer()
          }
    }
```

#### 从DB2中取数据

```java
    /*** 数据访问层 ***/
    public Class DB2DataBase{
          /****从DB2中取数据******/
           public list getDataFromDB2(){ }
    }
    /*** 业务逻辑层 ***/
    public Class Business{
          DB2DataBase db = new DB2DataBase();
          public void getData(){
             List list = db.getDataFromDB2()
          }
    }
```
   上述方法中业务逻辑层的Business类依赖于数据访问层的类%%%DataBase,类依赖于类，实现依赖于实现，不利于扩展, 不灵活.
    
   高效的方式应该将业务逻辑与数据访问的依赖减少. 实现Business层的可重用. 根据接口编程.
### high style:
```java
/***定义通用的数据访问接口***/
public interface DataBase{
    public void getData() { }
}

/***具体获取特定数据库的实现类***/
/***数据访问类***/
public Class SQLServerDataBase implements DataBase{
    public void getData(){
    /***获取SQLServer的数据***/
    }
}

/***业务逻辑层***/
/***依赖于接口***/
public Class Business{
    private DataBase db;
    public void setDataBase(DataBase db){
       this.db = db;
    }
    
    public void getData(){
       db.getData();
    }
}

/***测试类***/
public void TestBusiness{
   Business business = new Business();
   public void getbusinessdata(){
      business.setDataBase(new SQLServerDataBase());
      business.getData();
   }
}
```

  当业务需求更改时，将数据访问的实现类继承业务通用接口，无需更改业务逻辑层即Business类，在最终测试类中将具体DataBase的形参更改为需要的数据访问类即可。这样很方便的实现了程序的重用、扩展、以及高效率的特性。




