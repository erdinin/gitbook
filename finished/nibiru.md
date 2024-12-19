---
icon: terminal
---

# Nibiru

#### Nibiru Hakkında

> [Twitter](https://twitter.com/NibiruChain) | [Github](https://github.com/NibiruChain) | [Website](https://nibiru.fi/) | [Blog](https://nibiru.fi/blog) | [Discord](https://discord.gg/nibiru) | [Explorer](https://nibiru.explorers.guru/) | [xyznodes-Validator](https://nibiru.explorers.guru/validator/nibivaloper1c3dqepg3d09laank40dgulv5dzyvp4nf5cxfum)

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

***

#### Otomatik Kurulum

```
source <(curl -s https://raw.githubusercontent.com/erdinin/testnet-guides/main/nibiru/install.sh)
```

#### Validator Oluşturma

***

_**Cüzdan Oluşturma**_

```
nibid keys add cüzdan-adı
```

_**Örnek Çıktı**_**:**

```
- name: wallet
   type: local
   address: nibi1r9kmadqs9nsppn4wz5yp4rw8zn9545rc4zwvs7
   pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Auq9WzVEs5pCoZgr2WctjI7fU+lJCH0I3r6GC1oa0tc0"}'
   mnemonic: ""

#!!! seed phrase kısmını mutlaka kayıt edin.
kite upset hip dirt pet winter thunder slice parent flag sand express suffer chest custom pencil mother bargain remember patient other curve cancel sweet
```

**priv\_validator\_key.json dosyasını kayıt edin**

```
cat $HOME/.nibid/config/priv_validator_key.json
```

**false çıktısı alana kadar bekleyin.**

```
nibid status 2>&1 | jq .SyncInfo.catching_up
```

**`buraya cüzdan adresinizi girin` kısmını değiştirin.**

```
FAUCET_URL="https://faucet.itn-1.nibiru.fi/"
ADDR="buraya cüzdan adresinizi girin"
curl -X POST -d '{"address": "'"$ADDR"'", "coins": ["11000000unibi","100000000unusd","100000000uusdt"]}' $FAUCET_URL
```

**token aldıktan sonra cüzdanımızı kontrol edelim.**

```
nibid q bank balances $(nibid keys show wallet -a)
```

**örnek çıktı:**

```
console output:
  balances:
  - amount: "11000000"
    denom: unibi
```

**validator oluşturma**

```
nibid tx staking create-validator \
--amount=10000000unibi \
--pubkey=$(nibid tendermint show-validator) \
--moniker="$NODE_MONIKER" \
--chain-id=nibiru-itn-1 \
--commission-rate=0.1 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--min-self-delegation=1 \
--fees=2000unibi \
--from=cüzdan-adı \
-y
```

#### validator detaylarınızı aşağıdaki komutla görebilirsiniz.

```
nibid q staking validator $(nibid keys show wallet --bech val -a)
```
