---
icon: terminal
---

# Dymension

#### Dymension Hakkında

> [Twitter](https://twitter.com/dymensionXYZ) | [Github](https://github.com/dymensionxyz) | [Website](https://dymension.xyz/) | [Medium](https://medium.com/@dymensionXYZ) | [Discord](https://discord.gg/dymension) | [Explorer](https://exp.utsa.tech/dymension-test) | [xyznodes-Validator](https://dymension.explorers.guru/validator/dymvaloper1yp62xl4vm9jrhnd4tfcje084ncajxkqkx65rju)

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

#### Otomatik Kurulum

```
source <(curl -s https://raw.githubusercontent.com/erdinin/testnet-guides/main/dymension/install.sh)
```

#### Validator Oluşturma

***

_**Cüzdan Oluşturma**_

```
dymd keys add cüzdan-adı
```

_**Örnek Çıktı**_**:**

```
#- name: wallet
#  type: local
#  address: dym1d6z48lnw0g9l8ndumsy9r4cc5xw334mgatm4kx
#  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Auq9WzVEs5pCoZgr2WctjI7fU+lJCH0I3r6GC1oa0tc0"}'
#  mnemonic: ""

#!!! seed phrase kısmını mutlaka kayıt edin.
kite upset hip dirt pet winter thunder slice parent flag sand express suffer chest custom pencil mother bargain remember patient other curve cancel sweet
```

**priv\_validator\_key.json dosyasını kayıt edin**

```
cat $HOME/.dymension/config/priv_validator_key.json
```

**false çıktısı alana kadar bekleyin.**

```
dymd status 2>&1 | jq .SyncInfo.catching_up
```

[**Discord**](https://discord.gg/dymension) **#faucet kanalına gidin ve $request cüzdan adresi 35c-hub yazarak token alın**

```
$request CUZDAN_ADRESİ 35c-hub
```

**token aldıktan sonra cüzdanımızı kontrol edelim.**

```
dymd q bank balances $(dymd keys show cüzdan-adı -a)
```

**örnek çıktı:**

```
#  balances:
#  - amount: "10000000"
#    denom: udym
```

**validator oluşturma**

`MONIKER ADI GIRINIZ` ve `cüzdan-adı` kısımlarını doldurunuz.

```
dymd tx staking create-validator \
--amount=1000000udym \
--pubkey=$(dymd tendermint show-validator) \
--moniker="MONIKER ADI GIRINIZ" \
--chain-id=35-C \
--commission-rate=0.1 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--min-self-delegation=1 \
--fees=1000udym \
--from=cüzdan-adı \
-y
```

#### validator detaylarınızı aşağıdaki komutla görebilirsiniz.

```
dymd q staking validator $(dymd keys show wallet --bech val -a)
```

**validator detaylarınızı aşağıdaki komutla değiştirebilirsiniz. `idendity` , `details` kısımlarını doldurabilirsiniz.**

```
dymd tx staking edit-validator \
--new-moniker="validatör-adı" \
--identity=F287570B99E59F81 \
--details="node runner 💨 testnet addict 💻" \
--chain-id=35-C \
--from=cüzdan-adı \
--gas-prices=0.1udym \
--gas-adjustment=1.5 \
--gas=auto \
-y
```
