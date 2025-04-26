Bir LLM'nin nasıl çalıştığını hatırlayın; bu bir tahmin motorudur. Model, ardışık metni girdi olarak alır ve ardından eğitildiği verilere dayanarak sonraki belirtecin
ne olması gerektiğini tahmin eder.Büyük Dil Modeli (LLM), bu işlemi sürekli olarak yineleyerek çalışır. Model, önceki adımda tahmin ettiği kelimeyi mevcut metnin sonuna ekler 
ve ardından bir sonraki kelimeyi öngörmeye çalışır. Bir sonraki kelimenin tahmini, önceki kelimeler arasındaki ilişkiler ile modelin eğitim sürecinde öğrenmiş 
olduğu bilgiler temel alınarak gerçekleştirilir.

Bir istem (prompt) yazdığınızda, LLM’in doğru token dizilimini tahmin edebilmesini sağlamaya çalışırsınız.
İstem mühendisliği (prompt engineering), LLM’leri doğru çıktılar üretmeye yönlendirecek yüksek kaliteli istemler tasarlama sürecidir.
Bu süreç, en uygun istemi bulmak için çeşitli denemeler yapmayı, istemin uzunluğunu optimize etmeyi ve yazım tarzı ile yapısını verilen görevle uyumlu olacak şekilde değerlendirmeyi kapsar.
Doğal dil işleme ve büyük dil modelleri (LLM) bağlamında, istem; modele bir yanıt ya da tahmin üretmesi amacıyla sağlanan girdidir.

Bu istemler, metin özetleme, bilgi çıkarımı, soru-cevap, metin sınıflandırma, dil veya kod çevirisi, kod üretimi ile kod dokümantasyonu veya
akıl yürütme gibi çeşitli anlama ve üretim görevlerini gerçekleştirmek amacıyla kullanılabilir.

Basit ve etkili istem örnekleri içeren Google'ın istem kılavuzlarına²,³ başvurabilirsiniz.

İstem mühendisliğine başlarken öncelikle bir model seçmeniz gerekir.
İster Vertex AI içindeki Gemini dil modellerini, ister GPT, Claude ya da Gemma veya LLaMA gibi açık kaynaklı modelleri kullanıyor olun, istemlerin seçtiğiniz modele özel olarak optimize edilmesi gerekebilir.

İsteme ek olarak, büyük dil modelinin (LLM) çeşitli yapılandırmalarıyla da denemeler yapmanız gerekecektir.
