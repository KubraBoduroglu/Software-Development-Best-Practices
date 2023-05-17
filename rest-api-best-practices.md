# Rest Api Best Practices by Gökhan Ayrancıoğlu url:https://github.com/G-khan at Turkey Java Community YouTube Channel  
https://www.youtube.com/live/aNr3QXvPpFU?feature=share  

Api Naming  
* Resource'lar isim olmalı, fiil değil.  
* Resource'lar çoğul olarak isimlendirilmeli.  
* GET,POST,PUT,DELETE,PATCH(parçalı update)  
* İsimlendirmelerin tümü küçük harflerle olmalı.  
* URI'nin en sonunda /(slash) kullanılmamalı.  
* Hiyerarşik ilişkileri belirtmek için /(slash) kullanılmalı.  
* Dosya uzantıları kullanılmamalı.  
* Boşluk veya birden fazla kelime için -(tire) kullanılmalı. 
 
Collection Filtering  
* Collection'ları filtrelemek, sıralamak, listelemek için query parametreleri kullanabiliriz.  
* API tarafından döndürülen alanları sınırlamak için field anahtar kelimesini kullanabiliriz.  

Http Header  
* Request responların hangi dil, hangi içerik için olduğunu http header'ında belirtebiliriz.  
Microservice Mimarisinde REST API İsimlendirmesi  
* Özellikle internal haberleşmelerde karmaşayı önlemek için servis isimleriyle uri'yi başlatmak iyi bir çözüm olabilir.  
Versiyonlama  

*  API'yi kullanan ürün ve hizmetleri bozmamak için api versiyonlama kullanılması önemli.  
*  2 şekilde kullanımı var: 
  * api/v1/....  
  * api/v2.1/...  
*  Bu API versiyonlama, servis versiyonlamadan farklı.  

Pagination  
* offset, limit as query params

API Dokümantasyonu  
* Swagger, Postman  

HTTP Status Codes  
* 1xx: Bilgi. İstek sunucuya ulaştı.  
* 2xx: Başarılı. İstek sunucuya ulaştı, anlaşıldı, başarılı.  
* 3xx: Yönlendirme. Erişilmek istenen resource'un  başka kaynağa taşındığını ve yönlendirme gerektiğini ifade eder.  
* 4xx: Client kaynaklı hata. İstek yerine getirilmedi, geçersiz, doğru veri gönderilmedi, resource'a ulaşılamadı.  
* 5xx: Server kaynaklı hata. İstek sunucuya ulaştı,sunucudaki sorunlar nedeniyle yerine getirilemedi.  

2xx Başarılılar:  
* `200 OK`         - `GET` : İşlem başarılı  
* `201 CREATED`    - `POST`: Yeni resorce başarıyla oluşturuldu  
* `202 ACCEPTED`   - `POST`: Sunucu isteği kabul etti, işleme alacak (async. yapılar örnek).  
* `204 No Content` - `DELETE` :  Resource boş / resource silindi  

4xx Başarısızlar:  
400 Bad Request: Geçersiz istek/query  
401 Unauthorized: Yetki gerekiyor  
403 Forbidden: Sunucu isteği reddetti/ isteğe yetkiniz yok  
404 Not Found: Resource yok  
405 Method Not Allowed: Geçersiz HTTP metodu (sunucu get beklerken post geldi vs gibi)  
409 Conflict: Uyumsuz/ eski versiyondaki istek  
429 Too Many Request: Çok fazla istek  
415 Unsupported Media Type: Desteklenmeyen/ yanlış content type  


5xx Sunucu Kaynaklı Hatalar:  
500 Internal Server Error: Sunucuda bir hata oldu (null pointer gibi)  
501 Not Implemented: Sunucu istenilen isteği yerine getirecek şekilde yapılandırılmadı  
502 Bad Gateway: Gateway/sunucu kaynak sunucudan cevap alamıyor (çok yoğun trafik alan sitelerde)  
503 Service Unavailable: Sunucu şu anda hizmet vermiyor (çok yoğun trafik alan sitelerde)   
504 Gateway Timeout: Gateway/sunucu kaynak sunucudan belirli bir zaman içinde cevap alamadı (çok yoğun trafik alan sitelerde)  

Response Modelleri  
* İçerikler JSON olmalı  
* HTTP response kodları, dönen response içeriğiyle eşleşiyor olmalı  
 - 204 No content - içerik dönülmemeli  
 - 4xx responseları hata detaylarını içermeli  
* Response modelleriniz olabildiğince açk ve API'lerinizi kullanan geliştiricilerin işine yarayacak ve kolayca kullanabilecekleri yapıda olmalı  

API Security  
SSL kullanarak API'nizi saldırılara karşı daha güvenli hale getirin  
https -> SSL ile secure HTTP  

REST API Authentication  
* REST'in direkt sağladığı bir güvenlik katmanı bulunmamaktadır  
* Güvenlik için oturum tabanlı kimlilk doğrulama kullanılmalıdır  
* OAuth 2.0 ve JWT popüler olarak kullanılan en önemli yöntemlerdendir  
* OAuth 2.0 ve Open-Id ile birlikte SSO(Single Sign On) yapısına oldukça fazla başvurulmaktadır  
* Auth0 ve Okta en önemli sağlayıcılardandır  

REST API'de Güvenlik  
* Authorization: Yetkilendirme ile kullanıcı rollerine göre ulaşılabilecek API'ler  
* Validation:   
* Rate limiting: Network trafiğine sınırlama  

Q & A  
* Validation'lar client'da mı server'da mı yapılmalı?  
  Hem client hem server  
* Veri çekerken get yerine, post ile body bilgisinde filtre bilgisini vererek çekmek best practice e aykırı mı?  
  Aykırı. Siz xzellikle bir veri seti çekiyorsanız get kullanmanız gerekiyor. Get kullanırken de, get in sonrasın ?key=value pair olarak vermemiz gerekiyor  
* @GetMapping'de @RequestBody gönderilebilir mi?  
  Gönderilebilir ama best practice değil. Get işlemi yapıyorsanız key-value pair olarak göndermeniz gerekiyor. 
* @PathVariable ve @RequestParam farkı nedir?  
  @PathVariable kimliktir. Hangi user olduğunu gösterir. Özellikle Id'ler ile işlem yapıyorsam PathVariable kullanıyorum. .../api/users/1  
  @RequestParam ise hangi filtreleme, sıralama, limitleme çeşidi olduğunu gösterir. .../api/users?maxRecords=2
  
