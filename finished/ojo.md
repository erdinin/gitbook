# ojo

#### Ojo Hakkında

> [Twitter](https://twitter.com/ojo\_network) | [Github](https://github.com/ojo-network) | [Website](https://ojo.network/) | [Telegram](https://t.me/OjoNetwork) | [Discord](https://discord.gg/c8SaeRa3uZ) | [Explorer](https://ojo.explorers.guru/) | [xyznodes-Validator](https://ojo.explorers.guru/validator/ojovaloper1x6qr9v8ahe4an4u32g3qylnv4ez584r84tq4lk)

***

### Kurulum

#### Minimum Sistem Gereksinimleri

```
4CPU 8RAM 100GB
```

#### Önerilen Sistem Gereksinimleri

```
4CPU 16RAM 200GB
```

[**NodeJumper**](https://nodejumper.io/ojo-testnet/installation) **kurulum rehberi referans alınarak hazırlanmıştır.**

#### Otomatik Kurulum

```
source <(curl -s https://raw.githubusercontent.com/nodejumper-org/cosmos-scripts/master/ojo/ojo-devnet/install.sh)
```

#### Validator Oluşturma

***

_**Cüzdan Oluşturma**_

```
ojod keys add cüzdan-adı
```

_**Örnek Çıktı**_**:**

```
- name: wallet
    type: local
    address: ojo1ela8c0jhqgjsj2cq7twu9uhda2n8e6cs8ztxs3
    pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Auq9WzVEs5pCoZgr2WctjI7fU+lJCH0I3r6GC1oa0tc0"}'
    mnemonic: ""

#!!! seed phrase kısmını mutlaka kayıt edin.
kite upset hip dirt pet winter thunder slice parent flag sand express suffer chest custom pencil mother bargain remember patient other curve cancel sweet
```

**priv\_validator\_key.json dosyasını kayıt edin**

```
cat $HOME/.ojo/config/priv_validator_key.json
```

**false çıktısı alana kadar bekleyin.**

```
ojod status 2>&1 | jq .SyncInfo.catching_up
```

[**Discord**](https://discord.gg/c8SaeRa3uZ) **token isteyin.**

```
CALISTA#1429 yardımcı olacaktır.
```

**token aldıktan sonra cüzdanımızı kontrol edelim.**

```
ojod q bank balances $(ojod keys show wallet -a)
```

**örnek çıktı:**

```
  balances:
  - amount: "10000000"
    denom: uojo
```

**validator oluşturma**

`MONIKER ADI GIRINIZ` `cüzdan-adı` `website` ve `details` kısımlarını istediğiniz gibi doldurunuz.

```
ojod tx staking create-validator \
--amount=9000000uojo \
--pubkey=$(ojod tendermint show-validator) \
--moniker="MONIKER ADI GIRINIZ" \
--website="https://xyznodes.xyz" \
--details="node runner 💨 PoS Validator ⚛️ testnet addict 💻" \
--chain-id=ojo-devnet \
--commission-rate=0.1 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--min-self-delegation=1 \
--fees=2000uojo \
--from=cüzdan-adı \
-y
```

#### validator detaylarınızı aşağıdaki komutla görebilirsiniz.

```
ojod q staking validator $(ojod keys show wallet --bech val -a)
```

#### pricefeeder yükleme

```
cd || return
rm -rf price-feeder
git clone https://github.com/ojo-network/price-feeder
cd price-feeder || return
git checkout v0.1.1
make install
price-feeder version 
```

```
# örnek çıktı: HEAD-5d46ed438d33d7904c0d947ebc6a3dd48ce0de59
```

```
mkdir -p $HOME/.price-feeder
```

```
curl -s https://raw.githubusercontent.com/ojo-network/price-feeder/main/price-feeder.example.toml > $HOME/.price-feeder/price-feeder.toml
```

```
ojod keys add feeder-wallet --keyring-backend os
```

**Bu kısımda verdiği cüzdan adresini aşağıda `FEEDER_ADRESİNİZ` kısımı ile değiştirin.**

```
ojod tx bank send wallet FEEDER_ADRESİNİZ 10000000uojo --from wallet --chain-id ojo-devnet --fees 2000uojo -y
```

```
ojod q bank balances $(ojod keys show feeder-wallet --keyring-backend os -a)
```

**Değişkenleri tanımlayalım. Tek tek giriniz.**

```
CHAIN_ID=ojo-devnet
KEYRING_PASSWORD=YOUR_KEYRING_PASSWORD
WALLET_ADDRESS=$(ojod keys show wallet -a)
FEEDER_ADDRESS=$(ojod keys show feeder-wallet --keyring-backend os -a)
VALIDATOR_ADDRESS=$(ojod keys show wallet --bech val -a)
GRPC="localhost:9090"
RPC="http://localhost:26657"
```

```
ojod tx oracle delegate-feed-consent $WALLET_ADDRESS $FEEDER_ADDRESS --from wallet --fees 2000uojo -y
```

```
ojod q oracle feeder-delegation $VALIDATOR_ADDRESS
```

**Tek tek giriniz.**

```
sed -i '/^dir *=.*/a pass = ""' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^address *=.*|address = "'$FEEDER_ADDRESS'"|g' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^chain_id *=.*|chain_id = "'$CHAIN_ID'"|g' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^validator *=.*|validator = "'$VALIDATOR_ADDRESS'"|g' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^backend *=.*|backend = "os"|g' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^dir *=.*|dir = "'$HOME/.ojo'"|g' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^pass *=.*|pass = "'$KEYRING_PASSWORD'"|g' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^grpc_endpoint *=.*|grpc_endpoint = "'$GRPC'"|g' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^tmrpc_endpoint *=.*|tmrpc_endpoint = "'$RPC'"|g' $HOME/.price-feeder/price-feeder.toml
sed -i 's|^global-labels *=.*|global-labels = [["chain_id", "'$CHAIN_ID'"]]|g' $HOME/.price-feeder/price-feeder.toml
```

**Servis dosyası oluşturuyoruz.**

```
sudo tee /etc/systemd/system/price-feeder.service > /dev/null << EOF
[Unit]
Description=Ojo Price Feeder
After=network-online.target
[Service]
User=$USER
ExecStart=$(which price-feeder) $HOME/.price-feeder/price-feeder.toml --log-level debug
Restart=on-failure
RestartSec=10
LimitNOFILE=10000
Environment="PRICE_FEEDER_PASS=$KEYRING_PASSWORD"
[Install]
WantedBy=multi-user.target
EOF
```

```
sudo systemctl daemon-reload
```

```
sudo systemctl enable price-feeder
```

sudo systemctl start price-feeder

```
sudo journalctl -u price-feeder -f --no-hostname -o cat
```

**Kurulum tamamlandı. https://ojo.explorers.guru/validators üzerinde kendi adımızı aratarak bulabilirsiniz.**

#### Kullanışlı Komutlar

#### Cüzdan/Key

***

**yeni cüzdan ekleme**

```
ojod keys add cüzdan-adı
```

**mevcut cüzdanı kurtarma**

```
ojod keys add cüzdan-adı --recover
```

**cüzdanları listeleme**

```
ojod keys list
```

**cüzdan silme**

```
ojod keys delete cüzdan-adı
```

**cüzdan dışa aktarma (cüzdan-adi.backup şeklinde kayıt edin)**

```
ojod keys export cüzdan-adı
```

**cüzdan içe aktarma**

```
ojod keys import cüzdan-adı cüzdan-adı.backup
```

**cüzdan bakiye öğrenme**

```
ojod q bank balances $(ojod keys show cüzdan-adı -a)
```

#### Validator/Moniker

***

**yeni validator oluşturma (gerekli yerler doldurunuz.)**

```
ojod tx staking create-validator \
--amount=9000000uojo \
--pubkey=$(ojod tendermint show-validator) \
--moniker="validator-adı" \
--identity=F287570B99E59F81 (keybase) \
--details="bir şeyler yazın" \
--website="varsa web siteniz" \
--chain-id=ojo-devnet \
--commission-rate=0.1 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--min-self-delegation=1 \
--fees=2000uojo \
--from=cüzdan-adı \
-y
```

**validator düzenleme**

```
ojod tx staking edit-validator \
--new-moniker="validator-adı" \
--identity=F287570B99E59F81 \
--details="node runner 💨 testnet addict 💻" \
--chain-id=ojo-devnet \
--commission-rate=0.1 \
--from=cüzdan-adı \
--gas=auto \
-y 
```

**validator unjail**

```
ojod tx slashing unjail --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y
```

**signing info**

```
ojod query slashing signing-info $(ojod tendermint show-validator)  
```

**aktif validatorleri listele**

```
ojod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**inaktif validatorleri listele**

```
ojod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED") or .status=="BOND_STATUS_UNBONDING")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

**validator detaylarını görüntüle**

```
ojod q staking validator $(ojod keys show cüzdan-adı --bech val -a)  
```

#### Token

***

**bütün validatorlerin ödüllerini çek**

```
ojod tx distribution withdraw-all-rewards --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y 
```

**validatorunuzun komisyon ve ödüllerini çekin**

```
ojod tx distribution withdraw-rewards $(ojod keys show cüzdan-adı --bech val -a) --commission --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y  
```

**kendinize delege etme**

```
ojod tx staking delegate $(ojod keys show cüzdan-adı --bech val -a) 1000000uojo --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y  
```

**delegate**

```
ojod tx staking delegate valoper_adresi 1000000uojo --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y   
```

**redelegate**

```
ojod tx staking redelegate $(ojod keys show cüzdan-adı --bech val -a) valoper-adresi 1000000uojo --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y    
```

**unbond**

```
ojod tx staking unbond $(ojod keys show cüzdan-adı --bech val -a) 1000000uojo --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y  
```

**token gönderme**

```
ojod tx bank send cüzdan-adı YOUR_TO_WALLET_ADDRESS 1000000uojo --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y   
```

#### Governance

***

**yeni teklif oluşturma**

```
ojod tx gov submit-proposal \
--title="Başlık" \
--description="Açıklama" \
--deposit=1000000uojo \
--type="Text" \
--from=cüzdan-adı \
--gas-prices=0.1uojo \
--gas-adjustment=1.5 \
--gas=auto \
-y
```

**tüm proposal/teklifleri görüntüle**

```
ojod query gov proposals  
```

**id'e göre proposal görüntüleme. örnek: 1.**

```
ojod query gov proposal 1  
```

**evet oyu verme**

```
ojod tx gov vote 1 yes --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y   
```

**hayır oyu verme**

```
ojod tx gov vote 1 no --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y    
```

**no\_with\_veto oyu verme**

```
ojod tx gov vote 1 no_with_veto --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y    
```

**ABSTAIN oyu verme**

```
ojod tx gov vote 1 abstain --from cüzdan-adı --chain-id ojo-devnet --gas-prices 0.1uojo --gas-adjustment 1.5 --gas auto -y    
```

#### Çeşitli Komutlar

***

**validator bilgileri**

```
ojod status 2>&1 | jq .ValidatorInfo  
```

**ip adresi öğrenme**

```
 wget -qO- eth0.me 
```

**servisleri yeniden yükleme**

```
sudo systemctl daemon-reload 
```

**servis etkinleştirme**

```
sudo systemctl enable ojod 
```

**servis devre dışı bırakma**

```
sudo systemctl disable ojod 
```

**servis çalıştırma**

```
sudo systemctl start ojod  
```

**restart service**

```
sudo systemctl restart ojod 
```
