# Métricas de Produto — Mind Reader

> Documento vivo. Define a **estrela-guia**, o **funil** e as **definições** que devem
> orientar decisões de produto no Mind Reader. Pensado para um produto que, por
> princípio, **não coleta telemetria** — então a forma de medir é tão importante
> quanto o que se mede.

---

## 0. O contexto que muda tudo

Quase todo framework de métricas assume telemetria. O pilar de marca do Mind Reader é
o oposto: *"no account, no sync, no server — because there is no server. No telemetry."*

Consequência prática: **o sucesso do produto é real, mas em grande parte invisível** para
quem o constrói. O dashboard local (palavras completadas, tempo economizado, acceptance
rate, streak) existe **para o usuário**, não para nós — esses dados nunca saem da máquina.

Por isso este documento separa, sempre, **três planos**:

1. **O que define sucesso** (conceitual — a verdade do produto).
2. **O que dá pra medir hoje** sem trair a promessa de privacidade.
3. **Proxies e qualitativo** que aproximam o que não conseguimos ver diretamente.

> Regra de ouro: **nenhuma métrica justifica quebrar a promessa de privacidade.**
> Se um dia houver instrumentação, que seja opt-in explícito, agregada e sem conteúdo.
> (Exploração disso vive em `docs/METRICS-IDEIAS.md`.)

---

## 1. Estrela-guia (North Star)

O lema do produto é *"writes the predictable part, so you don't have to."*
A unidade de valor é, literalmente, **a palavra que você não precisou digitar**.

> ### ⭐ North Star: palavras aceitas por usuário ativo por semana.

Por que esta e não outra:

- **Combina três coisas num número só** — alcance (em quantos apps/campos funciona),
  engajamento (quanto a pessoa usa) e qualidade (a sugestão precisa ser boa o
  suficiente para ser aceita). Se qualquer uma piora, o número cai.
- **É a unidade de valor entregue**, não de atividade. Uma sugestão *mostrada* não vale
  nada; uma sugestão *aceita* economizou digitação de verdade.
- **"Tempo economizado" é derivado dela.** Ótimo para o usuário enxergar valor, mas fraco
  como métrica de direção: é uma estimativa (palavras × tempo médio por palavra).

### O que NÃO usar como North Star
- **Downloads** — vaidade. Mede curiosidade, não valor entregue nem retenção.
- **Sugestões mostradas** — incentiva ser barulhento, não útil.
- **Tempo economizado** — derivado e estimado; bom como narrativa, ruim como bússola.

---

## 2. O funil

A ordem importa: cada degrau é um ponto de morte clássico para apps deste tipo
(autocomplete + acessibilidade no nível de sistema).

| # | Etapa | O que é | Onde mede-se hoje |
|---|-------|---------|-------------------|
| 1 | **Aquisição** | Download do app | ✅ GitHub releases (`stats.html` / `history.json`) |
| 2 | **Ativação técnica** | Conceder Accessibility + Input Monitoring depois do aviso do Gatekeeper | ⚠️ Qualitativo / survey |
| 3 | **Aha moment** | Primeira palavra aceita (primeiro <kbd>Tab</kbd>) | ⚠️ Local / qualitativo |
| 4 | **Hábito** | Continua **ligado** (vs. pausado/desligado); streak diário | ⚠️ Local / proxy de update |
| 5 | **Qualidade** | Acceptance rate; calibração da confiança (o fade prevê o accept?) | ⚠️ Local |
| 6 | **Confiança / saúde** | Não desinstala, não revoga permissão, não dá crash | ⚠️ Qualitativo |

### O paredão real: etapa 2
A maior perda **não** é a qualidade da sugestão — é o caminho até a primeira sugestão:
- App self-signed → aviso *"could not verify"* → exige ir em System Settings → "Open Anyway".
- Duas permissões TCC (Accessibility + Input Monitoring) concedidas manualmente.

Cada fricção aqui derruba gente que nunca chegou a ver o produto funcionar. **Reduzir essa
fricção tende a ter mais impacto do que melhorar o modelo.**

---

