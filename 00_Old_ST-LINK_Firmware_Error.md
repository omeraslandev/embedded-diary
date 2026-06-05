Şu hatayı alıyordum:

```
Debug Server terminated: Old ST-LINK firmware.

Error running 'myFirstEmbeddedProject'
GDB server process terminated unexpectedly.
```
Ve anladım ki bu hata, CLion ve OpenOCD/GDB sunucusunun, elimdeki ST-LINK programlayıcının içindeki gömülü yazılımı (firmware) **eski ve güvensiz/uyumsuz** bulmasından kaynaklanıyor. STMicroelectronics, yeni IDE ve araç zincirlerinde eski firmware sürümlerine desteği kesebiliyor.

Bu sebeple, **donanımın yazılımını resmi araçla güncellemem gerektiğini** anladım.

# 1. Adım: `Gerekli Bağımlılıkları Kurma`
STM-LINK'in USB üzerinden düzgün haberleşebilmesi için `libusb` kütüphanesine ve arayüzün çalışması için Java Runtime (JRE) paketine ihtiyacımız var. Terminali açtım (`Ctrl + Alt + T`) ve şu komutları sırasıyla çalıştırdım:
```
sudo apt update
sudo apt upgrade
sudo apt install default-jre libusb-1.0-0
```

# 2. Adım: `STM32CubeProgrammer'ı İndirme`
1. Tarayıcıyı açtım ve şu resmi sayfaya gittim: `st.com/stm32cubeprog`
2. Karşıma çıkan **"Get Software"** butonuna bastım ve aşağıdaki indirme tablosuna yönlendirildim.
3. **STM32CubeProg-LIN** (Linux versiyonu) satırının en sağındaki **"Get Software** butonuna tıkladım.
4. ST benden giriş yapmamı istedi, giriş yaptım. Giriş yaptıktan hemen sonra indirme başladı.
   
# 3. Adım: `STM32CubeProgrammer'ı Kurma`
Dosya tarayıcı üzerinden `Downloads` klasörüne geldim, inen sıkıştırılmış dosyayı gördüm.

Sırasıyla şu komutları takip ettim:
```
# Zip dosyasini acmak icin
unzip en.stm32cubeprg-lin*.zip

# Acilan klasorun icine girmek icin
cd SetupSTM32CubeProgrammer-*

# Yukleyiciye calistirma izni vermek icin
chmod +x SetupSTM32CubeProgrammer-*.linux

# Yukleyiciyi baslatmak icin
./SetupSTM32CubeProgrammer-*.linux
```
Sonrasında ekrana grafik arayüzlü kurulum sihirbazı geldi. Kurulum sihirbazının kurulumunu tamamladım.

# 4. Adım: `Firmware Güncellemesini Yapma`
Yolun büyük kısmını atlattık. Şimdi Ubuntu'nun o karta erişebilmesi için gerekli izinleri verip ardından güncelleme arayüzünü açacağız.

#### I. Mikro-Adım: USB Yetkilerini Tanımlayacağız (Kritik Adım!)
Ubuntu varsayılan olarak ST-LINK gibi cihazlara alt seviye erişim izni vermez. Programın kartı görebilmesi için şu kuralları sisteme kopyalamamız gerekiyor. Terminalde şu komutları sırasıyla çalıştırdım:
```
cd /usr/local/STMicroelectronics/STM32Cube/STM32CubeProgrammer/Drivers/rules
sudo cp *.* /etc/udev/rules.d/
sudo udevadm control --reload-rules && sudo udevadm trigger
```
*Not: Eğer ilk komutta `no such file or directory` hatası alıyorsanız STM32CubeProgrammer'ı sadece kendi özel userınıza eklemiş olabilirsiniz. Bu durumda ihtiyacımız olan path'ı bulabilmek için şu komutu çalıştırın: `find ~ /usr/local -type d -name "rules" 2>/dev/null | grep -i stm32`*

#### II. Mikro-Adım: STM32CubeProgrammer'ı Başlatıyoruz
Şimdi STM32 kartımızı USB kablosuyla bilgisayara bağlıyoruz ve programı şu komutla çalıştırıyoruz:
```
/usr/local/STMicroelectronics/STM32Cube/STM32CubeProgrammer/bin/STM32CubeProgrammer
```

#### III. Mikro-Adım: Firmware Güncellemesini Yapıyoruz
Karşımıza grafiksel bir arayüz açılacak. Şu adımları dikkatlice takip edelim:
1. Açılan programın sağ üst köşesinde mavi renkli bir **"Firmware Upgrade"** butonu göreceğiz. Ona tıklamamız lazım.
2. Ekrana ufak bir pencere gelecek. Bu pencerede sağ taraftaki **"Device Connect** butonuna basacağız. (Kart bağlı ve udev kuralları doğruysa, program kartı tanıyacak ve mevcut eski firmware sürümünü ekranda gösterecektir.)
3. Kart bağlandıktan sonra alt kısımda aktifleşen **"Upgrade"** butonuna tıklıyoruz.
4. Ekranda bir yükleme barı çıkacak ve birkaç saniye içinde *"Firmware upgrade success"* uyarısını göreceğiz.