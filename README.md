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

##### Model Eğitimi İçin Kullanılan Kütüphaneler
- torch (GPU kullanımı için)
- pandas (Veri çerçevesi kullanmak için)
- transformers (ön eğitilmiş modelin kullanılması ve farklı bir problem için tekrar eğitilmesi için kullanılan mimariyi eklemek için)
- warnings (gereksiz uyarıları kapatmak için)
- time (süre hesabı ve rastgele sayı üretimi için)
- tqdm (ilerleme çubuğu oluşturmak için)

#### Model Eğitiminde GPU Varsa Kullanmak İçin
- Kullandığınız bilgisayarda modelinizi eğitmek için GPU varsa o GPU'yu kullanmak için
> device = torch.device("cuda:0") if torch.cuda.is_available() else torch.device("cpu") 
- GPU'da yapmak istediğiniz hesaplama / işlem için
> nesneyi .to(device) metodu ile kendine eşitlemeniz gerekmektedir.
> Örneğin, model = model.to(device)

- Daha detaylı kullanım için aşağıdaki torch komutları incelenebilir. 
> if torch.cuda.is_available():

> torch.cuda.current_device()

> torch.cuda.device(0)

> torch.cuda.device_count()

> torch.cuda.get_device_name(0)

### Modelimizi İyileştirdiğimiz Veri Kümemiz
- Mevcut Modelimizi KVKK kapsamında oluşturduğumuz 100 soru ve bu sorulara verilebilecek cevaplar ile eğittik.
- Veri kümemiz model eğitimimizin yanında metin benzerliği ile tespit ettiğimiz cevapların saklandığı (**_KVKK_100_SORU_CEVAP.txt_**) bir metin dosyasıdır.
- Soru numarası, soru ve cevap olmak üzere 3 sütun ve 101 satırdan oluşmaktadır. Satırdaki her bir kayıt alanı sekme ile ayrılmıştır (TSV).

SN | Soru | Cevap
--- | ------------- | -------------
1 | Kişisel Verilerin Korunmasına Neden İhtiyaç Duyulmuştur | Gerek kamu, gerekse özel kurum ve kuruluşlar, bir görevin yerine getirilmesi veya bir hizmetin sunumuyla bağlantılı olarak, kişisel veri niteliğindeki bilgileri, uzun süredir toplamaktadırlar. Bu durum, bazen kanunlardan kaynaklanmakta bazen kişilerin rızasına veya bir sözleşmeye dayanmakta bazen de yapılan işlemin niteliğine bağlı olarak ortaya çıkmaktadır. Belirtmek gerekir ki, kişilerin temel hak ve hürriyetlerinin veri işleme sürecinde de korunması öncelikli konulardan biridir.\nAyrıca, sosyal ve ekonomik hayatın düzen içinde sürdürülmesi, kamu hizmetlerinin etkin biçimde sunumu, mal ve hizmetlerin ekonominin gereklerine uygun biçimde geliştirilmesi, dağıtımı ve pazarlanması için kişisel verilerin toplanması kaçınılmaz olmakla birlikte, kişisel verilerin sınırsız ve gelişigüzel toplanmasının, yetkisiz kişilerin erişimine açılmasının, ifşası, amaç dışı ya da kötüye kullanımı sonucu kişisel hakların ihlal edilmesinin önüne geçilmesi gereklidir.\nBunun yanı sıra, Avrupa Konseyi tarafından, tüm üye ülkelerde kişisel verilerin aynı standartlarda korunması ve sınır ötesi veri akışı ilkelerinin belirlenmesi amacıyla hazırlanan “Kişisel Verilerin Otomatik İşleme Tabi Tutulması Karşısında Bireylerin Korunmasına İlişkin 108 Sayılı Sözleşme”, 28 Ocak 1981 tarihinde imzaya açılmış ve ülkemiz tarafından da imzalanmıştır. Bu sözleşme 17 Mart 2016 tarih ve 29656 sayılı Resmî Gazetede yayımlanarak iç hukuka dâhil edilmiştir. 108 sayılı Sözleşmenin 4. maddesi çerçevesinde,9 iç hukukta kişisel verilerin korunmasına yönelik yasal düzenleme yapılması gerekli hale gelmiştir.\nNitekim, Anayasa Mahkemesinin 9 Nisan 2014 tarih ve E:2013/122, K:2014/74 sayılı kararında da; “Kişisel verilerin korunması hakkı, kişinin insan onurunun korunmasının ve kişiliğini serbestçe geliştirebilmesi hakkının özel bir biçimi olarak, bireyin hak ve özgürlüklerini kişisel verilerin işlenmesi sırasında korumayı […]” amaçladığı tespit edilerek, “kişisel verilerin ticari işletmeler için kıymetli bir varlık niteliği kazanması neticesinde, özel sektör unsurlarınca yaratılan risklerin daha yaygın ve önemli boyutlara ulaşması ve terör ve suç örgütlerinin kişisel verileri ele geçirme yönündeki faaliyetlerinin artması gibi etkenler” sebebiyle kişisel verilerin geçmişte olduğundan çok daha fazla korunmaya muhtaç olduğu ifade edilmiştir.
2 | K | C
3 | K | C


### Sohbet Yazılımımız
- Kullanıcının sorularını tahmin modülüne ileten ve tahmin modülünden aldığı cevabı kullanıcıya döndüren ve kullanıcının gördüğü ara yüzü oluşturan bir PYTHON dosyamız mevcuttur.
