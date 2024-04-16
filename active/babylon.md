# Babylon

#### **Lava Network Hakkında**

{% hint style="success" %}
[**Twitter**](https://twitter.com/babylon\_chain) **|** [**Github** ](https://github.com/babylonchain)**|** [**Website** ](https://babylonchain.io/)**|** [**Discord**](https://discord.com/invite/babylonchain) **|** [**Docs**](https://docs.babylonchain.io/) **|** [**Explorer**](https://testnet.babylon.explorers.guru/)
{% endhint %}

{% hint style="info" %}
**Babylon**, Cosmos zone'larının ve diğer PoS zincirlerinin güvenliğini artırmak için Bitcoin'in güvenliğinden yararlanma vizyonuna sahip yeni bir Cosmos projesidir.
{% endhint %}

***

<details>

<summary>Kurulum Rehberi</summary>

#### Sistem Gereksinimleri

```
CPU	  8 core
RAM	  16GB
Storage	  500GB
OS	  Ubuntu 22.04
```

```bash
# Gerekli kütüphaneler ve güncellemeler
sudo apt update
sudo apt install -y curl git jq lz4 build-essential

# Go
sudo rm -rf /usr/local/go
curl -L https://go.dev/dl/go1.21.6.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile
source .bash_profile
```

```bash
# Repoyu klonlama
cd && rm -rf babylon
git clone https://github.com/babylonchain/babylon
cd babylon
git checkout v0.8.4

# Binaryleri build etme
make install

# CLI konfigrasyonları
babylond config set client chain-id bbn-test-3
babylond config set client keyring-backend test
babylond config set client node tcp://localhost:20657

# moniker-adiniz kısmını değiştiriniz.
babylond init "moniker-adiniz" --chain-id bbn-test-3

# genesis ve addrbook dosyaları
curl -L https://snapshots-testnet.nodejumper.io/babylon-testnet/genesis.json > $HOME/.babylond/config/genesis.json
curl -L https://snapshots-testnet.nodejumper.io/babylon-testnet/addrbook.json > $HOME/.babylond/config/addrbook.json

# seeds
sed -i -e 's|^seeds *=.*|seeds = "8da45f9ff83b4f8dd45bbcb4f850999637fbfe3b@seed0.testnet.babylonchain.io:26656,4b1f8a774220ba1073a4e9f4881de218b8a49c99@seed1.testnet.babylonchain.io:26656,9cb1974618ddd541c9a4f4562b842b96ffaf1446@3.16.63.237:26656,03ce5e1b5be3c9a81517d415f65378943996c864@18.207.168.204:26656,a5fabac19c732bf7d814cf22e7ffc23113dc9606@34.238.169.221:26656,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@testnet-seeds.polkachu.com:20656,798836777efb5555cfb940129e2073b44f9117e5@141.94.143.203:55706,86e9a68f0fd82d6d711aa20cc2083c836fb8c083@222.106.187.14:56000,326fee158e9e24a208e53f6703c076e1465e739d@babylon-testnet.cosmos-spaces.zone:26659,5e02bb2c9a644afae6109bf2c264d356fad27618@15.165.166.210:26656"|' $HOME/.babylond/config/config.toml

# minimum gas price ayarlama
sed -i -e 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.00001ubbn"|' $HOME/.babylond/config/app.toml

# pruning
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "17"|' \
  $HOME/.babylond/config/app.toml

# config
sed -i 's|^network *=.*|network = "signet"|g' $HOME/.babylond/config/app.toml

# portları değiştirme
sed -i -e "s%:1317%:20617%; s%:8080%:20680%; s%:9090%:20690%; s%:9091%:20691%; s%:8545%:20645%; s%:8546%:20646%; s%:6065%:20665%" $HOME/.babylond/config/app.toml
sed -i -e "s%:26658%:20658%; s%:26657%:20657%; s%:6060%:20660%; s%:26656%:20656%; s%:26660%:20661%" $HOME/.babylond/config/config.toml

# snapshot dosyası
curl "https://snapshots-testnet.nodejumper.io/babylon-testnet/babylon-testnet_latest.tar.lz4" | lz4 -dc - | tar -xf - -C "$HOME/.babylond"

# servis dosyası oluşturma
sudo tee /etc/systemd/system/babylond.service > /dev/null << EOF
[Unit]
Description=Babylon node service
After=network-online.target
[Service]
User=$USER
ExecStart=$(which babylond) start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
sudo systemctl daemon-reload
sudo systemctl enable babylond.service

# servisi başlatma ve logları görme
sudo systemctl start babylond.service
sudo journalctl -u babylond.service -f --no-hostname -o cat
```

</details>
