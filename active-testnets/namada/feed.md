# 📌 feed

<details>

<summary>Namada Shielded Expedition</summary>

**23.02.2024 Güncelleme - v0.31.6**

```
##priv_validator_state.json yedekle.
##validatörü durdur
##chain klasörünü sil
##v0.31.6 ya güncelle
##join-network yapıp chain dosyasını indir
##validator_state.json ı yerine koy.
##chain-id/config.toml daki timeout_precommit_delta = "0ms" (used to be "500ms")
##node'u başlat ve sync ol.
```

```
sudo apt update && sudo apt upgrade -y
```

```
sed -i '/NAMADA_TAG/d' "$HOME/.bash_profile"
NEWTAG=v0.31.6
echo "export NAMADA_TAG=$NEWTAG" >> ~/.bash_profile
source ~/.bash_profile
```

```
cd $HOME/namada
git reset --hard HEAD
git fetch
git checkout $NAMADA_TAG
make build-release
```

```
systemctl stop namadad
rm /usr/local/bin/namada /usr/local/bin/namadac /usr/local/bin/namadan /usr/local/bin/namadaw /usr/local/bin/namadar -rf
```

```
cd $HOME && cp "$HOME/namada/target/release/namada" /usr/local/bin/namada && \
cp "$HOME/namada/target/release/namadac" /usr/local/bin/namadac && \
cp "$HOME/namada/target/release/namadan" /usr/local/bin/namadan && \
cp "$HOME/namada/target/release/namadaw" /usr/local/bin/namadaw && \
cp "$HOME/namada/target/release/namadar" /usr/local/bin/namadar
```

```
namada --version
##Namada v0.31.6
```

```
sudo systemctl restart namadad && sudo journalctl -u namadad -f -o cat
```

***

\
**shielded-expedition.88f17d1d14**

**güncellemeler ve gereklilikler**&#x20;

```
cd $HOME
```

```
sudo apt update && sudo apt upgrade -y
```

```
sudo apt install curl tar wget clang pkg-config git make libssl-dev libclang-dev libclang-12-dev -y
```

```
sudo apt install jq build-essential bsdmainutils ncdu gcc git-core chrony liblz4-tool -y
```

```
sudo apt install original-awk uidmap dbus-user-session protobuf-compiler unzip -y
```

```
sudo apt install libudev-dev
```

```
cd $HOME
```

```
sudo apt update
```

```
sudo curl https://sh.rustup.rs -sSf | sh -s -- -y
```

```
. $HOME/.cargo/env
```

```
curl https://deb.nodesource.com/setup_18.x | sudo bash
```

```
sudo apt install cargo nodejs -y < "/dev/null"
```

```
cargo --version
```

```
node -v
```

```
if ! [ -x "$(command -v go)" ]; then
  ver="1.20.5"
  cd $HOME
  wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
  sudo rm -rf /usr/local/go
  sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
  rm "go$ver.linux-amd64.tar.gz"
  echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
  source ~/.bash_profile
fi
```

```
go version
```

```
cd $HOME && rustup update
```

```
PROTOC_ZIP=protoc-23.3-linux-x86_64.zip
```

```
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v23.3/$PROTOC_ZIP
```

```
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
```

```
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'
```

```
rm -f $PROTOC_ZIP
```

```
protoc --version
```

```
sed -i '/public-testnet/d' "$HOME/.bash_profile"
sed -i '/NAMADA_TAG/d' "$HOME/.bash_profile"
sed -i '/WALLET_ADDRESS/d' "$HOME/.bash_profile"
sed -i '/CBFT/d' "$HOME/.bash_profile"
```

```
echo "export NAMADA_TAG=v0.31.5" >> ~/.bash_profile
echo "export CBFT=v0.37.2" >> ~/.bash_profile
echo "export NAMADA_CHAIN_ID=shielded-expedition.88f17d1d14" >> ~/.bash_profile
echo "export KEY_ALIAS=wallet" >> ~/.bash_profile
echo "export BASE_DIR=$HOME/.local/share/namada" >> ~/.bash_profile
```

