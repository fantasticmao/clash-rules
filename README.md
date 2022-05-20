# FantasticMao 的 Clash 规则集

## 友情提醒

在使用 FantasticMao 的 Clash 规则集之前，不妨先看 [Loyalsoldier 的 Clash 规则集](https://github.com/Loyalsoldier/clash-rules)。

Loyalsoldier 版的规则集更加普适和通用，FantasticMao 版的规则集更加适合喜欢定制规则的伙伴。

## 使用方式

### 安装 Clash Premium

实现外部加载 Clash 规则时，需要使用 [Rule Provider](https://github.com/Dreamacro/clash/wiki/premium-core-features#rule-providers)
高级特性，因此需要用户安装闭源的 [Clash Premium](https://github.com/Dreamacro/clash/releases/tag/premium)，而非开源的 Clash。

Clash Premium 的使用方式请参考 [premium core features](https://github.com/Dreamacro/clash/wiki/premium-core-features)。

### 调整 Clash 配置文件

使用 Rule Provider 特性时，需要在 Clash 的原配置文件中添加 `rule-providers` 配置项和调整 `rules` 配置项，关键配置示例如下：

```yaml
proxies:
  - name: proxy01
    type: ss
    server: server
    port: 443
    cipher: chacha20-ietf-poly1305
    password: password
  - name: proxy02
    type: vmess
    server: server
    port: 443
    uuid: uuid
    alterId: 32
    cipher: auto

proxy-groups:
  - name: PROXY
    type: select
    proxies:
      - proxy01
      - proxy02
    url: http://www.gstatic.com/generate_204

# Premium only
rule-providers:
  common:
    type: http
    behavior: classical
    url: https://fastly.jsdelivr.net/gh/fantasticmao/clash-rules@main/common.yaml
    interval: 3600
    path: ./ruleset/common.yaml
  telegramcidr:
    type: http
    behavior: classical
    url: https://fastly.jsdelivr.net/gh/fantasticmao/clash-rules@main/telegramcidr.yaml
    interval: 3600
    path: ./ruleset/telegramcidr.yaml

rules:
  - RULE-SET,common,PROXY
  - RULE-SET,telegramcidr,PROXY
  # Others
  - GEOIP,CN,DIRECT
  - MATCH,DIRECT
```

Clash 的配置方式请参考 [configuration](https://github.com/Dreamacro/clash/wiki/configuration)。
