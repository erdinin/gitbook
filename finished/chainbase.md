---
icon: terminal
---

# Chainbase

#### Chainbase **Hakkında**

{% hint style="success" %}
[**Twitter**](https://x.com/ChainbaseHQ) **|** [**Website**](https://chainbase.com/) **|** [**Discord**](https://discord.com/invite/chainbase) **|** [**Docs**](https://docs.chainbase.com/introduction/about)
{% endhint %}

{% hint style="info" %}
**Chainbase**, yapay zeka için dünyanın en büyük omnichain veri ağı.
{% endhint %}

***

<details>

<summary><strong>Chainbase Genesis - Testnet</strong></summary>

[https://genesis.chainbase.com/referral?referral\_code=ZSAKS4S6T](https://genesis.chainbase.com/referral?referral_code=ZSAKS4S6T)

</details>

<details>

<summary>Chainbase AVS Operator Rehberi</summary>

**Chainbase AVS Kurulumu**

1.  **Chainbase AVS setup deposunu klonlayın:**

    ```bash
    git clone https://github.com/chainbase-labs/chainbase-avs-setup
    ```
2.  **Mainnet’te Chainbase AVS operatör düğümünü çalıştırmak için (davetli ve beyaz listedeyseniz):**

    ```bash
    cd chainbase-avs-setup/mainnet
    ```

    **Testnet’te Chainbase AVS operatör düğümünü çalıştırmak için (herkese açık):**

    ```bash
    cd chainbase-avs-setup/holesky
    ```
3.  **Ortam dosyasını oluşturun:**

    ```bash
    cp .env.example .env
    ```
4.  **`.env` dosyasındaki tüm alanları kendi bilgilerinize göre düzenleyin:**

    ```bash
    NODE_ECDSA_KEY_FILE_PATH=/your/ecdsa/key/path  
    NODE_BLS_KEY_FILE_PATH=/your/bls/key/path  
    OPERATOR_ECDSA_KEY_PASSWORD=yourECDSAKeyPassword  
    OPERATOR_BLS_KEY_PASSWORD=yourBlsKeyPassword  
    OPERATOR_ADDRESS=yourECDSAKeyAddress  
    NODE_SOCKET=yourNodeSocket  
    OPERATOR_NAME=yourOperatorName  
    ```

    ➡️ **ECDSA ve BLS anahtar yollarınızı ve operatör adresinizi öğrenmek için şu komutu çalıştırabilirsiniz:**

    ```bash
    eigenlayer operator keys list
    ```

    * **OPERATOR\_ADDRESS**: Operatör adresinizi belirtin (ECDSA anahtar adresinizle eşleşmeli).
    * **NODE\_SOCKET**: Sunucunuzun genel IP adresini şu formatta belirtin: `<sunucu_genel_ip_adresiniz>:8011`.

    **Önemli:**

    * Sunucunuzun genel IP adresinin internete erişilebilir olduğundan emin olun.
    * 8011 portunun açık ve güvenlik duvarı ayarlarında doğru yapılandırılmış olduğundan emin olun.
5.  **Komut dosyasına çalıştırma izni verin:**

    ```bash
    chmod +x ./chainbase-avs.sh
    ```

***

**Chainbase AVS İşletimi**

1.  **Operatör olarak kaydolun:**

    ```bash
    ./chainbase-avs.sh register
    ```
2.  **Düğümü çalıştırın:**

    ```bash
    ./chainbase-avs.sh run
    ```
3.  **Düğümü test edin:**

    ```bash
    ./chainbase-avs.sh test
    ```

    Eğer komut çıktısında “_All systems are working for your manuscript node_” yazısını görüyorsanız, düğümünüz doğru şekilde çalışıyor demektir.

***

**Düğüm Soketini Güncelleme**

Eğer operatör olarak kaydolduktan sonra sunucunuzun genel IP adresi değişirse:

* `.env` dosyasındaki `NODE_SOCKET` alanını güncelleyin.
*   Şu komutu çalıştırın:

    ```bash
    ./chainbase-avs.sh socket
    ```

***

**Düğüm Sürümünü Güncelleme**

Düğüm sürümünü güncellemek için şu komutları sırasıyla çalıştırın:

```bash
./chainbase-avs.sh stop  
./chainbase-avs.sh update  
./chainbase-avs.sh run  
```

***

**Logları İzleme**

Konteyner loglarını görüntülemek için şu komutlardan birini kullanabilirsiniz:

```bash
docker compose logs -f  
docker compose logs -f <konteyner_adı>  
docker logs -f <konteyner_id>  
```











</details>

