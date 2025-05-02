## One-shot & few-shot
Yapay zekâ modelleri için istem oluştururken, modele örnekler sunmak oldukça faydalıdır.
Bu örnekler, modelin sizden ne talep ettiğinizi anlamasını kolaylaştırır. Özellikle belirli bir çıktı yapısına veya desenine yönlendirme yapılmak istenen durumlarda örnek kullanımı oldukça etkilidir.

One-shot istem, modele yalnızca tek bir örnek sunar—adı da buradan gelir.
Bu yöntemde model, verilen örneği taklit ederek görevi en iyi şekilde tamamlamaya çalışır.

Few-shot istem ise modele birden fazla örnek sağlar.
Bu yaklaşım, modelin takip etmesi gereken örüntüyü (pattern) açık bir biçimde ortaya koyar.
One-shot yaklaşımıyla benzer bir mantığa dayanır; ancak, istenilen yapıya ait birden çok örnek sunulması, modelin bu yapıyı takip etme olasılığını artırır.

Few-shot prompt için kaç örnek kullanılacağı; görevin karmaşıklığı, örneklerin kalitesi ve kullanılan üretken yapay zekâ (gen AI) modelinin yetenekleri gibi birkaç faktöre bağlıdır. Genel bir kural olarak, few-shot prompt için en az üç ila beş örnek kullanılması önerilir. Ancak, daha karmaşık görevlerde daha fazla örnek gerekebilir veya modelin giriş uzunluğu sınırlamaları nedeniyle daha az örnek kullanılması gerekebilir.

Tablo 2, bir few-shot istem örneğini göstermektedir. Daha önce kullandığımız gemini-pro model yapılandırma ayarlarını bu örnekte de kullanabiliriz; yalnızca, daha uzun yanıt ihtiyacını karşılamak için token sınırı artırılmıştır.

## 📊 Tablo 2 – Few-Shot İstemleme Örneği (Pizza Siparişi)
| **Alan**            | **Değer** |
|---------------------|-----------|
| **Amaç**            | Pizza siparişlerini geçerli JSON formatına dönüştürmek |
| **Model**           | gemini-pro |
| **Sıcaklık**        | 0.1 |
| **Token Sınırı**    | 250 |
| **Top-K**           | N/A |
| **Top-P**           | 1 |
| **İstem (Prompt)**  | Pizza siparişlerini geçerli JSON formatına dönüştürün:  <br><br> **ÖRNEK:** <br> I want a small pizza with cheese, tomato sauce, and pepperoni. <br> **JSON Yanıtı:**  <br> ```json{ 
"size": "small", 
"type": "normal", 
"ingredients": [["cheese", "tomato sauce", "pepperoni"]] 
 } <``` <br><br> **ÖRNEK:** <br> Can I get a large pizza with tomato sauce, basil and mozzarella? <br> ```json  <br> { <br> "size": "large", <br> "type": "normal", <br> "ingredients": [["tomato sauce", "basil", "mozzarella"]] <br> } <br> ``` <br><br> Now, I would like a large pizza, with the first half cheese and mozzarella. And the other half tomato sauce, ham and pineapple. <br><br> **JSON Yanıtı:** |
| **Çıktı**           | ```json <br> { <br> "size": "large", <br> "type": "half-half", <br> "ingredients": [["cheese", "mozzarella"], ["tomato sauce", "ham", "pineapple"]] <br> } <br> ``` |
    

İstemleriniz için örnekler seçerken, gerçekleştirmek istediğiniz göreve doğrudan ilgili örnekler kullanmalısınız.Seçilen örneklerin çeşitli, yüksek kaliteli ve dil açısından düzgün biçimde yazılmış olması gerekir. Küçük bir hata bile modelin kafasını karıştırabilir ve istenmeyen çıktılara yol açabilir.

Eğer modelin farklı türde girdilere karşı dayanıklı (robust) yanıtlar üretmesini istiyorsanız, örneklerinizin içine uç durumları (edge cases) da dahil etmeniz önemlidir.Uç durumlar; alışılmadık veya beklenmeyen girdilerdir, ancak modelin yine de bu girdileri doğru şekilde işleyebilmesi gerekir.