## 3. O insight que já está nas suas mãos: auto-update como proxy de retenção

Sem telemetria, parece impossível medir retenção. Mas o **Sparkle bate no `appcast.xml`**,
e a **contagem de downloads do `.zip` por versão ao longo do tempo** é um proxy de retenção
que respeita 100% a privacidade.

Quem continua puxando updates semanas após instalar **ainda está usando**. Daí dá pra extrair:

- **Instalações ativas estimadas** ≈ quantos baixaram a versão N nos primeiros X dias.
- **Retenção** ≈ pessoas que adotam versões lançadas em janelas separadas no tempo.
- **DMG vs ZIP**: o `.dmg` é instalação humana **nova** (aquisição); o `.zip` é
  auto-update de quem **já tem** (base existente). Hoje o `history.json` mistura os dois —
  separá-los distingue *crescimento novo* de *base retida*.

> Detalhamento de como evoluir o stats para isso está em `docs/METRICS-IDEIAS.md` (Ideia 1).

---

## 4. O conjunto enxuto para acompanhar

Não uma laundry list — uma North Star + poucos guardrails. Mais métricas = menos foco.

| Camada | Métrica | Como medir hoje (sem telemetria) | Status |
|--------|---------|----------------------------------|--------|
| **North Star** | Palavras aceitas / usuário ativo / semana | Dashboard local + survey/qualitativo | 🔴 indireto |
| Aquisição | Downloads novos (**DMG**) por semana | GitHub releases | 🟡 dado existe, falta separar |
| Retenção | Curva de adoção de update (**ZIP**) | Derivável do `appcast` | 🔴 falta construir |
| Ativação | % que passa permissões → 1ª aceitação | Survey / entrevistas | 🔴 qualitativo |
| Qualidade | Acceptance rate; calibração do fade | Local (+ opt-in agregado, se um dia) | 🔴 local |
| Confiança | Motivos de desinstalação | Form de uninstall / emails | 🔴 qualitativo |

---

## 5. Definições (para não medir coisas diferentes com o mesmo nome)

- **Usuário ativo (semana):** instância que gerou ≥1 sugestão aceita na semana.
  (Conceitual — só observável localmente ou via opt-in.)
- **Palavra aceita:** token confirmado pelo usuário via <kbd>Tab</kbd> (palavra a palavra)
  ou via aceitar-linha-inteira. Aceitar a linha conta todas as palavras da linha.
- **Acceptance rate:** palavras aceitas ÷ palavras mostradas. Mede qualidade da predição.
- **Calibração da confiança:** correlação entre o nível do *fade* (confiança exibida) e a
  probabilidade real de aceitação. *"The fade is the confidence"* só é verdade se calibrado.
- **Instalação nova:** download de `.dmg`.
- **Auto-update:** download de `.zip` (Sparkle).
- **Instalação ativa estimada:** instalações que adotaram a versão N dentro de X dias do
  lançamento (proxy — ver Ideia 1).
- **Streak:** dias consecutivos com ≥1 palavra aceita. (Local; serve de engajamento.)

---

## 6. Anti-métricas (cuidado)

- **Total acumulado de downloads** — só cresce, nunca informa. Pior tipo de vaidade.
- **Tempo economizado em horas** — ótimo para marketing, perigoso como meta: infla com
  qualquer mudança na estimativa de "tempo por palavra".
- **Nº de apps suportados** — alcance importa, mas suportar mais apps sem que ninguém os
  use é esforço sem valor. Pondere sempre pelo uso real.

---

## 7. Perguntas em aberto

1. Vale instrumentar telemetria opt-in agregada para enxergar a North Star de verdade,
   ou o custo de marca não compensa? (ver `docs/METRICS-IDEIAS.md`, Ideia 3)
2. Qual é o maior gargalo do funil hoje — etapa 2 (permissões) ou etapa 5 (qualidade)?
   Sem ativação, melhorar qualidade não move nada.
3. Dá pra estimar instalações ativas com confiança só a partir do `appcast`, ou o ruído
   de bots/scrapers nos downloads contamina demais?
