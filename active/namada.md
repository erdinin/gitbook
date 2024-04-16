# Namada

#### **Namada Hakkında**

{% hint style="success" %}
[Twitter](https://twitter.com/namada) | [Github](https://github.com/anoma/namada) | [Website](https://namada.net/) | [Discord](https://discord.gg/namada) | [Telegram](https://t.me/+uB3fAFC56KIzZDVk) | [Docs](https://docs.namada.net/) | [Explorer](https://namada.info/)
{% endhint %}

{% hint style="info" %}
**Namada Anoma Vakfı tarafından geliştirilen IBC uyumlu, zk teknolojisi kullanan Ethereum ile iki yönlü bir köprü ile çalışan asset-agnostic gizlilik sağlayan bir katman 1 blokzincir projesidir.**
{% endhint %}

***

#### Sistem Gereksinimleri

```csharp
CPU	  4 core
RAM	  16GB
Storage	  1TB
OS	  Ubuntu 22.04
```

<details>

<summary>Kurulum Rehberi</summary>



**Paket Güncellemeleri ve Bağımlılklar**

```csharp
sudo apt update && sudo apt upgrade -y
sudo apt-get install -y make git-core libssl-dev pkg-config libclang-12-dev build-essential protobuf-compiler
```

**Go Kurulumu**

```csharp
cd $HOME
! [ -x "$(command -v go)" ] && {
VER="1.20.3"
wget "<https://golang.org/dl/go$VER.linux-amd64.tar.gz>"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
}
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

_**Cüzdan bilgilerinizi ve validatör bilgilerinizi güncelleyin. Değişkenleri export edin. Port ayarlayın.(gerekliyse)**_

```csharp
NAMADA_PORT=26
echo "export NAMADA_PORT="$NAMADA_PORT"" >> $HOME/.bash_profile
echo "export ALIAS="CHOOSE_A_NAME_FOR_YOUR_VALIDATOR"" >> $HOME/.bash_profile
echo "export MEMO="CHOOSE_YOUR_tpknam_ADDRESS"" >> $HOME/.bash_profile
echo "export WALLET="wallet"" >> $HOME/.bash_profile
echo "export PUBLIC_IP=$(wget -qO- [eth0.me](<http://eth0.me/>))" >> $HOME/.bash_profile
echo "export TM_HASH="v0.1.4-abciplus"" >> $HOME/.bash_profile
echo "export CHAIN_ID="shielded-expedition.88f17d1d14"" >> $HOME/.bash_profile
echo "export BASE_DIR="$HOME/.local/share/namada"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

**Rust yükleyin.**

```csharp
curl --proto '=https' --tlsv1.2 -sSf [<https://sh.rustup.rs>](<https://sh.rustup.rs/>) | sh -s -- -y
source $HOME/.cargo/env
```

**CometBFT yükleyin.**

```csharp
cd $HOME
rm -rf cometbft
git clone <https://github.com/cometbft/cometbft.git>
cd cometbft
git checkout v0.37.2
make build
sudo cp $HOME/cometbft/build/cometbft /usr/local/bin/
cometbft version

```

**Namada indirin ve kurun.**

```csharp
cd $HOME
rm -rf namada
git clone <https://github.com/anoma/namada>
cd namada
wget <https://github.com/anoma/namada/releases/download/v0.32.1/namada-v0.32.1-Linux-x86_64.tar.gz>
tar -xvf namada-v0.32.1-Linux-x86_64.tar.gz
rm namada-v0.32.1-Linux-x86_64.tar.gz
cd namada-v0.32.1-Linux-x86_64
sudo mv namad* /usr/local/bin/
if [ ! -d "$BASE_DIR" ]; then
mkdir -p "$BASE_DIR"
fi
```

_**Namada versionu kontrol edin.**_

```csharp
namada --version
```

*   **Pre-Genesis Validatör iseniz;**

    ```csharp
    cd $HOME
    cp -r ~/.namada/pre-genesis $BASE_DIR/
    namada client utils join-network --chain-id $CHAIN_ID --genesis-validator $ALIAS
    ```

**Full Node veya Post-Genesis iseniz;**

```csharp
namada client utils join-network --chain-id $CHAIN_ID
```

**Servis dosyası oluşturma**

```csharp
sudo tee /etc/systemd/system/namadad.service > /dev/null <<EOF
[Unit]
Description=namada
After=network-online.target

[Service]
User=$USER
WorkingDirectory=$BASE_DIR
Environment=TM_LOG_LEVEL=p2p:none,pex:error
Environment=NAMADA_CMT_STDOUT=true
ExecStart=$(which namada) node ledger run
StandardOutput=syslog
StandardError=syslog
Restart=always
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

**Servis dosyasını başlatma ve loglara bakma**

```csharp
sudo systemctl daemon-reload
sudo systemctl enable namadad
sudo systemctl restart namadad && sudo journalctl -u namadad -f
```

*   **☠ Node’u kapatma ve kaldırma ☠**

    ```csharp
    sudo systemctl stop namadad
    sudo systemctl disable namadad
    sudo rm -rf /etc/systemd/system/namadad.service
    sudo systemctl daemon-reload
    sudo rm $(which namada)
    sudo rm -rf $HOME/.local/share/namada/shielded-expedition.88f17d1d14
    ```

</details>
