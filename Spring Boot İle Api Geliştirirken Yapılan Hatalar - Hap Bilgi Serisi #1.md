
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
![App Screenshot](https://github.com/KubraBoduroglu/Software-Development-Best-Practices/blob/main/utils-s%C4%B1n%C4%B1flar.png)  

1. Spring context dışında, Spring bean'inin kullanamadığımız yerlerde bu sınıfı kullanamayız. Mesela bir enum içerisine bunu dahil edemeyiz. 
2. Spring Context'ine gereksiz bir sınıf yüklemiş oluyoruz. Bu da contexti yorar. Buna zaten ihtiyaç kalmaması lazım. Utils içindeki metotları zaten `static` tanımladığımızda zaten Utils sınıfının objesinin oluşmasına ihtiyaç yok, o zaman da `@Component` olmasına ihtiyaç yok.  

### 4. Exception Handling  
* Spring'in exception desteği; Spring uygulama içerisinde fırlattığımız RuntimeException'ları yakalayıp işleyebiliyor.  Global Exception yapısı bilinmiyorsa genelde metot bir try-catch'e alınıp catch'de exception fırlatılıyot veya return true/false yapılıyor.  
* Ancak Spring ile `GlobalExceptionHandler` sınıfı tanımlanabilir. Bu sınıfın içerisinde `@ExceptionHandler()` ile metotlar işaretlenir. Böylece o exception uygulama iiçerisinde nerede fırlatılırsa Spring onu yakalar.  
![App Screenshot]()  

### 5. `@ResponseBody` Eklemeye Gerek Olmaması  
* Eğer sınıfın `@RestController` ise `@ResponseBody` kullanmana gerek yok.  
`@RestController = @Controller + @ResponseBody`  

### 6. Her Sınıfa Bir Interface Oluşturulması Da Gereksiz  
* Her bir sınıf için bir interface çok da gerekli değil. Tek yaptığı Controller'dan alıp service'e gitmek ise olmasa da olur.    
* Interface nerelerde kullanılıyor: 2 servis birbiri ile sürtüşüyor ise burada kullanılabilir. Veya client'In içindeki sorumlulukları gizlemek istiyorsan araya bir Interface koyulabilir. Bir davranışı birden fazla sınıf implemente ediyorsa.  
* Her bir sınıf için bir interface oluşturulması aslında bir alışkanlık. Spring'in eski sürümleri JDK'nın dynamic proxy'sini kullanıyor. O da interface olmadan o sınıfın instance'ını oluşturamıyor. Instance oluşmayınca context'e dahil edemiyor, context'e dahil edemeyince de uygulama ayağa kalkmıyor.  
* Bunu nasıl çözdüler: SCL proxy kullandılar. SCL proxy de o uygulamayı inject alarak o sınıfın proxy'sini oluşturuyor yani bildiğimiz extend alıyor. 

### 7. REST API İsimlendirmeleri
* REST API standartlarındaki gibi isimlendirmeli.  
* Fiil değil isim kullanımalı  
* Çoğul kullanımalı.  
* Versiyonlama yapılmalı.  

### 8. Lombok Kullanımı  
* `@Data` anotasyonunu sorun yaratıyor. Performans sorunlarına sebep olabiliyor. Çünkü içinde `toString` var, çok büyük entitylerde bekletebiliyor. Ayrıca ,`equals` ve `hashCode` var, one-one, many-to-one ilişkilerde sonsuz döngülere sebep olabiliyor.  
* Çözüm: Gerektiği yerde gerektiği kadar Lombok anotasyonu kullanmak. `@Data` yerine `@Getter`,`@Setter`,`@NoArgs` gibi. `equals`, `hashCode`, `toString` yerine metot implementasyonu yapma gibi.  

### 9. @RequestParam Kullanımı  
* İstekte belli bir pattern varsa `@RequestParam` kullanılabilir. Hepsiburada vs URL'lerindeki gibi.  
* `@PathVariable` daha spesifik örnekler için. Bir resource'u getirdiğimiz zaman, bir id ile mesela.  
* Genişleyebilecek endpointlerde, isteklerde `@RequestParam` yerine `@RequestBody` kullanabiliriz. query işletiyorsak, core bussiness yapmıyorsak `@RequestParam` kullanılabilir.  

## TODO
* Null object pattern  
* service mesh  
