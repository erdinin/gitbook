# BEVM

#### Pryzm Hakkında

> [Twitter ](https://twitter.com/BTClayer2)| [Github ](https://github.com/btclayer2/BEVM)| [Website](https://www.bevm.io/) | [Discord ](https://discord.gg/csuxN8mMEh)| [Blog ](https://bevm-blog.webflow.io/)| [Docs](https://documents.bevm.io/) | [Explorer](https://telemetry.bevm.io/)

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
