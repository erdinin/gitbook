# Pryzm

#### Pryzm Hakkında

> [Twitter ](https://twitter.com/pryzm\_zone)| [Github ](https://github.com/pryzm-finance)| [Website ](https://pryzm.zone/)| [Discord ](https://discord.gg/pryzm-869587037286715422)| [Telegram ](https://t.me/+uB3fAFC56KIzZDVk)| [Medium ](https://pryzm.medium.com/)| [Docs ](https://docs.pryzm.zone/)| [Explorer](https://testnet.itrocket.net/pryzm)

***

### Kurulum

#### Minimum Sistem Gereksinimleri

```
2CPU 4RAM 100GB
```

#### Önerilen Sistem Gereksinimleri

```
4CPU 8RAM 200GB
```

#### Dependencies & Gereklilikler

```
sudo apt update
```

```
sudo apt install -y curl git jq lz4 build-essential
```

**Go'yu yüklüyoruz.**

```
sudo rm -rf /usr/local/go
curl -L https://go.dev/dl/go1.21.6.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
echo 'export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin' >> $HOME/.bash_profile
source .bash_profile
```

**Node'u yüklüyoruz.**

```
cd $HOME
curl -s https://storage.googleapis.com/pryzm-zone/core/0.11.1/pryzmd-0.11.1-linux-amd64 > pryzmd
chmod +x pryzmd
mkdir -p $HOME/go/bin
mv pryzmd $HOME/go/bin
```

```
pryzmd config chain-id indigo-1
pryzmd config keyring-backend test
pryzmd config node tcp://localhost:24857
```

**"`node-adınız`" yazan kısımı değiştiriniz.**

```
pryzmd init "node-adınız" --chain-id indigo-1
```

```
curl -L https://snapshots-testnet.nodejumper.io/pryzm-testnet/genesis.json > $HOME/.pryzm/config/genesis.json
curl -L https://snapshots-testnet.nodejumper.io/pryzm-testnet/addrbook.json > $HOME/.pryzm/config/addrbook.json
```

```
sed -i -e 's|^seeds *=.*|seeds = "ff17ca4f46230306412ff5c0f5e85439ee5136f0@testnet-seed.pryzm.zone:26656,fbfd48af73cd1f6de7f9102a0086ac63f46fb911@pryzm-testnet-seed.itrocket.net:41656,cdcd86ca01858275d0e78ee66b82109ee06df454@65.108.72.253:40656,cddf23604f62d0b7a6b0bb19418a9e8625d04f2a@207.244.246.204:41656"|' $HOME/.pryzm/config/config.toml
```

```
sed -i -e 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.015upryzm,0.01factory/pryzm15k9s9p0ar0cx27nayrgk6vmhyec3lj7vkry7rx/uusdsim,0.001ibc/27394FB092D2ECCD56123C74F36E4C1F926001CEADA9CA97EA622B25F41E5EB2,0.001ibc/265435C653FE85CD659E88CD51D4A735BDD4D3804871400378A488C71D68C72B,0.001ibc/92E0120F15D037353CFB73C14651FC8930ADC05B93100FD7754D3A689E53B333,0.001ibc/1704820C9E1F4A9925E0F23D3B92ED0E53DEE28726257E39FABD444BFC6B6AE3"|' $HOME/.pryzm/config/app.toml
```

**tek komut**

```
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "17"|' \
  $HOME/.pryzm/config/app.toml
```

**tek komut**

```
sed -i -e "s%:1317%:24817%; s%:8080%:24880%; s%:9090%:24890%; s%:9091%:24891%; s%:8545%:24845%; s%:8546%:24846%; s%:6065%:24865%" $HOME/.pryzm/config/app.toml
sed -i -e "s%:26658%:24858%; s%:26657%:24857%; s%:6060%:24860%; s%:26656%:24856%; s%:26660%:24861%" $HOME/.pryzm/config/config.toml
```

```
curl "https://snapshots-testnet.nodejumper.io/pryzm-testnet/pryzm-testnet_latest.tar.lz4" | lz4 -dc - | tar -xf - -C "$HOME/.pryzm"
```

**servis dosyası oluşturuyoruz**

```
sudo tee /etc/systemd/system/pryzmd.service > /dev/null << EOF
[Unit]
Description=Pryzm Testnet node service
After=network-online.target
[Service]
User=$USER
ExecStart=$(which pryzmd) start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

```
sudo systemctl daemon-reload
sudo systemctl enable pryzmd.service
```

```bash
sudo systemctl start pryzmd.service
```

```bash
apt install screen
```

```bash
screen -S pryzm
```

```bash
sudo journalctl -u pryzmd.service -f --no-hostname -o cat
```

CTRL + A + D tuşları ile screenden çıkıyoruz.

**Cüzdan ve validatör oluşturma işlemleri**

`cüzdan-adınız` yazan kısımı değiştiriniz. verdiği kelimeleri saklayınız.

```bash
pryzmd keys add cüzdan-adınız
```

[https://testnet.pryzm.zone/faucet](https://testnet.pryzm.zone/faucet) siteye gidip yukarıda oluşturduğumuz cüzdana token alınız. biraz bekledikten sonra bakiyemizi kontrol etmek için aşağıdaki komutu giriniz. `cüzdan-adınız`değiştirin.

```bash
pryzmd q bank balances $(pryzmd keys show cüzdan-adınız -a)
```

şimdi validatörümüzü oluşturacağız. aşağıdaki başta `validator-adınız` olmak üzere dilediğiniz yerleri size uygun bir şekilde değiştiriniz.

```bash
pryzmd tx staking create-validator \
--amount=1000000upryzm \
--pubkey=$(pryzmd tendermint show-validator) \
--moniker="validator-adınız" \
--identity=F287570B99E59F81 \
--details="https://xyznodes.xyz" \
--chain-id=indigo-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=cüzdan-adınız \
--gas-prices=0.1upryzm \
--gas-adjustment=1.5 \
--gas=auto \
-y
```

[https://testnet.itrocket.net/pryzm/staking](https://testnet.itrocket.net/pryzm/staking) buradan kendimizi kontrol edebiliriz. faucetten aldığımız token aktif sete girmemiz için yeterli olmadığından kendimizi inactive kısımında arayacağız.
