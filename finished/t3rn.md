---
icon: terminal
---

# t3rn

#### t3rn **Hakkında**

{% hint style="success" %}
[**Twitter**](https://x.com/t3rn_io) **|** [**Website**](https://www.t3rn.io/) **|** [**Discord**](https://discord.com/invite/S5kHFQTtp6) **|** [**Docs**](https://docs.t3rn.io/intro)
{% endhint %}

{% hint style="info" %}
**t3rn**, Kullanıcılar için üstün takas, oluşturucular için güçlü modülerlik.
{% endhint %}

***

<details>

<summary><strong>t3rn Executor Node</strong></summary>

```csharp
wget <https://github.com/t3rn/executor-release/releases/download/v0.29.0/executor-linux-v0.27.0.tar.gz>
tar -xvzf executor-linux-v0.29.0.tar.gz
cd /root/executor/executor/bin
```

```bash
export NODE_ENV=testnet

```

```csharp
export LOG_LEVEL=debug
export LOG_PRETTY=false
```

```csharp
export EXECUTOR_PROCESS_ORDERS=true
export EXECUTOR_PROCESS_CLAIMS=true
```

```bash
export PRIVATE_KEY_LOCAL=private keyinizi girin
```

```bash
export ENABLED_NETWORKS='arbitrum-sepolia,base-sepolia,optimism-sepolia,l1rn'

```

```csharp
./executor
```

* Arbitrum Sepolia ETH: Minimum 4 ETH
* Blast Sepolia ETH: Minimum 4 ETH
* Optimism Sepolia ETH: Minimum 4 ETH
* Base Sepolia ETH: Minimum 4 ETH
* Minimum 11 $BRN
* &#x20;[http://gas.zip](http://gas.zip/) kullanarak yukarıdaki test ETH ler satın alınabilir. \
  \
  Gas tracker; [https://brn.explorer.caldera.xyz/gas-tracker](https://brn.explorer.caldera.xyz/gas-tracker)

</details>

<details>

<summary>t3rn Testnet</summary>

[https://bridge.t1rn.io/](https://bridge.t1rn.io/)

</details>
