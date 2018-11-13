* **ADO.NET (ActiveX Data Object) nedir ?**

  Microsoft tarafından sunulan, uygulamalarımız için veritabanı işlemlerini(CRUD) yapmamızı sağlayan bir framework'tür.
  Sadece SQL Server için değil ayrıca *Access* ve *Oracle* için de kullanabiliriz.
  ```csharp
  static void Main()
    {
        string connectionString =
            "Data Source=(local);Initial Catalog=Northwind;"
            + "Integrated Security=true";

        // Provide the query string with a parameter placeholder.
        string queryString =
            "SELECT ProductID, UnitPrice, ProductName from dbo.products "
                + "WHERE UnitPrice > @pricePoint "
                + "ORDER BY UnitPrice DESC;";

        // Specify the parameter value.
        int paramValue = 5;

        // Create and open the connection in a using block. This
        // ensures that all resources will be closed and disposed
        // when the code exits.
        using (SqlConnection connection =
            new SqlConnection(connectionString))
        {
            // Create the Command and Parameter objects.
            SqlCommand command = new SqlCommand(queryString, connection);
            command.Parameters.AddWithValue("@pricePoint", paramValue);

            // Open the connection in a try/catch block. 
            // Create and execute the DataReader, writing the result
            // set to the console window.
            try
            {
                connection.Open();
                SqlDataReader reader = command.ExecuteReader();
                while (reader.Read())
                {
                    Console.WriteLine("\t{0}\t{1}\t{2}",
                        reader[0], reader[1], reader[2]);
                }
                reader.Close();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            Console.ReadLine();
        }
    ```
* **DAO(Data Access Object) nedir ?**

  Data Acces Object temel olarak, bizim database'e erişmemizi sağlayan bir object veya interface'dir.
  Örnekle: Employee'mizi temsil edecek bir entity'miz olduğunu düşünelim;
  ```csharp
    public class Employee {

      private int id;
      private String name;


      public int getId() {
          return id;
      }
      public void setId(int id) {
          this.id = id;
      }
      public String getName() {
          return name;
      }
      public void setName(String name) {
          this.name = name;
      }

    }
  ```
  Bir Employee entity'sini manipüle etmek için gereken veritabanı işlemlerini yürütmek için basit bir DAO arayüzü şöyle olabilir:
  ```csharp
    interface EmployeeDAO {

    List<Employee> findAll();
    List<Employee> findById();
    List<Employee> findByName();
    boolean insertEmployee(Employee employee);
    boolean updateEmployee(Employee employee);
    boolean deleteEmployee(Employee employee);

    }
  ```
  Bir sonraki adım bu interface'i implement edip içini doldurmak olacaktır.
* **DAL(Data Access Layer nedir ?**
  
  Database connection'ı oluşturmak SELECT, INSERT, UPDATE ve DELETE komutları ve benzeri komutlar DAL'ın içinde yer almalı.
  Data Access Layer tipik olarak DB'ye erişmek için kullanılan methodları içerir.
  Örnek olarak:
  ```csharp
    GetCategories(), //Tüm kategoriler hakkında bilgi döner.
    GetProducts(), //Tüm ürünler hakkında bilgi döner.
    GetProductsByCategoryID(categoryID), //Bir kategoriye ait tüm ürünleri döner.
    GetProductByProductID(productID), // Specific bir ürün hakkında bilgi döner.
  ```
* **DBAL nedir ?**

  My understanding is that a data access layer does not actually abstract the database, but rather makes database operations
  and query building easier.
  For example, data access layers usually have APIs very similar to SQL syntax that still require knowledge of the database's 
  structure in order to write:
  ```
    $Users->select('name,email,datejoined')->where('rank > 0')->limit(10);
  ```
  Data abstraction layers are usually full blown ORM's (Object-Relational Mappers) that theoretically prevent the need to
  understand any underlying database structure or have any knowledge of SQL. The syntax might be something like this:
  ```
    Factory::find('Users', 10)->filter('rank > 0');
  ```
  And all the objects might be fully populated with all the fields, possibly joined with any parent or child objects 
  if you set it that way.
  [link](https://stackoverflow.com/questions/2838661/what-is-the-difference-between-database-abstraction-layer-data-access-layer)
* **DBMS-(JDBC) nedir ?**

  
* **ORM nedir ?**

* **Entity Framework nedir ?**

* **Entity Relationship Model nedir ?**
