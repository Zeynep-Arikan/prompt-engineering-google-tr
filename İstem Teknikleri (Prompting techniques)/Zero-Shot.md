## Genel İstemleme / Zero-Shot

Zero-shot istem, en basit istem türüdür. Bu tür istemler yalnızca yapılacak görevin tanımını ve LLM'in işleme başlayabilmesi için kısa bir metin sunar. Bu giriş metni bir soru, bir hikâyenin başlangıcı ya da doğrudan talimatlar olabilir. “Zero-shot” ifadesi, örneksiz anlamına gelir—yani modele herhangi bir örnek verilmeden sadece görev tanımı sunulur.

Bu tekniği test etmek için Vertex AI içindeki Vertex AI Studio (Language modülü) kullanılabilir. Bu platform, istemleri denemek için etkileşimli bir çalışma alanı sunar. Aşağıda yer alan Tablo 1’de, film yorumlarını sınıflandırmak amacıyla kullanılan örnek bir zero-shot istemi göreceksiniz.
Bu şekilde tablo biçiminde istemleri belgelemek, istem mühendisliği sürecini takip etmenin son derece etkili bir yoludur.Gerçek dünyada kullandığınız istemler çoğunlukla birçok kez yineleme (iteration) sürecinden geçer ve son hâline ulaşmadan önce defalarca gözden geçirilir.
Bu nedenle, istem geliştirme sürecini disiplinli ve yapılandırılmış bir şekilde belgelemek önemlidir. Bu bölümün ilerleyen kısımlarında (“İstem Geliştirme Sürecinin Belgelenmesi" başlığı altında), bu tablo biçimi, istem mühendisliğinin izlenmesinin önemi ve geliştirme adımları detaylı şekilde ele alınacaktır.

Bu görev için modelin sıcaklık (temperature) değeri düşük bir değere ayarlanmalıdır, çünkü bu senaryoda yaratıcılığa ihtiyaç yoktur. Ayrıca, gemini-pro modelinde varsayılan olarak gelen Top-K ve Top-P ayarları, etkili bir şekilde devre dışı bırakılmıştır (yani çok düşük ya da sınırsız ayarlardadır).(Bkz. “LLM Çıktı Yapılandırması” bölümü.) Üretilen çıktıya dikkat etmek önemlidir.
Örnekte geçen "disturbing" (rahatsız edici) ve "masterpiece" (başyapıt) gibi zıt anlamlı kelimeler aynı cümlede yer aldığından, modelin tahmini daha karmaşık hâle gelebilir.

### 📊 Tablo 1 – Zero-Shot İstemleme Örneği

| Alan              | Değer |
|-------------------|-------|
| **Ad**            | `1_1_movie_classification ` |
| **Amaç**          | Film yorumlarını olumlu, nötr veya olumsuz olarak sınıflandırmak |
| **Model**         | gemini-pro |
| **Sıcaklık**      | 0.1 |
| **Token Sınırı**  | 5 |
| **Top-K**         | N/A |
| **Top-P**         | 1 |
| **İstem (Prompt)** | Film yorumlarını **OLUMLU**, **NÖTR** veya **OLUMSUZ** olarak sınıflandırın. <br> Yorum: *"Her" filmi, insanlığın kontrolsüz bir şekilde evrim geçirmesine izin verilirse nereye doğru gittiğini ortaya koyan rahatsız edici bir çalışmadır. Böyle başyapıtların daha fazla yapılmasını dilerdim.* <br> Duygu: |
| **Çıktı**         | OLUMLU |

Zero-shot yaklaşımının yeterli olmadığı durumlarda, istem içerisine örnekler veya gösterimler ekleyebilirsiniz. Bu durum sizi, one-shot ve few-shot istemleme tekniklerine yönlendirir.