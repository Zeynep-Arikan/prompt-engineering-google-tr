Büyük Dil Modelleri (LLM'ler) doğrudan tek bir token'ı tahmin etmezler. Bunun yerine, bir sonraki token'ın ne olabileceğine dair olasılık dağılımı üretirler ve LLM'in kelime dağarcığındaki her token için bir olasılık değeri belirlenir.

Ardından, bu token olasılıkları örneklenerek (sampling) hangi token'ın üretileceği belirlenir.

Sıcaklık (temperature), Top-K ve Top-P ayarları, tahmin edilen token olasılıklarının nasıl işleneceğini ve tek bir çıktı token'ının nasıl seçileceğini belirleyen en yaygın yapılandırma seçenekleridir.

## Temperature (Sıcaklık)

Temperature, token seçiminde rastgelelik (randomness) derecesini kontrol eder.
Daha düşük sıcaklık değerleri, daha kararlı (deterministik) yanıtlar beklenen istemler için uygundur; buna karşılık, daha yüksek sıcaklıklar daha çeşitli veya beklenmedik sonuçlara yol açabilir.

Sıcaklık değeri 0 olarak ayarlandığında (greedy decoding), her zaman en yüksek olasılığa sahip token seçilir. Ancak, iki token’ın aynı en yüksek tahmin olasılığına sahip olması durumunda, kullanılan eşitlik bozma (tiebreaking) yöntemine bağlı olarak sıcaklık 0 olsa bile her zaman aynı çıktının üretilmeyebileceği unutulmamalıdır.

Maksimuma yakın sıcaklık değerleri daha rastgele çıktılar üretme eğilimindedir. Sıcaklık değeri arttıkça, tüm token'ların bir sonraki tahmin edilen token olma olasılığı neredeyse eşit hale gelir.

Gemini modellerinde sıcaklık kontrolü, makine öğrenmesinde kullanılan softmax fonksiyonuna benzer şekilde anlaşılabilir.Düşük sıcaklık ayarı, düşük softmax sıcaklığına (T) karşılık gelir ve yüksek bir kesinlik oranıyla belirli bir token'ın seçilmesini vurgular.Diğer yandan, yüksek sıcaklık ayarı daha geniş bir token aralığını kabul edilebilir hale getirir.Bu artan belirsizlik, özellikle yaratıcı çıktılar üretmek gibi, katı ve kesin bir yanıtın gerekli olmadığı senaryolarda tercih edilebilir.

## Top-K and Top-P
Top-K ve Top-P (aynı zamanda çekirdek örnekleme — nucleus sampling — olarak da bilinir⁴), LLM’lerde bir sonraki tahmin edilen token’ın, en yüksek tahmin olasılıklarına sahip token’lar arasından seçilmesini sağlamak için kullanılan iki farklı örnekleme yöntemidir.Sıcaklık(Temperature) parametresinde olduğu gibi, bu örnekleme yöntemleri de üretilen metnin rastlantısallığını ve çeşitliliğini kontrol eder.

__Top-K__

Top-K örnekleme, modelin tahmin dağılımındaki en yüksek olasılığa sahip K adet token’ı seçer. Top-K değeri yükseldikçe, modelin çıktısı daha yaratıcı ve çeşitli hale gelir. Top-K değeri azaldıkça, model daha kısıtlı ve gerçeğe dayalı çıktılar üretir. Top-K değeri 1 olarak ayarlandığında, bu durum greedy decoding (en yüksek olasılıklı token'ın her zaman seçilmesi) ile eşdeğerdi

__Top-P__

Top-P örneklemesi, kümülatif olasılığı belirli bir eşik değeri (P) aşmayan en iyi token'ları seçer. P değeri 1 olduğunda ise modelin tüm kelime dağarcığındaki (vocabulary) token'lar seçim için dikkate alınır.


Top-K ve Top-P arasında seçim yapmanın en iyi yolu, her iki yöntemi (veya ikisinin kombinasyonunu) deneyerek ihtiyaca en uygun sonucu üreten ayarları belirlemektir.

## Herşey bir arada

Top-K, Top-P, sıcaklık (temperature) ve üretilecek token sayısı arasında seçim yapmak, uygulamanın özel gereksinimlerine ve istenen çıktıya bağlıdır. Bu ayarların her biri diğerlerini etkileyebilir, dolayısıyla tüm yapılandırmaların birbirleriyle olan ilişkilerini dikkate almak önemlidir. Ayrıca, seçtiğiniz modelin farklı örnekleme ayarlarını nasıl birleştirdiğini anlamanız da kritik öneme sahiptir.

Eğer sıcaklık, Top-K ve Top-P ayarlarının tümü kullanılabiliyorsa (örneğin Vertex AI Studio gibi platformlarda), önce hem Top-K hem de Top-P kriterlerini karşılayan token’lar belirlenir, ardından sıcaklık ayarı bu token'lar arasında seçim yapmak için uygulanır.

Eğer sadece Top-K veya sadece Top-P ayarı mevcutsa, aynı işlem yapılır ancak yalnızca mevcut olan tek bir kriter kullanılır. Sıcaklık ayarı bulunmuyorsa, Top-K ve/veya Top-P kriterlerini karşılayan token’lardan rastgele bir seçim yapılarak bir sonraki tahmin edilen token üretilir. Örnekleme yapılandırmalarından herhangi birinin aşırı uç değerlere ayarlanması durumunda, bu ayar diğer yapılandırmaların etkisini ortadan kaldırabilir ya da onları önemsiz hale getirebilir.

- Sıcaklık (temperature) değeri 0 olarak ayarlandığında, Top-K ve Top-P ayarları önemsiz hale gelir; en yüksek olasılığa sahip token bir sonraki tahmin edilen token olarak seçilir.Sıcaklık değeri aşırı yüksek bir seviyeye ayarlanırsa (1’in üzerinde, genellikle 10 ve üzeri değerlerde), bu kez sıcaklık ayarı etkisiz hale gelir ve Top-K ve/veya Top-P kriterlerini geçen token'lar arasından rastgele seçim yapılarak bir sonraki token belirlenir.

- Top-K değeri 1 olarak ayarlandığında, sıcaklık (temperature) ve Top-P ayarları önemsiz hale gelir.
Çünkü yalnızca bir token Top-K kriterini geçer ve o token bir sonraki tahmin edilen token olur. Top-K değeri aşırı yüksek bir değere (örneğin, LLM’in kelime dağarcığının büyüklüğüne) ayarlanırsa, sıfırdan büyük olasılığa sahip herhangi bir token Top-K kriterini karşılamış olur ve böylece token'lar arasından herhangi biri elenmeden seçim yapılır.

- Eğer Top-P değeri 0’a (veya çok küçük bir değere) ayarlanırsa, çoğu LLM örnekleme yöntemi yalnızca en yüksek olasılığa sahip token'ı dikkate alır ve bu durumda sıcaklık ve Top-K ayarları önemsiz hale gelir. Top-P değeri 1’e ayarlanırsa, sıfırdan büyük olasılığa sahip tüm token'lar Top-P kriterini karşılar ve herhangi bir token elenmeden seçim yapılır.

Genel bir başlangıç noktası olarak, sıcaklık değerini 0.2, Top-P değerini 0.95 ve Top-K değerini 30 olarak ayarlamak, yaratıcı fakat aşırıya kaçmayan, nispeten tutarlı sonuçlar elde etmenizi sağlar. Özellikle yaratıcı sonuçlar elde etmek istiyorsanız, sıcaklık değerini 0.9, Top-P değerini 0.99 ve Top-K değerini 40 olarak ayarlayarak başlayabilirsiniz. Daha az yaratıcı, daha tutarlı sonuçlar almak istiyorsanız, sıcaklık değerini 0.1, Top-P değerini 0.9 ve Top-K değerini 20 olarak belirleyerek işe başlayabilirsiniz. Son olarak, eğer göreviniz her zaman tek bir doğru cevabı gerektiriyorsa (örneğin bir matematik problemini yanıtlamak gibi), sıcaklık değerini 0 olarak ayarlamanız tavsiye edilir.

## Not:
Daha fazla özgürlük tanındığında (yani sıcaklık, Top-K, Top-P ve çıktı token sayısı arttırıldığında), LLM’in ürettiği metin, göreve daha az ilgili veya alakasız olabilir.

## ⚠️ Uyarı:

Bir cevabın büyük miktarda dolgu (gereksiz) kelimelerle sona erdiğini hiç fark ettiniz mi?
Bu durum, tekrar döngüsü hatası (repetition loop bug) olarak adlandırılır ve büyük dil modellerinde (LLM'lerde) sıkça karşılaşılan bir sorundur.Bu hata, modelin bir kelimeyi, ifadeyi veya cümle yapısını tekrar tekrar üretmeye takılmasıyla oluşur ve genellikle uygunsuz sıcaklık (temperature) ile Top-K/Top-P ayarlarının kullanılmasından kaynaklanır.

Bu döngüsel hata hem düşük hem de yüksek sıcaklık ayarlarında meydana gelebilir, ancak farklı nedenlerle ortaya çıkar:

Düşük sıcaklıklarda, model aşırı deterministik davranarak en yüksek olasılıklı yolu katı bir şekilde takip eder. Eğer bu yol daha önce üretilen bir metin parçasına geri dönerse, model bir döngüye girebilir.

Yüksek sıcaklıklarda ise modelin çıktısı aşırı rastlantısal hale gelir. Bu durum, rastgele seçilen bir kelime veya ifadenin şans eseri önceki bir duruma geri dönmesine ve böylece bir döngü oluşmasına neden olabilir.

Her iki durumda da, modelin örnekleme (sampling) süreci "takılır" ve çıktı penceresi dolana kadar monoton ve verimsiz içerik üretmeye devam eder. Bu sorunu çözmek için genellikle sıcaklık ile Top-K/Top-P değerleri üzerinde dikkatli bir şekilde oynamak ve deterministiklik ile rastlantısallık arasında dengeli bir yapılandırma bulmak gerekir.