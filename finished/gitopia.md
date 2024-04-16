# gitopia

#### Gitopia Hakkında

> [Twitter](https://twitter.com/gitopiaDAO) | [Github](https://gitopia.com/gitopia) | [Website](https://gitopia.com/) | [Docs](https://docs.gitopia.com/) | [Discord](https://discord.gg/eKHqZGXW2S) | [Explorer](https://gitopia.explorers.guru/) | [xyznodes-Validator](https://gitopia.explorers.guru/validator/gitopiavaloper1qvf9ge9jhf8d0gf72mce4ptty95ecfs8up22qp)

***

### Kurulum

#### Minimum Sistem Gereksinimleri

```
4CPU 8RAM 100GB
```

#### Önerilen Sistem Gereksinimleri

```
8CPU 32RAM 200GB
```

**Eğer** [**Manual Kurulum**](broken-reference) **yapacaksanız. Yaptıktan sonra Validator Oluşturma kısımından devam ediniz.**

#### Otomatik Kurulum

```
source <(curl -s https://raw.githubusercontent.com/erdinin/testnet-guides/main/gitopia/install.sh)
```

#### Validator Oluşturma

***

_**Cüzdan Oluşturma**_

```
gitopiad keys add wallet
```

_**Örnek Çıktı**_**:**

```
- name: wallet
  type: local
  address: gitopia1fhxvtld4u6d6a4tvnhyrguvzm53gl3tkkfcfyc
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Auq9WzVEs5pCoZgr2WctjI7fU+lJCH0I3r6GC1oa0tc0"}'
  mnemonic: ""

#!!! seed phrase kısmını mutlaka kayıt edin.
kite upset hip dirt pet winter thunder slice parent flag sand express suffer chest custom pencil mother bargain remember patient other curve cancel sweet
```

**priv\_validator\_key.json dosyasını kayıt edin**

```
cat $HOME/.gitopia/config/priv_validator_key.json
```

**false çıktısı alana kadar bekleyin.**

```
gitopiad status 2>&1 | jq .SyncInfo.catching_up
```

[**Gitopia**](https://gitopia.com/) **'a gidin ve faucet test token alın.**

**token aldıktan sonra cüzdanımızı kontrol edelim.**

```
gitopiad q bank balances $(gitopiad keys show wallet -a)
```

**örnek çıktı:**

```
balances:
  - amount: "10000000"
    denom: utlore
```

**validator oluşturma**

`MONIKER ADI GIRINIZ` `website` `wallet` ve `details` kısımlarını istediğiniz gibi doldurunuz.

```
gitopiad tx staking create-validator \
--amount=9000000utlore \
--pubkey=$(gitopiad tendermint show-validator) \
--moniker="MONIKER ADI GIRINIZ" \
--website="https://xyznodes.xyz" \
--details="node runner 💨 PoS Validator ⚛️ testnet addict 💻" \
--chain-id=gitopia-janus-testnet-2 \
--commission-rate=0.1 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--min-self-delegation=1 \
--fees=20000utlore \
--gas=auto \
--from=wallet \
-y
```

#### validator detaylarınızı aşağıdaki komutla görebilirsiniz.

```
gitopiad q staking validator $(gitopiad keys show wallet --bech val -a)
```

#### Kullanışlı Komutlar

#### Cüzdan/Key

***

**yeni cüzdan ekleme**

```
gitopiad keys add cüzdan-adi
```

**mevcut cüzdanı kurtarma**

```
gitopiad keys add cüzdan-adi --recover
```

**cüzdanları listeleme**

```
gitopiad keys list
```

**cüzdan silme**

```
gitopiad keys delete cüzdan-adi
```

**cüzdan dışa aktarma (cüzdan-adi.backup şeklinde kayıt edin)**

```
gitopiad keys export cüzdan-adi
```

**cüzdan içe aktarma**

```
gitopiad keys import cüzdan-adi cüzdan-adi.backup
```

**cüzdan bakiye öğrenme**

```
gitopiad q bank balances $(gitopiad keys show cüzdan-adi -a)
```

#### Validator/Moniker

***

**yeni validator oluşturma (gerekli yerler doldurunuz.)**

```
gitopiad tx staking create-validator \
--amount=1000000utlore \
--pubkey=$(gitopiad tendermint show-validator) \
--moniker="validator-adi" \
--identity=F287570B99E59F81 (keybase) \
--details="https://xyznodes.xyz" \
--chain-id=gitopia-janus-testnet-2 \
--commission-rate=0.10  \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=cüzdan-adi \
--gas-prices=0.1utlore \
--gas-adjustment=1.5 \
--gas=auto \
-y 
```

**validator düzenleme**

```
gitopiad tx staking edit-validator \
--new-moniker="validator-adi" \
--identity=F287570B99E59F81 (keybase) \
--details="https://xyznodes.xyz" \
--chain-id=gitopia-janus-testnet-2 \
--commission-rate=0.1 \
--from=cüzdan-adi \
--gas-prices=0.1utlore \
--gas-adjustment=1.5 \
--gas=auto \
-y 
```

**validator unjail**

```
gitopiad tx slashing unjail --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y 
```

**signing info**

```
gitopiad query slashing signing-info $(gitopiad tendermint show-validator)  
```

**aktif validatorleri listele**

```
gitopiad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl  
```

**inaktif validatorleri listele**

```
gitopiad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED") or .status=="BOND_STATUS_UNBONDING")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl  
```

**validator detaylarını görüntüle**

```
gitopiad q staking validator $(gitopiad keys show cüzdan-adi --bech val -a)  
```

#### Token

***

**bütün validatorlerin ödüllerini çek**

```
gitopiad tx distribution withdraw-all-rewards --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y 
```

**validatorunuzun komisyon ve ödüllerini çekin**

```
gitopiad tx distribution withdraw-rewards $(gitopiad keys show cüzdan-adi --bech val -a) --commission --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y  
```

**kendinize delege etme**

```
gitopiad tx staking delegate $(gitopiad keys show cüzdan-adi --bech val -a) 4000000000000000000utlore --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y  
```

**delegate**

```
gitopiad tx staking delegate valoper adresi 4000000000000000000utlore --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y  
```

**redelegate**

```
gitopiad tx staking redelegate $(gitopiad keys show cüzdan-adi --bech val -a) valoper adresi 4000000000000000000utlore --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y   
```

**unbound**

```
gitopiad tx staking unbond $(gitopiad keys show cüzdan-adi --bech val -a) 4000000000000000000utlore --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y  
```

**token gönderme**

```
gitopiad tx bank send cüzdan-adi alici-adresi 4000000000000000000utlore --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y  
```

#### Governance

***

**yeni teklif oluşturma**

```
gitopiad tx gov submit-proposal \
--title="Başlık" \
--description="Açıklama" \
--deposit=1000000utlore \
--type="Text" \
--from=cüzdan-adi \
--gas-prices=0.1utlore \
--gas-adjustment=1.5 \
--gas=auto \
-y   
```

**tüm proposal/teklifleri görüntüle**

```
gitopiad query gov proposals  
```

**id'e göre proposal görüntüleme. örnek: 1.**

```
gitopiad query gov proposal 1  
```

**evet oyu verme**

```
gitopiad tx gov vote 1 yes --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y  
```

**hayır oyu verme**

```
gitopiad tx gov vote 1 no --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y   
```

**no\_with\_veto oyu verme**

```
gitopiad tx gov vote 1 no_with_veto --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y   
```

**ABSTAIN oyu verme**

```
gitopiad tx gov vote 1 abstain --from cüzdan-adi --chain-id gitopia-janus-testnet-2 --gas-prices 0.1utlore --gas-adjustment 1.5 --gas auto -y   
```

#### Çeşitli Komutlar

***

**validator bilgileri**

```
gitopiad status 2>&1 | jq .ValidatorInfo  
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
sudo systemctl enable gitopiad 
```

**servis devre dışı bırakma**

```
sudo systemctl disable gitopiad 
```

**servis çalıştırma**

```
sudo systemctl start gitopiad  
```

**restart service**

```
sudo systemctl restart gitopiad 
```

**servis durumu kontrol etme**

```
sudo systemctl status gitopiad 
```

**servis logları**

```
sudo journalctl -u gitopiad -f --no-hostname -o cat
```
