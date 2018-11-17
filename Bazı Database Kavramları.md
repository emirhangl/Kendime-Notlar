# Konu Başlıkları

1. [ActiveX Data Object (ADO.NET) nedir ?](#adonet-activex-data-object-nedir-)
2. [Data Access Object (DAO) nedir ?](#daodata-access-object-nedir-)
3. [Data Access Layer (DAL) nedir ?](#daldata-access-layer-nedir-)
4. [DB Abstraction Layer (DBAL) nedir ?](#dbaldb-abstraction-layer-nedir-)
5. [Database Management System (DBMS)&(JDBC) nedir ?](#dbmsdatabase-management-system-jdbcjava-database-connectivity-nedir-)
6. [Object Relational Mapping (ORM) nedir ?](#ormobject-relational-mapping-nedir)
7. [Entity Framework (EF) nedir ?](#entity-framework-nedir-)
8. [Entity Relationship Model nedir ?](#entity-relationship-model-nedir-)
9. [Language Integrated Query (LINQ) nedir ?](#linq-language-integrated-query-nedir-)



* ## ADO.NET (ActiveX Data Object) nedir ?

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
* ## DAO(Data Access Object) nedir ?

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
* ## DAL(Data Access Layer) nedir ?
  
  Database connection'ı oluşturmak SELECT, INSERT, UPDATE ve DELETE komutları ve benzeri komutlar DAL'ın içinde yer almalı.
  Data Access Layer tipik olarak DB'ye erişmek için kullanılan methodları içerir.
  Örnek olarak:
  ```csharp
    GetCategories(), //Tüm kategoriler hakkında bilgi döner.
    GetProducts(), //Tüm ürünler hakkında bilgi döner.
    GetProductsByCategoryID(categoryID), //Bir kategoriye ait tüm ürünleri döner.
    GetProductByProductID(productID), // Specific bir ürün hakkında bilgi döner.
  ```
* ## DBAL(DB Abstraction Layer) nedir ?

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
* ## DBMS(Database Management System)-(JDBC(Java Database Connectivity) nedir ?

  *DBMS* database oluşturma ve yönetme yazılım sistemine denir. Bu sistem bilgisayar bilimiyle uğraşanlar insanalara
   sistematik   ve güvenli bir şekilde veri alma, güncelleme ve yönetme olanağı sağlayan sistemdir.
   
   *JDBC* özellikle bir ilişkisel veritabanında saklanan veriler olmak üzere her türlü tablo verilerine erişebilen bir Java API'sıdır. *JDBC*, Windows, Mac OS ve çeşitli UNIX sürümleri gibi çeşitli platformlarda Java ile çalışır. _(Write once, run everywhere)_
* ## ORM(Object Relational Mapping) nedir
  
  ORM object oriented paradigmasını kullanarak, database'i query'lememizi ve manipüle etmemizi sağlayan tekniktir.
  Bir ORM kütüphanesi senin, programlama dil tercihinde yazılmış, db'yi manipüle edecek kodları encapsule eden kütüphanedir.
  Böylece, bundan sonra SQL ile uğraşmazsın; direkt object ile etkileşime geçersin.
  Aşağıdaki i. örnekte 'linux' yazarının kitaplarını getirmek için normalde aşağıdaki kullanılırken, ORM kütüphanesi kullanılırsa kod ii. örnekteki gibi gözükür.
  1. Örnek 
  ```
    book_list = new List();
    sql = "SELECT book FROM library WHERE author = 'Linus'";
    data = query(sql); // I over simplify ...
    while (row = data.next())
    {
         book = new Book();
         book.setAuthor(row.get('author');
         book_list.add(book);
    }
  ```
  2. Örnek 
  ```
    book_list = BookTable.query(author="Linus");
  ```
  ##### Dillere göre kullanılan kütüphaneler:
  - Java: Hibernate.
  - PHP: Propel or Doctrine (I prefer the last one).
  - Python: the Django ORM or SQLAlchemy (My favorite ORM library ever).
  - C#: NHibernate or Entity Framework
  
* ## Entity Framework nedir ?

    DB aktivitelerini otomatize etmek için [ORM](#ormobject-relational-mapping-nedir) tekniğiyle Microsoft tarafından geliştirilmiş bir framework'tür.
    
    .NET 3.5 öncesi, developerlar, uygulama verilerini ilgili DB'ye kaydetmek veya getirmek için sıklıkla ADO.NET kodu yazarlardı.
    Bunun için database connection açılırdı, veriyi çekmek veya göndermek için DataSet oluşturulurdu. Ayrıca bu veriler DataSet'ten .NET object'lere ve tam tersi olmak üzere convert edilirdi. Bu hantal ve hataya eğilimli bir yöntemdi. Bunun üzerine, bütün bu DB bağlantılı aktiviteleri otomatize etmek için Microsoft cc. Entity Framework'u yarattı.
    
    _Entity Framework Microsft tarafından desteklenen açık kaynaklı bir [ORM](#ormobject-relational-mapping-nedir) framework'üdür._
    ```csharp
      namespace Geocat.Data
      {
        public class CityDb
        {
            /// <summary>
            /// veri tabanına yeni bir şehir eklemek için kullanılan fonksiyon
            /// </summary>
            /// <param name="_city">
            /// Veri tabanına eklenmek istenen city objesi
            /// </param>
            /// <returns>
            /// Eklenen city objesi, Id bilgisi ile birlike geri döner
            /// </returns>
            public city AddNewCity(city _city)
            {
                try
                {
                    using (var context = new GeoCatEntities())
                    {
                        context.cities.Add(_city);
                        var numberofAddedObject = context.SaveChanges();

                        return numberofAddedObject > 0 ? _city : null;
                    }
                }
                catch (Exception)
                {

                    throw;
                }
            }

            public bool DeleteCity(int _id)
            {
                try
                {
                    using (var context = new GeoCatEntities())
                    {
                        var deleteObject = context.cities.FirstOrDefault(c => c.Id == _id);

                        if (deleteObject != null)
                        {
                            context.cities.Remove(deleteObject);
                            int numberOfDeleteObjet = context.SaveChanges();

                            return numberOfDeleteObjet > 0;

                        }

                        return false;
                    }
                }
                catch (Exception)
                {

                    throw;
                }
      }
    ```
    [CodeFirst proje geliştirmek için örnek link](https://docs.microsoft.com/tr-tr/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)
    
    [DatabaseFirst proje geliştirmek için örnek link](https://docs.microsoft.com/tr-tr/aspnet/mvc/overview/getting-started/database-first-development/setting-up-database)
    
* ## Entity Relationship Model nedir ?

  ER Diagram olarak da biliniyor. Database'imizin haritasını çıkardığımız modeldir. Bununla DB'mizdeki bütün entity'lerin ilişkilerini, ilişkiler arasındaki cardinality(sayısallıkları)'ı veya özelliklerini belirleriz veya görürüz. Örnek vermek gerekirse:
  ![ER Model](http://seoclearly.com/wp-content/uploads/2018/08/relationship-diagram-templates-boat-jeremyeaton-co.svg "ER Model")
  
  **Cardinality'leri anlamamız için görsel:**
  
  <p align="center">
  <img width="460" height="300" src="https://d2slcw3kip6qmk.cloudfront.net/marketing/pages/chart/erd-symbols/ERD-Notation.PNG">
  </p>

* ## LINQ (Language Integrated Query) nedir ?

  LINQ query yeteneklerinin direkt olarak C# diline entegre edilmesine denir.
  Query ifadeleri bilinen query syntax'i ile yazılır. Böylece, filtreleme, sıralama ve gruplama işlemlerini minimum kod ile yapmış oluruz.
  
  **Query işlemi 3 temel aksiyondan oluşuyor:**
  1. Data kaynağını içermek
    ```csharp
        // XML için
        // using System.Xml.Linq;
        XElement contacts = XElement.Load(@"c:\myContactList.xml");
    ```
    ```csharp
        // DB için
        Northwnd db = new Northwnd(@"c:\northwnd.mdf");  

        // Query for customers in London.  
        IQueryable<Customer> custQuery =  
            from cust in db.Customers  
            where cust.City == "London"  
            select cust;
    ```
  2. Query'i yazma
  3. Query'i execute etme
  
  Aşağıdaki örnekte query işlemini görelim. Örnekte, data source oluşturuluyor, query yazılıyor ve foreach ifadesinde execute ediliyor 
  ```csharp
      class LINQQueryExpressions
      {
          static void Main()
          {

              // Specify the data source.
              int[] scores = new int[] { 97, 92, 81, 60 };

              // Define the query expression.
              IEnumerable<int> scoreQuery =
                  from score in scores
                  where score > 80
                  select score;

              // Execute the query.
              foreach (int i in scoreQuery)
              {
                  Console.Write(i + " ");
              }            
          }
      }
      // Output: 97 92 81
  ```
  
  
