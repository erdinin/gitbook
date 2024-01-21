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
sudo apt install -y curl git jq lz4 build-essential unzip
```

**Go'yu yüklüyoruz.**

```
bash <(curl -s "https://raw.githubusercontent.com/erdinin/testnet-guides/main/utils/go2.sh")
```

```
source .bash_profile
```

```
cd $HOME
```

```
curl -s https://storage.googleapis.com/pryzm-zone/core/0.11.1/pryzmd-0.11.1-linux-amd64 > pryzmd
```

```
chmod +x pryzmd
```

```
mkdir -p $HOME/go/bin
```

```
mv pryzmd $HOME/go/bin
```

```
pryzmd config chain-id indigo-1
```

```
pryzmd config keyring-backend test
```

**`node-adiniz`** kısmını değiştiriniz

```
pryzmd init "node-adiniz" --chain-id indigo-1
```

```
curl -L https://snapshots-testnet.nodejumper.io/pryzm-testnet/genesis.json > $HOME/.pryzm/config/genesis.json
```

```
curl -L https://snapshots-testnet.nodejumper.io/pryzm-testnet/addrbook.json > $HOME/.pryzm/config/addrbook.json
```

```bash
sed -i \
  -e 's|^seeds *=.*|seeds = "ff17ca4f46230306412ff5c0f5e85439ee5136f0@testnet-seed.pryzm.zone:26656,fbfd48af73cd1f6de7f9102a0086ac63f46fb911@pryzm-testnet-seed.itrocket.net:41656"|' \
  -e 's|^peers *=.*|peers = "cdcd86ca01858275d0e78ee66b82109ee06df454@65.108.72.253:40656,cddf23604f62d0b7a6b0bb19418a9e8625d04f2a@207.244.246.204:41656"|' \
  $HOME/.pryzm/config/config.toml
```

```bash
sed -i -e 's|^minimum-gas-prices *=.*|minimum-gas-prices = "0.015upryzm,0.01factory/pryzm15k9s9p0ar0cx27nayrgk6vmhyec3lj7vkry7rx/uusdsim"|' $HOME/.pryzm/config/app.toml
```

```bash
sed -i \
  -e 's|^pruning *=.*|pruning = "custom"|' \
  -e 's|^pruning-keep-recent *=.*|pruning-keep-recent = "100"|' \
  -e 's|^pruning-interval *=.*|pruning-interval = "17"|' \
  $HOME/.pryzm/config/app.toml
```

```bash
curl "https://snapshots-testnet.nodejumper.io/pryzm-testnet/pryzm-testnet_latest.tar.lz4" | lz4 -dc - | tar -xf - -C "$HOME/.pryzm"
```

```bash
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

```bash
sudo systemctl daemon-reload
```

```bash
sudo systemctl enable pryzmd.service
```

```bash
sudo systemctl start pryzmd.service
```

```bash
sudo journalctl -u pryzmd.service -f --no-hostname -o cat
```

