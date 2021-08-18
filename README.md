# Kişisel Verilerin Korunumu Kanunu Soru - Cevap Sistemi
**Türkiye Açık Kaynak Platformu** tarafından **Türkçe Doğal Dil İşleme** konusunda farkındalık oluşturmak amacıyla 2021 yılında **TeknoFest 2021 İSTANBUL** kapsamında çevrimiçi düzenlenen yarışma için geliştirdiğimiz projemiz **Kişisel Verilerin Korunumu Kanunu (KVKK)** hakkindaki **sorulara cevap veren** otonom bir sohbet yazılımı / çağrı merkezidir.

## Soru Cevap Sisteminin Parçaları
### Model Eğitimi
- Projede kullandığımız ana model _(sentence-transformers/paraphrase-xlm-r-multilingual-v1)_ cümle benzerliğinin bulunmasında kullanılan bir modeldir.
- Ana model olarak kullandığımız RoBERTa modeli A Robustly Optimized BERT Pretraining Approach olarak anılır ve Yinhan Liu, Myle Ott, Naman Goyal, Jingfei Du, Mandar Joshi, Danqi Chen, Omer Levy, Mike Lewis, Luke Zettlemoyer ve Veselin Stoyanov tarafından Google’ın 2018 BERT modeli referans alınarak yapılmıştır.
- Kullandığımız ana modele ve API uygulamasına https://huggingface.co/sentence-transformers/paraphrase-xlm-r-multilingual-v1 İnternet sayfasından ulaşabilirsiniz.
- Mevcut Modelimizi kendi veri kümeniz ile eğitebilmeniz için hazırladığımız bir PYTHON dosyamız mevcuttur. __*/ModelEgitimiYeniYontem.py*__ isimli dosyamız TSV (Tab-Separated Values) içerikli metin dosyasından ikişer satır okuyup modele eğitim için göndermektedir. Mevcut veri kümenizi saklama biçiminize göre __*def sorulariDosyadanOkuveListeOlarakDondur(son):*__ isimli fonksiyonu değiştirmeniz gerekir.

![Model Eğitilirken](/ModelEgitimiOrnek01.png)

![Model Eğitilmiş Hali](/ModelEgitimiOrnek02.png)

### Modelimizi İyileştirdiğimiz Veri Kümemiz
- Mevcut Modelimizi KVKK kapsamında oluşturduğumuz 100 soru ve bu sorulara verilebilecek cevaplar ile eğittik.
- Veri kümemiz model eğitimimizin yanında metin benzerliği ile tespit ettiğimiz cevapların saklandığı (**_KVKK_100_SORU_CEVAP.txt_**) bir metin dosyasıdır.
- Soru numarası, soru ve cevap olmak üzere 3 sütun ve 101 satırdan oluşmaktadır. Satırdaki her bir kayıt alanı sekme ile ayrılmıştır (TSV).

### Sohbet Yazılımımız
- Kullanıcının sorularını tahmin modülüne ileten ve tahmin modülünden aldığı cevabı kullanıcıya döndüren ve kullanıcının gördüğü ara yüzü oluşturan bir PYTHON dosyamız mevcuttur.
