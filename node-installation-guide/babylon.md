# babylon

#### Babylon Hakkında

> [Twitter](https://twitter.com/babylon\_chain) | [Github](https://github.com/babylonchain) | [Website](https://www.babylonchain.io/) | [Discord](https://discord.gg/babylonchain) | [Explorer](https://babylon.explorers.guru/) | [xyznodes-Validator](https://babylon.explorers.guru/validator/bbnvaloper1c3dqepg3d09laank40dgulv5dzyvp4nfq6h8jd)

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
source <(curl -s https://raw.githubusercontent.com/erdinin/testnet-guides/main/babylon/install.sh)
```

#### Validator Oluşturma

***

_**Cüzdan Oluşturma**_

```
babylond keys add cüzdan-adı
```

_**Örnek Çıktı**_**:**

```
#- name: cüzdan-adı
#  type: local
#  address: bbn1d6z48lnw0g9l8ndumsy9r4cc5xw334mgatm4kx
#  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Auq9WzVEs5pCoZgr2WctjI7fU+lJCH0I3r6GC1oa0tc0"}'
#  mnemonic: ""

#!!! seed phrase kısmını mutlaka kayıt edin.
kite upset hip dirt pet winter thunder slice parent flag sand express suffer chest custom pencil mother bargain remember patient other curve cancel sweet
```

**priv\_validator\_key.json dosyasını kayıt edin**

```
cat $HOME/.babylond/config/priv_validator_key.json
```

**false çıktısı alana kadar bekleyin.**

```
babylond status 2>&1 | jq .SyncInfo.catching_up
```

[**Discord**](https://discord.gg/babylonchain) **#faucet kanalına gidin ve cüzdan adresinizi yazarak token alın**

**token aldıktan sonra cüzdanımızı kontrol edelim.**

```
babylond q bank balances $(babylond keys show wallet -a)
```

**örnek çıktı:**

```
#  balances:
#  - amount: "100"
#    denom: ubbn
```

```
babylond create-bls-key $(babylond keys show wallet -a)
```

```
sudo systemctl restart babylond
```

**validator oluşturma**

`MONIKER ADI GIRINIZ` `website` ve `details` kısımlarını istediğiniz gibi doldurunuz.

```
babylond tx checkpointing create-validator \
--amount=10ubbn \
--pubkey=$(babylond tendermint show-validator) \
--moniker="$NODE_MONIKER" \
--chain-id=bbn-test1 \
--commission-rate=0.1 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--min-self-delegation=1 \
--fees=20ubbn \
--from=wallet \
-y
```

#### validator detaylarınızı aşağıdaki komutla görebilirsiniz. validatörünüzün web sitesinde görünmesi 30 dakika kadar sürebiliyor.

```
babylond q staking validator $(babylond keys show wallet --bech val -a)
```
