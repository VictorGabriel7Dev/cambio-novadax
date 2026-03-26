[📄 Página](https://victorgabriel.dev/projetos/cambio-novadax) · [💻 GitHub](https://github.com/VictorGabriel7Dev/cambio-novadax)
# Câmbio NovaDax

Consultor de cotações e custos de envio de criptomoedas via [NovaDax](https://www.novadax.com.br), direto no terminal.

Dado um valor e uma moeda, o script responde:

- **Qual o preço atual** da moeda na NovaDax (em BRL ou SOL)
- **Quanto você precisa ENVIAR** da sua carteira para que o destino receba exatamente o valor solicitado (já descontando a taxa de rede)
- **Quanto você precisa COMPRAR** na exchange para cobrir o envio e a taxa de negociação (taker)
- **Qual o custo total em BRL**, com detalhamento de cada componente

Funciona para todas as moedas listadas na NovaDax. Redes com taxa dinâmica (`dynamic`) exibem aviso, pois o custo só é conhecido no momento do saque.

---

## Exemplo de saída

```
════════════════════════════════════════════════════════════════════
  NovaDax  ·  Câmbio Cripto
  Consultando  0.1 DASH
════════════════════════════════════════════════════════════════════

  ▶  DASH  ·  Dash (DASH)
────────────────────────────────────────────────────────────────────
── Mercado

  Cotação atual (1 DASH)                          R$ 178,29

── Limites e taxas de rede

  Mínimo de depósito                    0.19113150 DASH
                                        →  R$ 34,08

  Taxa de saque da rede                 0.00100000 DASH
                                        →  R$ 0,18

── Custo para comprar e enviar

  Destino receberá exatamente  :  0.10000000 DASH
  Taxa de compra (taker)       :  0.60%  (VIP0)

── Em reais (BRL)

  Valor da moeda  (0.1000 × R$178,29)            R$ 17,83
  Taxa de saque   (0.0010 × R$178,29)             R$ 0,18
  ··················································
  Subtotal                                        R$ 18,01
  Taxa de compra  (0.60% s/ subtotal)              R$ 0,11
  ──────────────────────────────────────────────────
  TOTAL A PAGAR (BRL)                             R$ 18,12

── Em DASH

  Quantidade que chegará ao destino        0.10000000 DASH
  Taxa descontada pela rede no envio       0.00100000 DASH
  ··················································
  Precisa ENVIAR  (sai da sua carteira)    0.10100000 DASH
  Taxa de compra  (0.60%)                  0.00060600 DASH
  ──────────────────────────────────────────────────
  Precisa COMPRAR na exchange              0.10160966 DASH

────────────────────────────────────────────────────────────────────
```

---

## Uso

```bash
python Cambio-NovaDax.py 0.1 DASH
python Cambio-NovaDax.py 0.1DASH
python Cambio-NovaDax.py DASH0.1
```

Aceita número e moeda em qualquer ordem, com ou sem espaço, vírgula ou ponto como separador decimal.

---

## Requisitos

- Python 3.10 ou superior
- Sem dependências externas — usa apenas a biblioteca padrão (`urllib`, `json`, `re`, `sys`)

---

## Instalação

```bash
git clone https://github.com/VictorGabriel7Dev/cambio-novadax.git
cd cambio-novadax
python Cambio-NovaDax.py 0.05 BTC
```

Nenhum `pip install` necessário.

---

## Como funciona

**Cotação usada:** maior preço entre os 15 melhores asks do livro de ordens, que representa o pior caso de execução a mercado.

**Par de referência:** tenta `MOEDA_BRL` primeiro. Se não existir, tenta `SOL_MOEDA` e exibe a conversão adicional necessária em SOL.

**Fórmulas:**

```
# Em BRL
subtotal   = (quantidade × cotacao) + (taxa_rede × cotacao)
total_brl  = subtotal / (1 - taker_fee)

# Em moeda
a_enviar   = quantidade + taxa_rede          # o que sai da sua carteira
a_comprar  = a_enviar   / (1 - taker_fee)   # o que você compra na exchange
```

---
## Configuração

Abra o arquivo e ajuste as constantes no topo:
```python
VIP_LEVEL      = "VIP0"   # nível padrão; ajuste conforme seu cadastro
QUANTIDADE_MEDIA = 15     # quantidade de ofertas a buscar; usa o preço mais alto
USER_AGENT     = "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:92.0) Gecko/20100101 Firefox/92.0"
```

**`VIP_LEVEL`** — define a taxa taker usada nos cálculos. O valor é buscado automaticamente da API conforme o nível informado. Níveis disponíveis: `VIP0` a `VIP10`.

**`QUANTIDADE_MEDIA`** — quantas ofertas do livro de ordens são consultadas. O script usa o **maior preço** entre elas, representando o pior caso de execução a mercado.

**`USER_AGENT`** — para ser usado nas requisições http.

---

## Observações

- Redes com `feeType: dynamic` não têm taxa de saque fixa — o valor é calculado pela NovaDax no momento do saque. O script exibe um aviso e não apresenta o breakdown de custo para essas redes.
- Quando uma moeda possui múltiplas redes (ex: DOGE na rede nativa e BEP20), o script exibe o resultado para cada rede separadamente.
- Os valores são informativos e podem variar entre o momento da consulta e a execução da operação.

---

## Autor

**Victor Gabriel**  
· [victorgabriel.dev](https://victorgabriel.dev)  
· [victorgabriel.dev.br](https://victorgabriel.dev.br)  
· GitHub: [github.com/VictorGabriel7Dev](https://github.com/VictorGabriel7Dev)  
- LinkedIn: [in/victor-gabriel-182a763b9](https://linkedin.com/in/victor-gabriel-182a763b9/)  
· Discord: `@VictorGabriel.dev`  
· Telegram: [t.me/VictorGabriel_Dev](https://t.me/VictorGabriel_Dev)  
· E-mail: contato@victorgabriel.dev  
  
Gerado por [Claude](https://claude.ai).

---

## Licença

Este projeto está licenciado sob a [Licença Pública Geral Affero GNU v3.0](https://www.gnu.org/licenses/agpl-3.0.html)  
Para mais detalhes, consulte o arquivo [LICENSE](LICENSE).  
