# Ideias de medição — para pensar depois

> Companheiro do `METRICS.md`. Aqui ficam as **duas ideias acionáveis** que não
> precisam de decisão agora, mas que destravam visibilidade real do produto.
> Nada aqui está decidido — é material de reflexão.

---

## Ideia 1 — Separar DMG vs ZIP e construir a curva de retenção

### O problema
Hoje o `stats/history.json` agrega **downloads brutos por asset, por dia**. Isso mistura
duas coisas que querem dizer coisas opostas:

- `.dmg` → **instalação humana nova** (alguém baixou e arrastou para Applications) = **aquisição**.
- `.zip` → **auto-update do Sparkle** (quem já tem o app puxando a nova versão) = **base retida**.

Somados, viram um número de vaidade que só cresce. Separados, contam a história real:
*estou ganhando gente nova? E a base antiga continua aqui?*

### O que dá pra extrair (sem telemetria, só do que já existe)
1. **Aquisição nova por semana** = soma dos downloads de `.dmg` na janela.
2. **Adoção de update** = velocidade com que a versão N é puxada via `.zip` após o release.
   Curva íngreme = base ativa e saudável; curva rasa = gente parou de usar.
3. **Instalações ativas estimadas** = nº de `.zip` da versão N baixados nos primeiros X dias
   (ex.: 7 ou 14). É um proxy: quem auto-atualiza ainda está com o app rodando.
4. **Retenção por coorte** = cruzar quem adotou versões lançadas em janelas separadas no tempo.

### Esboço de implementação
- **`history.json`**: ao snapshotar, classificar cada asset por extensão (`.dmg` / `.zip`) e
  guardar os dois agregados separados, além do total. Manter compatibilidade com os snapshots
  antigos (campos novos, não renomear os existentes).
- **`stats.html`**: dois gráficos em vez de um — "Instalações novas (DMG)" e "Adoção de
  update / ativos estimados (ZIP)". Manter o estilo monocromático *"the fade is the confidence"*.
- **Higiene de dados**: downloads de release no GitHub incluem bots/scrapers/CDNs.
  Antes de chamar de "ativos", checar a ordem de grandeza e, se possível, marcar o ruído.

### Por que vale
É **retenção real, derivada de dados que você já coleta, sem tocar na privacidade.**
Provavelmente o maior retorno por esforço de toda a lista.

### Riscos / coisas a validar
- Contagem de download do GitHub conta re-downloads e bots — é proxy, não verdade.
- Sparkle pode baixar o `.zip` em background mesmo sem o usuário "usar" o app naquele dia.
- Delta-updates (se um dia adotados) mudam o que um download de `.zip` significa.

---

## Ideia 3 — Telemetria opt-in, anônima e agregada (só se valer a pena)

### A tensão
A North Star ("palavras aceitas por usuário ativo por semana") só é **diretamente** visível
com algum sinal vindo da máquina do usuário. Mas o pilar de marca é *"no telemetry, no server."*
Qualquer instrumentação **arranha** essa promessa — e, em produto de privacidade, confiança
perdida é caríssima de recuperar.

> Premissa não-negociável: se isso existir, é **opt-in explícito** (default OFF), **agregado**,
> **sem nenhum conteúdo** do que o usuário digitou, e auditável.

### Princípios de design (se um dia for feito)
1. **Default OFF.** Liga só quem escolhe ativamente, com texto claro do que é enviado.
2. **Nunca conteúdo.** Jamais texto digitado, sugerido, nomes de app específicos sensíveis,
   ou qualquer coisa reconstruível. Só **contadores**.
3. **Só agregados pré-computados na máquina.** Enviar números já somados (ex.: "X palavras
   aceitas esta semana"), não eventos individuais.
4. **Sem identidade.** Sem conta, sem device-id estável. No máximo um id rotativo por janela,
   ou nada — contar instalações distintas importa menos que enxergar a tendência agregada.
5. **Privacidade diferencial / k-anonimato.** Ruído nos contadores e supressão de buckets
   pequenos, para que nenhum envio individual diga nada sobre uma pessoa.
6. **Transparente e auditável.** Payload visível ao usuário antes de enviar; idealmente
   documentado no README e no app. "Veja exatamente o que sai."
7. **Kill switch real.** Desligar para de enviar imediatamente e apaga o que for local.

### Qual seria o conjunto mínimo de contadores (exemplo)
Apenas o suficiente para ver a North Star e o topo do funil — nada além:
- Palavras aceitas no período (bucketizado, com ruído).
- Sugestões mostradas no período (para derivar acceptance rate).
- App está ligado vs. pausado (engajamento, sem dizer em qual app).
- Versão do app + versão do macOS (para entender base instalada / Apple Intelligence vs Ollama).

### Alternativas mais leves (antes de construir um servidor)
- **Survey opt-in no app** ("ajude o experimento — responda 3 perguntas"), one-shot,
  sem coleta contínua. Captura ativação e satisfação sem infra.
- **Form de desinstalação** ("por que está saindo?") — o sinal de confiança mais barato.
- **Canal qualitativo** (email/feedback no menu) tratado como fonte de métrica, não só suporte.

### A pergunta de fundo (decidir depois, com calma)
**O ganho de resolução compensa o custo de marca?** Para um *"free personal experiment"*
focado em privacidade, talvez a resposta honesta seja **não** — e o caminho seja
maximizar a Ideia 1 (proxies) + qualitativo, mantendo "there is no server" literalmente verdadeiro.

Se um dia a resposta virar "sim", o design acima é o piso mínimo de respeito ao usuário.

---

## Resumo de uma linha
- **Ideia 1**: alto retorno, baixo risco, faz agora-ish — proxies de retenção a partir do `appcast`.
- **Ideia 3**: alto risco de marca, faz só com convicção — telemetria opt-in agregada, ou nem isso.
