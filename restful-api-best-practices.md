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
