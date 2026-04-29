# Claude Skills — Forex & Oro (XAU/USD)

> Descargadas: 2026-04-29

Skills de repositorios open source reconocidos, seleccionadas por cobertura explícita de forex, commodities y oro.

---

## Fuentes

| Repo | Estrellas | Enfoque |
|---|---|---|
| [tradermonty/claude-trading-skills](https://github.com/tradermonty/claude-trading-skills) | ⭐1135 | Equity + forex + macro, multi-skill ecosystem |
| [ThomasPraun/mql-developer](https://github.com/ThomasPraun/mql-developer) | ⭐13 | MQL4/MQL5 — Expert Advisors nativos para MetaTrader |
| [Hainrixz/maia-skill](https://github.com/Hainrixz/maia-skill) | ⭐73 | Multi-agente: cubre forex + oro + crypto + stocks |

---

## tradermonty — Skills con cobertura Forex/Oro (15 skills)

| Skill | Relevancia XAU/Forex |
|---|---|
| [[tradermonty/technical-analyst]] | Análisis técnico de charts — **explicita forex pairs** |
| [[tradermonty/market-environment-analysis]] | Global macro — **FX, commodities, yields** |
| [[tradermonty/macro-regime-detector]] | Regímenes macro — **Gold sector, equity/bond, yield curve** |
| [[tradermonty/market-news-analyst]] | Noticias macro: FOMC, geopolitics, **commodity drivers** |
| [[tradermonty/economic-calendar-fetcher]] | Calendario económico: Fed, ECB, NFP, CPI |
| [[tradermonty/stanley-druckenmiller-investment]] | Marco macro Druckenmiller — **Gold como activo de reserva** |
| [[tradermonty/sector-analyst]] | Sector rotation incluyendo materiales/metales |
| [[tradermonty/institutional-flow-tracker]] | Flujo institucional — smart money en forex y commodities |
| [[tradermonty/pair-trade-screener]] | Pairs trading — directamente aplicable a correlaciones XAUUSD |
| [[tradermonty/scenario-analyzer]] | Análisis de escenarios multi-outcome |
| [[tradermonty/market-top-detector]] | Detección de techos de mercado (útil para timing en oro) |
| [[tradermonty/options-strategy-advisor]] | Estrategias de opciones sobre commodities |
| [[tradermonty/portfolio-manager]] | Gestión de portfolio con activos no correlacionados |
| [[tradermonty/position-sizer]] | Sizing de posiciones |
| [[tradermonty/backtest-expert]] | Backtesting de estrategias |

---

## ThomasPraun/mql-developer — MetaTrader 4/5 (skill completa)

Skill para desarrollo nativo de EAs y sistemas automáticos en MetaTrader — la plataforma estándar de forex.

| Archivo | Contenido |
|---|---|
| [[mql-developer/SKILL.md]] | Skill principal — MQL4/MQL5 full ecosystem |
| [[mql-developer/references/mql4-reference]] | Referencia completa MQL4 |
| [[mql-developer/references/mql5-reference]] | Referencia completa MQL5 con OOP |
| [[mql-developer/references/architecture-patterns]] | Patrones de arquitectura para EAs |
| [[mql-developer/references/trading-operations]] | Orders, positions, risk management, trailing stops |
| [[mql-developer/references/indicators-and-ui]] | Indicadores personalizados, paneles |
| [[mql-developer/references/external-communication]] | WebRequest, REST API, JSON |
| [[mql-developer/references/backtesting]] | Strategy Tester, walk-forward, Monte Carlo |
| [[mql-developer/references/security-licensing]] | Protección de código |

---

## Hainrixz/maia — Multi-Agent Investment Analysis

Sistema de 5 agentes paralelos que cubre explícitamente:
- **Currencies Agent**: DXY, USD/MXN y los pares más relevantes del momento
- **Materials Agent**: **Gold (always included)** + Oil WTI + commodities dinámicas

| Archivo | Contenido |
|---|---|
| [[maia-multiagent/SKILL.md]] | Skill principal — orquestador de 5 agentes |
| [[maia-multiagent/agent-prompts]] | Prompts para cada agente (Crypto/Stocks/Currencies/Materials/Strategy) |

---

## Skills más relevantes para XAU/USD específicamente

1. **[[tradermonty/technical-analyst]]** — análisis de charts XAUUSD (imagen → análisis técnico)
2. **[[tradermonty/market-environment-analysis]]** — contexto macro para oro: DXY, yields, riesgo
3. **[[tradermonty/macro-regime-detector]]** — régimen actual: inflacionario = bull gold
4. **[[tradermonty/stanley-druckenmiller-investment]]** — gold como cobertura macro
5. **[[maia-multiagent/SKILL.md]]** — reporte diario multi-activo con gold siempre incluido
6. **[[mql-developer/SKILL.md]]** — construir EA para XAUUSD en MT4/MT5

---

## Nota sobre repositorios oficiales de Anthropic

Anthropic no publica repositorios de skills de trading. El marketplace oficial de Claude Code es [claude.ai/marketplace](https://claude.ai/marketplace) pero no tiene skills de forex/oro. Los repos más reconocidos por la comunidad Claude Code son `tradermonty` (⭐1135) y `jeremylongshore` (el que ya tienes descargado).
