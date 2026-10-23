# Hoffmeister ICT

Indicador Pine Script (TradingView, `@version=5`) com camadas de detecção
ICT/SMC (Inner Circle Trader / Smart Money Concepts), portado a partir de um
lab de backtest em Python. É puramente visual — sem MTF e sem sinal de
entrada — calibrado originalmente para o timeframe de 15 minutos (swing 5,
FVG mínimo de $2).

## Camadas (todas ligáveis/desligáveis individualmente)

1. **Estrutura** — swings (topos/fundos) + BOS/CHoCH, com o nível protegido
   trilhando o pivô mais recente a favor do viés.
2. **FVG / iFVG** — Fair Value Gaps, com mitigação por fechamento (evita
   repaint intrabar) e inversão para iFVG quando o preço atravessa.
3. **Order Blocks / Breaker** — OB da vela-origem do impulso, virando Breaker
   Block (BB) quando o corpo fecha do outro lado.
4. **Suporte / Resistência (S/R)** — alternativa mais simples ao OB/Breaker:
   linha horizontal que inverte de papel no 1º rompimento e é removida no 2º.
5. **Sessões (Ásia / NY)** — caixa diária com o range de cada sessão.
6. **Range / Concentração** — detecta contração de range (squeeze) comparando
   contra a média histórica do próprio range.
7. **Trade Zone** — janela de operação pessoal (NY e Ásia), configurável por
   horário e fuso.
8. **Acumulação (estrutura)** — faixa formada por ≥ 3 topos e ≥ 3 fundos
   (pivôs) sem continuação (HH+HL / LH+LL), do maior topo ao menor fundo;
   pavios que fecham de volta estendem a faixa e o 1º fechamento fora a
   encerra (↑ ou ↓). Pivôs outliers (pernas de deslocamento, como o topo
   antes de uma queda ou o fundo de um flush) são descartados pelo input
   "Corte de outlier". Diferente do Range, não depende de volatilidade.
   Camada em pausa (desligada por padrão): o critério visual ainda não foi
   fechado.
9. **Liquidez** — fontes de liquidez do setup "tomada → reversão":
   *swing válido* (`$$`: o topo 2 só vale se o preço fechar abaixo do fundo 1
   antes de fechar acima do 2; espelho para fundos) e *topos/fundos
   nivelados* (`EQH`/`EQL`: pivôs a menos de 0,25 × ATR um do outro). Os
   níveis vêm do TF escolhido em "TF da liquidez" (vazio = TF do gráfico; ex.: 15
   para níveis do M15, via `request.security`);
   a tomada é checada pelo pavio a cada barra do gráfico. As linhas só são
   desenhadas em gráficos a partir de M15 (input); em TFs menores o nível é
   apenas calculado.
   A tomada (pavio passa do nível) vira pontilhado cinza com ✕ e dispara
   `alertcondition` ("Tomada de liquidez de TOPO/FUNDO").
10. **Gatilho** — padrão 1-2-3; por padrão só vale depois de uma tomada de
    liquidez do lado certo (input "Exigir tomada", desligável); roda só em
    gráficos até M5 (input).
    Compra: topo 1 → fundo 2 → topo 3 acima do 1 (pivôs nos fechamentos, como
    no gráfico de linha) → fecha abaixo do fundo 2 → fecha acima do topo 3.
    O POI é a caixa entre o extremo da perna de baixa e o topo 3, cobrindo
    os **pavios** (ou só o corpo, via input); a linha de entrada fica na borda do POI ou em
    50%. Stop e alvo são calculados mas não desenhados: aparecem na Data
    Window e no texto do alerta. Venda é o espelho. Se o preço fechar além da
    borda oposta do POI (acima do topo numa venda, abaixo do fundo numa compra),
    a caixa, a linha de entrada e o rótulo COMPRA/VENDA somem. Só
    desenha (sem ordem/estratégia) e dispara `alertcondition`.

## Instalação no TradingView

1. Abra o [Pine Editor](https://www.tradingview.com/pine-editor/) no
   TradingView.
2. Crie um novo indicador em branco.
3. Copie e cole o conteúdo de
   [`hoffmeister_ict.pine`](hoffmeister_ict.pine).
4. Clique em **Add to chart**.
5. Ative/desative cada camada e ajuste os parâmetros pelo painel de inputs,
   agrupados por seção (Estrutura, FVG, OB, Sessões, Range, Trade Zone, S/R).

## Aviso

Indicador visual de apoio à análise — não gera sinais de entrada/saída
automáticos e não constitui recomendação de investimento.