**moniker adınız ve mail adresiniz kısımlarını değiştiriniz.**

```
echo "export VALIDATOR_ALIAS=moniker adınız" >> ~/.bash_profile
echo "export EMAIL=mail adresiniz" >> ~/.bash_profile
```

```
source ~/.bash_profile
```

```
cd $HOME && git clone https://github.com/anoma/namada && cd namada && git checkout $NAMADA_TAG
```

```
make build-release
```

```
cd $HOME && git clone https://github.com/cometbft/cometbft.git && cd cometbft && git checkout $CBFT
```

```
make build
```

```
cd $HOME && cp $HOME/cometbft/build/cometbft /usr/local/bin/cometbft && \
cp "$HOME/namada/target/release/namada" /usr/local/bin/namada && \
cp "$HOME/namada/target/release/namadac" /usr/local/bin/namadac && \
cp "$HOME/namada/target/release/namadan" /usr/local/bin/namadan && \
cp "$HOME/namada/target/release/namadaw" /usr/local/bin/namadaw && \
cp "$HOME/namada/target/release/namadar" /usr/local/bin/namadar
```

```
cometbft version
namada --version 
```

```
#v0.31.0 çıktısı almamız gerekiyor
```

```
sudo tee /etc/systemd/system/namadad.service > /dev/null <<EOF
[Unit]
Description=namada
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.local/share/namada
Environment=TM_LOG_LEVEL=p2p:none,pex:error
Environment=NAMADA_CMT_STDOUT=true
ExecStart=/usr/local/bin/namada node ledger run 
StandardOutput=syslog
StandardError=syslog
Restart=always
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```

```
sudo systemctl daemon-reload
sudo systemctl enable namadad
```

**sadece pre-genesis validatörler için olan kısım**

```
namada client utils join-network --chain-id $NAMADA_CHAIN_ID --genesis-validator $VALIDATOR_ALIAS
```

```
sudo systemctl restart namadad && sudo journalctl -u namadad -f -o cat
```

**pre-genesis bu kadar**

```
cd $HOME && namada client utils join-network --chain-id $NAMADA_CHAIN_ID
```

```
sudo systemctl start namadad && sudo journalctl -u namadad -f -o cat
```

```
curl -s localhost:26657/status
```

**false çıktısı alana kadar bekleyelim. sonra devam edelim.**

**cüzdanadi yazan kısmı değiştirin. komutu girdikten sonra mnemonic isteyecek balances.toml dosyası içerisindeki cüzdamızın mnemonic keyini girin.**

```
cd $HOME
namadaw derive --alias cüzdanadi
```

validatör oluşturma komutunu girelim.

```
cd $HOME
namada client init-validator \
--alias $VALIDATOR_ALIAS \
--account-keys $KEY_ALIAS \
--signing-keys $KEY_ALIAS \
--commission-rate 0.05 \
--max-commission-rate-change 0.01 \
--email $EMAIL \
--unsafe-dont-encrypt
```

```
namada client bond \
--validator $VALIDATOR_ALIAS \
--amount 20000
```

<pre><code><strong>namada client bonded-stake 
</strong>namada client bonds
namada client slashes
namadac validator-state --validator $VALIDATOR_ALIAS

</code></pre>

```
##### ❗❗❗ NODE'U SİLME ❗❗❗ !! #####

cd $HOME && mkdir $HOME/namada_backup
cp -r $HOME/.local/share/namada/pre-genesis $HOME/namada_backup/
systemctl stop namadad && systemctl disable namadad
rm /etc/systemd/system/namada* -rf
rm $(which namada) -rf
rm /usr/local/bin/namada* /usr/local/bin/cometbft -rf
rm $HOME/.namada* -rf
rm $HOME/.local/share/namada -rf
rm $HOME/namada -rf
rm $HOME/cometbft -rf
```

</details>
