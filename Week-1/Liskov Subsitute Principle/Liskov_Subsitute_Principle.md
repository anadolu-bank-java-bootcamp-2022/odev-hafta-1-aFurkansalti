# Liskov Subsitute Principle

### Liskov Subsitute Principle nedir?
Barbara Liskov'un ortaya atmış olduğu bu prensibe göre üst sınıf ile alt sınıf veya alt sınıflar arasında davranışsal olarak hiç bir fark olmamalıdır ve birbirlerinin yerine kullanılabilmelidirler. Örneğin bir oyuncak arabaya dört adet tekerlek taktık fakat bu tekerler dönme özelliğine sahip değiller ve araba bu sebepten dolayı ileri geri hareket edememekte, işte bu durum LSP'ye aykırı bir durumdur. Yani bir özellik atadıysak bu özelliğin gerçekten bir işlevi olmalıdır. LSP, Dummy Code dediğimiz buna benzer durumların önüne geçmektedir. Azıcık daha teknik bir dille ifade etmek gerekirse, Base Class'ların (katılım veren sınıfların), Sub Class'lardaki (katılım alan sınıflardaki) işlevselliği tam olarak yerine getirmiş olduğundan emin olmalıyız. 

### Bu Prensipteki Temel Amaç 
Bu prensip, alt sınıflarınızın nesnelerinin, üst sınıfınızın nesneleriyle aynı şekilde davranmasını gerektirir. Bir üst sınıfın nesnesinin uygulamada sorunlara neden olmadan alt sınıflarının nesneleri tarafından değiştirilebilmesi gerektiğini belirtir. Özellikte bu metotlarda instanceof gibi nesnelerin tipleri arasında kıyaslama yapılmak zorunda kalındığı zaman, LSP prensibi çiğnenmiş olur ki, bu alt sınıfların varlığından haberdar olunduğu anlamına gelir. İdeal olan ise kullanıcı sınıfları alt sınıfların varlığını bilmesidir.

### Liskov Subsitute Principle nasıl uygulanır?
Bu prensibin uygulanışını örnekle açıklamak daha anlaşılır olacaktır. Şimdi, kitapçı, uygulamaya yeni bir teslimat işlevi eklememizi istiyor. Bu nedenle, müşterileri siparişlerini alabilecekleri konum sayısı hakkında bilgilendiren bir "OrderofBook" sınıfı oluşturuyoruz:
```Java
   class OrderofBook {
	String title;
	int userID;
 
	void getOrderLocation() {/***/}
}
```
Bununla birlikte, mağaza aynı zamanda sadece kendi cadde mağazalarına teslim etmek istedikleri lüks deri kılıflı kitaplar da satmaktadır. Bu yüzden, "OrderofBook" öğesini genişleten ve "getOrderLocation()" yöntemini kendi işlevselliği ile geçersiz kılan yeni bir "OrderofHardcover" alt sınıfı oluşturuyoruz:
```Java
class OrderofHardcover extends OrderofBook {
 
	@Override
	void getOrderLocation() {/***/}
 
}
```
Daha sonra, mağaza bizden ekitaplar için teslimat işlevleri oluşturmamızı istedi. Şimdi, mevcut "OrderofBook" sınıfını bir "OrderofEBook" alt sınıfı ile genişletiyoruz. Ancak, "getOrderLocation()" yöntemini geçersiz kılmak istediğimizde, ekitapların fiziksel konumlara teslim edilemeyeceğini biliyoruz.
```Java
class OrderofEBook extends OrderofBook {
 
	@Override
	void getOrderLocation() {/* fiziksel konum olmadığı için bunu kullanamayız*/}
}
```
Bununla birlikte, "getOrderLocation()" yönteminin bazı özelliklerini değiştirebiliriz, ancak Liskov Subsitute Principle ihlal edilmiş olacaktır. Değişiklikten sonra, uygulamayı bozmadan "OrderofBook" üst sınıfını "OrderofEBook" alt sınıfıyla değiştiremedik.

Sorunu çözmek için, kalıtım hiyerarşisini düzeltmemiz gerekir. Kitap teslim türlerini daha iyi ayırt eden ekstra bir katman sunalım. Yeni "OfflineOrder" ve "OnlineOrder" sınıfları, "OrderofBook" üst sınıfını ayıralım. Ayrıca, "getOrderLocation()" yöntemini "OfflineOrder" öğesine taşır ve "OnlineOrder" sınıfı için yeni bir "getSoftwareOption()" yöntemi oluştururuz (bu, çevrimiçi teslimatlar için daha uygundur).
```Java
class OrderofBook {
 
	String title;
	int userID;
 
}
 
class OfflineOrder extends OrderofBook {
 
	void getOrderLocation() {...}
 
}
 
class OnlineOrder extends OrderofBook {
 
	void getSoftwareOption() {...}
 
}
```
Refactor yapılan kodda "OrderofHardcover", "OfflineOrder" öğesinin alt sınıfı olacak ve kendi işlevleriyle "getOrderLocation()" yöntemini geçersiz kılacaktır.

"eKitapSiparis", şu anda "getOrderLocation()" yöntemiyle uğraşması gerekmediği için iyi bir haber olan "OnlineOrder"in alt sınıfı olacaktır. Bunun yerine, ebeveyinin "getSoftwareOption()" yöntemini kendi uygulamasıyla geçersiz kılabilir.
```Java
class OrderofHardcover extends OfflineOrder {
 
	@Override
	void getSoftwareOption() {...}
 
}
 
class eKitapSiparis extends OnlineOrder {
 
	@Override
	void getSoftwareOption() {...}
 
}
```
Yenileme işleminden sonra, herhangi bir alt sınıfı, üst sınıfı yerine uygulamayı kırmadan kullanabiliriz.


