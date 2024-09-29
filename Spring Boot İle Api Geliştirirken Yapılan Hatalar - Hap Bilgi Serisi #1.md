
## [Spring Boot İle Api Geliştirirken Yapılan Hatalar - Hap Bilgi Serisi #1](https://www.youtube.com/watch?v=B2wHTdKzpok&list=WL&index=7)  

### 1. @Autowired Kullanımından Kaçınılmalı  
* Sonarqube entegre edildiğinde `@Autowired` kullanma diyor.  
* İki başlıkta inceleniyor: 
  1. Testabilitiy  
* `@Autowired` kullanmadığımız zaman genelde constructor injection yapıyoruz. Böyle olunca bu injeciton `final` tanımlanıyor ve bir de constructor oluşturuluyor.   
````  
private final RequestHandlerFilter requestHandlerFilter;

public PaymentGateway(RequestHandlerFilter requestHandlerFilter) {
  this.requestHandlerFilter = requestHandlerFilter;
}
````   
* Const oluşturup sonra testin, yazdığımız zaman bizi bununla gelen bütün bağımlılıkların testini yazmaya zorluyor.   
  2. Mutability  
* Spring context'inde bu class için değişiklik olabilir, ama bu nadir bir durumdur. Ama bir lib yazıyorsanız ve uygulama içinde sıklıkla reflection kullanıyorsanız ve Spring içerisindeki bu fieldlara erişip bu fieldların stateini değiştiriyorsanız burada önemli olur.  
* Nesne Singleton bir nesneyse ve `final` ile işaretlediyseniz kolay kolay gidip bunu değiştiremezsiniz. Çünkü `final` tanımladınız ve değiştirilemez olduğunu artık kanıtladınız.  
  3. Kompleks Sınıfların Fark Edilmesi  
`@Autowired` aslında çok karmaşık sınıfların kompleksitesini biraz gizliyor gibi. Bu durum ne zaman refaktör yapmamıza gerek olduğunu fark edemememize sebep olaibilir. Refaktörü gecikterebilir.  

### 3. Chain of Responsibility Pattern'i Kullanılabilir  
* Kompleksiteden kaçınmak için kullanılabilir. Design pattern'leri daha iyi kullanabilmek için de OOP daha iyi öğrenilmeli.  
* Örnek olarak, metoda gelen parametrelerden biri flag olarak kullanılıp, bu ``flag=true`` ise bir filter değilse başka bir filter kullanılabilir. Spring de arka planda benzer şeyler yapıyor. If else yerine chaning de yapılaibilir.  
* Null object pattern  
### Utils Sınıflarının Kullanımı  
* Utils sınıfı için `@Component` anotasyonunu kullanıyorsak bu doğru değil. Neden: 
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

1. Spring context dışında, Spring bean'inin kullanamadığımız yerlerde bu sınıfı kullanamayız. Mesela bir enum içerisine bunu dahil edemeyiz. 
2. Spring Context'ine gereksiz bir sınıf yüklemiş oluyoruz. Bu da contexti yorar. Buna zaten ihtiyaç kalmaması lazım. Utils içindeki metotları zaten `static` tanımladığımızda zaten Utils sınıfının objesinin oluşmasına ihtiyaç yok, o zaman da `@Component` olmasına ihtiyaç yok.  

### 4. Exception Handling  
* Spring'in exception desteği; Spring uygulama içerisinde fırlattığımız RuntimeException'ları yakalayıp işleyebiliyor. 

## TODO
* Null object pattern  
