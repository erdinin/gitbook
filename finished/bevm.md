# BEVM

**BEVM Hakkında**

{% hint style="success" %}
[**Twitter**](https://twitter.com/BTClayer2) **|** [**Github**](https://github.com/btclayer2/BEVM) **|** [**Website** ](https://www.bevm.io/)**|** [**Discord**](https://discord.com/invite/AGcRqkTBAV) **|** [**Docs**](https://documents.bevm.io/) **|** [**Explorer**](https://telemetry.chainx.org/#list/0x6ac13efb5b368b97b4934cef6edfdd99c2af51ba5109bfb8dacc116f9c584c10)
{% endhint %}

{% hint style="info" %}
**BEVM**, BTC'yi Gaz olarak kullanan, tamamen merkezi olmayan EVM uyumlu ilk Bitcoin L2'dir. Ethereum ekosisteminde çalışabilen tüm DApp'lerin Bitcoin L2 üzerinde çalışmasına olanak tanır.
{% endhint %}

***

### Kurulum

```
sudo apt update && sudo apt upgrade
```

```
sudo apt install curl
```

```
curl -fsSL https://get.docker.com -o get-docker.sh
```

```
sudo sh get-docker.sh
```

```
cd /var/lib
```

```
mkdir node_bevm_test_storage
```

```
sudo docker pull btclayer2/bevm:v0.1.1
```

**`node-adınız` yazan kısımı değiştiriniz**

```
sudo docker run -d -v /var/lib/node_bevm_test_storage:/root/.local/share/bevm btclayer2/bevm:v0.1.1 bevm "--chain=testnet" "--name=node-adiniz" "--pruning=archive" --telemetry-url "wss://telemetry.bevm.io/submit 0"
```

[https://telemetry.bevm.io/](https://telemetry.bevm.io/) adresine giderek node-adınız kısımına yazdığımız isimi yazınız ve bir kere üstüne bastığınızda her zaman üstte kalacaktır.&#x20;
