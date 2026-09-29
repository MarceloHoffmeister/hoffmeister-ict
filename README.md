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
