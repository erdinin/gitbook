# base

#### Base Hakkında

> [Twitter](https://twitter.com/BuildOnBase) | [Github](https://github.com/base-org) | [Website](https://base.org/) | [Docs](https://docs.base.org/) | [Discord](https://discord.gg/buildonbase) | [Explorer](https://goerli.basescan.org/)

***

### Kurulum

#### Minimum Sistem Gereksinimleri

```
4CPU 16RAM 100GB
```

#### Önerilen Sistem Gereksinimleri

```
8CPU 32RAM 100GB+
```

**Sistem Güncellemeleri**

```
sudo apt update
```

```
sudo apt upgrade
```

**Docker Kurulumu**

```
apt install docker-compose
```

```
sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
```

**Gerekli Dosyalar**

```
apt install screen
```

```
git clone https://github.com/base-org/node.git
```

```
screen -S base
```

```
cd node
```

**Kendimize ait bir Ethereum Goerli L1 node RPC'miz olması gerekiyor.**

**Bunun için** [**BlockPI**](https://dashboard.blockpi.io/register)**'yi kullanacağız. Gerekli bilgileri dolduruyoruz ve üye oluyoruz. Gelen maili onaylayıp üyeliğimizi aktive ediyoruz. Login olduktan sonra gelen ekranı default şekilde bırakıp confirm diyoruz. Ardından gelen sayfada Generate API Key'e basıyoruz. Bu kısımı `Ethereum` - `Goerli` - `API Key Name` belirliyoruz ve Generate diyoruz. Alt tarafa doğru indiğimizde https ile başlayan rpc adresini kopyalıyoruz. Bu bize gerekli olacak.**

```
nano docker-compose.yml
```

**Bu komutu girdikten sonra aşağıdaki gibi bir çıktı alacaksınız. `OP_NODE_L1_ETH_RPC`** **`✨..✨`arasında kalan kısımları kopyaladığımız `rpc adresi` ile değiştiriyoruz. Değiştirdikten sonra `CTRL+X+Y` ile kayıt ediyoruz.**

```
services:
  geth: # this is Optimism's geth client
    build: .
    ports:
      - 8545:8545
      - 8546:8546
      - 30303:30303
    command:
      - ./geth-entrypoint.sh
    environment:
      - OP_GETH_GENESIS_FILE_PATH=goerli/genesis-l2.json
      - OP_GETH_SEQUENCER_HTTP=https://goerli.base.org
      - OP_NODE_L2_ENGINE_AUTH=/tmp/engine-auth-jwt
      - OP_NODE_L2_ENGINE_AUTH_RAW=688f5d737bad920bdfb2fc2f488d6b6209eebda1dae949a8de91398d932c517a # for localdev only
  node:
    build: .
    depends_on:
      - geth
    ports:
      - 7545:8545
      - 9222:9222
      - 7300:7300 # metrics
      - 6060:6060
    command:
      - ./op-node-entrypoint.sh
    environment:
      - OP_NODE_L1_ETH_RPC=✨https://goerli.blockpi.network/v1/rpc/e71x3b4e381f1292abief7d0c19235b1y2c10789✨ # [recommended] replace with your preferred L1 (ethereu>      - OP_NODE_L2_ENGINE_AUTH=/tmp/engine-auth-jwt
      - OP_NODE_L2_ENGINE_AUTH_RAW=688f5d737bad920bdfb2fc2f488d6b6209eebda1dae949a8de91398d932c517a # for localdev only
      - OP_NODE_L2_ENGINE_RPC=http://geth:8551
      - OP_NODE_LOG_LEVEL=info
      - OP_NODE_METRICS_ADDR=0.0.0.0
      - OP_NODE_METRICS_ENABLED=true
      - OP_NODE_METRICS_PORT=7300
      - OP_NODE_P2P_AGENT=base
      - OP_NODE_P2P_BOOTNODES=enr:-J64QBbwPjPLZ6IOOToOLsSjtFUjjzN66qmBZdUexpO32Klrc458Q24kbty2PdRaLacHM5z-cZQr8mjeQu3pik6jPSOGAYYFIqBfgmlkgnY0gmlwhDaRWFWHb3BzdGF>      - OP_NODE_P2P_LISTEN_IP=0.0.0.0
      - OP_NODE_P2P_LISTEN_TCP_PORT=9222
      - OP_NODE_P2P_LISTEN_UDP_PORT=9222
      - OP_NODE_ROLLUP_CONFIG=goerli/rollup.json
      - OP_NODE_RPC_ADDR=0.0.0.0
      - OP_NODE_RPC_PORT=8545
```

**Çalıştırma**

```
docker compose up
```

**Yukarıdaki komutu girdikten sonra buna benzer bir çıktı alacaksınız.**

```
[+] Building 20.4s (7/19)                                                                                                                                         
 => [node-node internal] load build definition from Dockerfile                                                                                               0.1s
 => => transferring dockerfile: 873B                                                                                                                         0.0s
 => [node-node internal] load .dockerignore                                                                                                                  0.1s
 => => transferring context: 2B                                                                                                                              0.0s
 => [node-geth internal] load build definition from Dockerfile                                                                                               0.1s
 => => transferring dockerfile: 873B                                                                                                                         0.0s
 => [node-geth internal] load .dockerignore                                                                                                                  0.1s
 => => transferring context: 2B                                                                                                                              0.0s
 => [node-node internal] load metadata for docker.io/library/golang:1.19                                                                                     1.9s
 => [node-geth geth 1/4] FROM docker.io/library/golang:1.19@sha256:5d947843dde82ba1df5ac1b2ebb70b203d106f0423bf5183df3dc96f6bc5a705                         18.4s
 => => resolve docker.io/library/golang:1.19@sha256:5d947843dde82ba1df5ac1b2ebb70b203d106f0423bf5183df3dc96f6bc5a705                                         0.1s
 => => sha256:5d947843dde82ba1df5ac1b2ebb70b203d106f0423bf5183df3dc96f6bc5a705 2.36kB / 2.36kB                                                               0.0s
 => => sha256:d6dfff1f6f3ddd2194ea0775f199572e8b2d75c38713eef0444d6b1fd0ac7604 10.88MB / 10.88MB                                                             0.8s
 => => sha256:7767d772324bf9e98417806bfc321a805ab0f41bedcbd1a854929a5e33e29444 1.80kB / 1.80kB                                                               0.0s
 => => sha256:ff3cd58379d9fe62ec35adbdea400c18bf642bb54e03c3b66420207b17ff7f06 7.10kB / 7.10kB                                                               0.0s
 => => sha256:32fb02163b6bb519a30f909008e852354dae10bdfd6b34190dbdfe8f15403ea0 55.05MB / 55.05MB                                                             3.1s
 => => sha256:167c7feebee855d117e192389484ea8367be1ba84e7ee35f4e5e5663195facbf 5.17MB / 5.17MB                                                               0.7s
 => => sha256:e9cdcd4942ebc7445d8a70117a83ecbc77dcc5ffc72c4b6f8e24c0c76cfee15d 54.59MB / 54.59MB                                                             3.1s
 => => sha256:543368fb39eebb09d53cdd07e735a73a50b9773ad9019a5563e816d88a75e067 85.99MB / 85.99MB                                                             6.4s
 => => sha256:2b9e0f536a8462dc9978a846920ed5f7597dae4d1f0f04398e37280b747a1b4c 149.01MB / 149.01MB                                                           8.8s
 => => sha256:5194065603787f6175cc14a3eae788fb86babcf6c36caa888eca4059f4a10495 156B / 156B                                                                   3.4s
 => => extracting sha256:32fb02163b6bb519a30f909008e852354dae10bdfd6b34190dbdfe8f15403ea0                                                                    7.4s
 => => extracting sha256:167c7feebee855d117e192389484ea8367be1ba84e7ee35f4e5e5663195facbf                                                                    0.6s
 => => extracting sha256:d6dfff1f6f3ddd2194ea0775f199572e8b2d75c38713eef0444d6b1fd0ac7604                                                                    0.8s
 => => extracting sha256:e9cdcd4942ebc7445d8a70117a83ecbc77dcc5ffc72c4b6f8e24c0c76cfee15d                                                                    5.4s
 => [node-geth internal] load build context                                                                                                                  0.3s
 => => transferring context: 11.95MB                                                                                                                         0.2s
 => [node-node internal] load build context                                                                                                                  0.3s
 => => transferring context: 11.95MB                                                                                                                         0.2s
```

**Biraz bekledikten sonra Loglar akmaya başlayacaktır.**
