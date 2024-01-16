# berachain

#### Andromeda Hakkında

> [Twitter](https://twitter.com/AndromedaProt) | [Github](https://github.com/andromedaprotocol) | [Website](https://www.andromedaprotocol.io/) | [Telegram](https://t.me/andromedaprotocol) | [Discord](https://discord.gg/9V2htcxKgS) | [Explorer](https://andromeda.explorers.guru/) | [xyznodes-Validator](https://andromeda.explorers.guru/validator/andrvaloper1h9x8jjn5z9sv2cup9u23efqava2q6vmp96hlw9)

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
source <(curl -s https://raw.githubusercontent.com/erdinin/testnet-guides/main/andromeda/install.sh)
```

#### Validator Oluşturma

***

_**Cüzdan Oluşturma**_

```
andromedad keys add wallet
```

_**Örnek Çıktı**_**:**

```
#- name: wallet
#  type: local
#  address: andr1d6z48lnw0g9l8ndumsy9r4cc5xw334mgatm4kx
#  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Auq9WzVEs5pCoZgr2WctjI7fU+lJCH0I3r6GC1oa0tc0"}'
#  mnemonic: ""

#!!! seed phrase kısmını mutlaka kayıt edin.
kite upset hip dirt pet winter thunder slice parent flag sand express suffer chest custom pencil mother bargain remember patient other curve cancel sweet
```

**priv\_validator\_key.json dosyasını kayıt edin**

```
cat $HOME/.andromedad/config/priv_validator_key.json
```

**false çıktısı alana kadar bekleyin.**

```
andromedad status 2>&1 | jq .SyncInfo.catching_up
```

[**Discord**](https://discord.gg/9V2htcxKgS) **#faucet-pub kanalına gidin ve $request cüzdan adresinizi yazarak token alın**

```
$request CUZDAN_ADRESİ
```

**token aldıktan sonra cüzdanımızı kontrol edelim.**

```
andromedad q bank balances $(andromedad keys show wallet -a)
```

**örnek çıktı:**

```
#  balances:
#  - amount: "2000000"
#    denom: uandr
```

**validator oluşturma**

`MONIKER ADI GIRINIZ` `website` ve `details` kısımlarını istediğiniz gibi doldurunuz.

```
andromedad tx staking create-validator \
--amount=1000000uandr \
--pubkey=$(andromedad tendermint show-validator) \
--moniker="MONIKER ADI GIRINIZ" \
--website="https://xyznodes.xyz" \
--details="node runner 💨 PoS Validator ⚛️ testnet addict 💻" \
--chain-id=galileo-3 \
--commission-rate=0.1 \
--commission-max-rate=0.2 \
--commission-max-change-rate=0.05 \
--min-self-delegation=1 \
--fees=10000uandr \
--from=wallet \
-y
```

#### validator detaylarınızı aşağıdaki komutla görebilirsiniz.

```
andromedad q staking validator $(andromedad keys show wallet --bech val -a)
```
