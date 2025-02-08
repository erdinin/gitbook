---
icon: terminal
---

# Union

#### Union **Hakkında**

{% hint style="success" %}
[**Twitter**](https://x.com/Gaianet_AI) **|** [**Website**](https://www.gaianet.ai/) **|** [**Discord**](https://discord.gg/pQrcbFx76N) **|** [**Docs**](https://docs.gaianet.ai/intro/)
{% endhint %}

{% hint style="info" %}
**Gaia**, herkesin kendi tarzlarını, değerlerini, bilgilerini ve uzmanlıklarını yansıtan yapay zeka ajanlarını oluşturmasını, dağıtmasını, ölçeklendirmesini ve bunlardan gelir elde etmesini sağlayan merkeziyetsiz bir bilişim altyapısıdır.
{% endhint %}

***
Union Ceremony
Sunucu Hazırlığı ve Masaüstü Ortamı Kurulumu
{% hint style="info" %} Bu işlem, sunucunuzda bir masaüstü ortamı (XFCE) oluşturup, uzaktan erişim (RDP) ile bağlantı kurmanıza olanak tanır. {% endhint %}

Güncelleme ve XFCE Kurulumu
Öncelikle sisteminizi güncelleyin:

bash
Kopyala
Düzenle
sudo apt update && sudo apt upgrade -y
XFCE masaüstü ortamının mevcut olup olmadığını kontrol edin:

bash
Kopyala
Düzenle
apt-cache search xfce4
XRDP Kurulumu (Uzak Masaüstü Bağlantısı)
XRDP, uzak masaüstü bağlantısı yapabilmek için gereklidir. Aşağıdaki komutları sırasıyla çalıştırarak kurulumu tamamlayın:

bash
Kopyala
Düzenle
sudo apt install xrdp -y
sudo systemctl enable xrdp
sudo systemctl start xrdp
echo "xfce4-session" >~/.xsession
sudo ufw allow 3389/tcp
Tarayıcı ve Terminal Araçlarının Kurulumu
Bu aşamada, Firefox tarayıcısını ve diğer gerekli araçları kuruyoruz.

bash
Kopyala
Düzenle
sudo apt install firefox -y
sudo apt install screen
screen -S union
sudo apt install curl iptables build-essential git wget jq make gcc nano automake autoconf tmux htop pkg-config libssl-dev tar clang unzip -y
Docker’ı yükleyin ve çalıştığını doğrulayın:

bash
Kopyala
Düzenle
sudo apt update
sudo apt install docker.io -y
docker --version
{% hint style="success" %} 📌 Bu aşamadan sonra terminali açık bırakın ve kendi bilgisayarınıza geçin.
Bir sonraki adımda, MobaXterm ile uzak masaüstü bağlantısı kuracağız. {% endhint %}

MobaXterm ile Uzak Bağlantı (RDP)
💻 Kendi bilgisayarınızdan sunucunuza bağlanmak için aşağıdaki adımları takip edin:

1️⃣ MobaXterm’i indirin:
🔗 İndirme Linki
2️⃣ Kurulumu tamamlayın ve çalıştırın.
3️⃣ Sol üst köşeden Session butonuna tıklayın.
4️⃣ Açılan pencerede "RDP" seçeneğini seçin.

Bağlantı Ayarları:
Alan	Değer
Remote Host	Sunucunun IP adresi
Username	root
Port	3389
5️⃣ "OK" butonuna basarak bağlanın.

📸 Referans Görsel:

Union Ceremony Kurulumu
Artık sunucu üzerinden tarayıcıyı açarak kurulumu tamamlayabilirsiniz.

1️⃣ Application Finder'ı açın ve Firefox'u başlatın.
2️⃣ Aşağıdaki linke gidin:
🔗 Union Ceremony Sitesi
3️⃣ Google veya GitHub ile giriş yapın.
4️⃣ Linux seçeneğini seçip "Copy Command" butonuna basın.
5️⃣ Terminale geri dönerek kopyalanan kodu yapıştırın ve çalıştırın.

{% hint style="info" %} Kurulum tamamlandıktan sonra CTRL + A + D tuşlarına basarak screen oturumundan çıkabilirsiniz. {% endhint %}

Son Adımlar ve Bekleme Süreci
1️⃣ MobaXterm'e geri dönün ve "Address" ile "Generate Key" kısımlarını tamamlayın.
2️⃣ Generate Key işlemi tamamlandıktan sonra, Mozilla-Downloads klasöründen anahtar dosyanızı bulun.
3️⃣ Her şey doğru yapıldıysa, sıra numarasıyla katılımınız tamamlanmış olacak. 🎉

📌 Bu rehber, Union Ceremony sürecini tamamlamanıza yardımcı olmak için hazırlanmıştır. Herhangi bir hata ile karşılaşırsanız, komutları kontrol edin ve adımları tekrar gözden geçirin. 🚀

Ek Notlar
Node çalışmasını ve sıraya girildiğini kontrol etmek için "node-info" sekmesine göz atabilirsiniz.
Kurulum sırasında hata alırsanız, log dosyalarını inceleyerek sorunları tespit edebilirsiniz.
Daha fazla bilgi için resmi dökümantasyonu ziyaret edin: Union Docs
