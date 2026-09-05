# Slash commands i Claude Code — referenceoversigt

**Verificeret:** 5. september 2026
**CLI-version brugt som anker:** **Claude Code v2.1.261** — nyeste udgivelse øverst i den officielle changelog ([CHANGELOG.md, `anthropics/claude-code@main`](https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md))
**Desktop-appens versionsnummer:** **[ikke verificeret]** — se [Versionsforankring](#versionsforankring) nedenfor.

---

## Versionsforankring

| Spørgsmål | Svar | Kilde |
|---|---|---|
| Nyeste CLI-version | **2.1.261** | [CHANGELOG.md](https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md) |
| Nyeste desktop-app-version | **[ikke verificeret]** | Anthropic publicerer ikke desktop-appens versionsnummer i dokumentationen. Docs nævner kun *minimumskrav*, fx “require Claude Desktop **v1.2581.0** or later” for pane-layout, terminal og fileditor ([desktop](https://code.claude.com/docs/en/desktop)) |
| Hvordan brugeren selv aflæser det | Desktop: **macOS** → Claude-menuen → **About Claude**; **Windows** → **Help** → **About**. Klik på versionsnummeret for at kopiere det. CLI: `claude --version` | [desktop → Check your version](https://code.claude.com/docs/en/desktop) |

**Vigtigt om desktop-appen:** Desktop-appen har tre faner — **Chat**, **Cowork** og **Code**. Kun **Code**-fanen er Claude Code. Den kører *den samme motor* som CLI'en og læser de samme konfigurationsfiler, så kommando-*sættet* er grundlæggende identisk med CLI'ens; det er **adfærden** for dialogbaserede kommandoer der afviger. Se [Appendiks A](#appendiks-a--forskelle-desktop-vs-cli-vs-web).

> **Kildekritisk forbehold — læs dette først.**
> Den officielle kommandotabel på [code.claude.com/docs/en/commands](https://code.claude.com/docs/en/commands) er så lang, at hentningsværktøjet i denne session afkortede siden (`[Content truncated due to length...]`) ved gentagne forsøg. Rækkerne gengivet nedenfor er hentet verbatim over to hentninger der tilsammen dækkede tabellen fra `/add-dir` til `/workflow`, og de er krydstjekket mod andre dokumentationssider hvor det var muligt. **Jeg kan ikke garantere at listen er 100 % udtømmende.** Konkret er mindst fem kommandoer dokumenteret på *andre* officielle sider uden at kunne bekræftes i kommandotabellen — se [Ikke-verificerede punkter](#ikke-verificerede-punkter). Din egen `/help`-output er den autoritative liste for netop din version og dit abonnement.

---

## 1. Oversigtstabel — alle kommandoer

Kolonnen **Overflade** følger de dokumenterede regler (se [Appendiks A](#appendiks-a--forskelle-desktop-vs-cli-vs-web)), ikke en per-kommando-matrix fra Anthropic — en sådan matrix findes ikke i dokumentationen.
Forkortelser: **C** = CLI, **D** = Desktop (Code-fanen), **W** = Web (cloud), **I** = IDE-extension.
`[S]` = bundled skill · `[F]` = bundled workflow · `[H]` = skjult i menuen indtil du skriver navnet fuldt ud.

| Kommando | Formål (én linje) | Overflade |
|---|---|---|
| `/add-dir <path>` | Tilføj en ekstra arbejdsmappe til sessionen | C · D · W · I |
| `/advisor [model\|off]` | Slå rådgiver-værktøjet til/fra (konsulterer en anden model undervejs) | C · D · W |
| `/agents` | Printer en påmindelse om at bede Claude oprette/administrere subagents | C · D · W · I |
| `/artifacts` | List, vedhæft, åbn eller kopiér link til dine artifacts | C · D · W |
| `/auto-mode-setup` | Udkast til `autoMode.environment` ud fra projekt og seneste sessioner | C |
| `/autocompact [auto\|<tokens>]` | Sæt auto-compact-vinduet (hvor fyldt konteksten bliver før auto-komprimering) | C · D · W |
| `/autofix-pr [prompt]` | Start en web-session der overvåger branchens PR og pusher fixes | C |
| `/background [prompt]` | Frigør terminalen ved at køre sessionen videre som baggrundsagent (alias `/bg`) | C |
| `/batch <instruction>` `[S]` | Parallelisér store ændringer over 5–30 subagents i hver sin git-worktree | C · D · W |
| `/branch [name]` | Forgren samtalen her og skift over i forgreningen | C · D · W |
| `/btw [question]` | Stil et sidespørgsmål uden at tilføje til samtalen | C · D · W |
| `/bug [report]` | Rapportér en fejl / del samtalen (alias `/share`) | C · D · W · I |
| `/cd <path>` | Flyt sessionen til en ny arbejdsmappe, behold samtalen | C · D · W |
| `/chrome` | Konfigurér Claude in Chrome | C · D |
| `/claude-api [migrate\|upgrade\|managed-agents-onboard\|prompt-audit\|cost-optimize]` `[S]` | Indlæs Claude API-/Managed Agents-referencemateriale for projektets sprog | C · D · W |
| `/clear [name]` | Start ny samtale med tom kontekst (alias `/reset`, `/new`) | C · D · **ikke W** |
| `/code-review [level] [--fix] [--comment] [mål]` `[S]` | Gennemgå diff/PR/branch/sti for korrekthedsfejl og oprydning (alias `/review`) | C · D · W |
| `/color [color\|default]` | Sæt promptbarens farve for sessionen | C · D · W |
| `/compact [instructions]` | Frigør kontekst ved at opsummere samtalen indtil nu | C · D · W |
| `/config [key=value ...]` | Åbn indstillinger, eller sæt en værdi direkte (alias `/settings`) | C · D\* · W\* |
| `/context [all]` | Visualisér kontekstforbrug som farvet gitter | C · D · W |
| `/copy [N]` | Kopiér seneste (eller N'te-seneste) svar til udklipsholder | C · D · W |
| `/cost` | Alias for `/usage` | C · D · W |
| `/dataviz [request]` `[S]` | Designvejledning til grafer, diagrammer og dashboards | C · D · W |
| `/debug [description]` `[S]` | Slå debug-logning til og fejlsøg ud fra sessionens debug-log | C · D · W |
| `/deep-research <question>` `[F]` | Fan-out websøgninger, krydstjek kilder, syntetisér en citeret rapport | C · D · W |
| `/design [brief]` `[S]` | Tegn UI-mockups/flows/plakater som artboards på ét canvas i en artifact | C · D · W |
| `/design-login` | Autorisér design-system-adgang for `/design-sync` | C · D · W |
| `/design-sync [hint]` `[S]` | Konvertér repoets React-designsystem og upload det til Claude Design | C · D · W |
| `/desktop` | Fortsæt den aktuelle session i desktop-appen (alias `/app`) | C |
| `/diff` | Gennemgå ændringerne i working tree, inkl. Claudes egne edits | C · D · W |
| `/doctor` `[S]` | Opsætningstjek der diagnosticerer og kan reparere (alias `/checkup`) | C · D · W |
| `/effort [level\|auto\|status]` | Sæt effort-niveau (`low`…`xhigh`, `max`, `ultracode`, `auto`) | C · D · W |
| `/exit` | Afslut CLI'en (alias `/quit`) | C |
| `/export [filename]` | Eksportér samtalen som ren tekst | C · D · W |
| `/fast [on\|off]` | Slå fast mode til/fra | C · D · W |
| `/feedback [report]` | Send produktfeedback om Claude Code | C · D · W · I |
| `/fewer-permission-prompts` `[S]` | Byg en allowlist ud fra dine transskripter for at mindske tilladelsesprompter | C · D · W |
| `/focus` | Slå fokus-visning til/fra (kun sidste prompt, værktøjsresumé, slutsvar) | C (fullscreen) · I\* |
| `/fork [prompt]` | Kopiér samtalen til en ny baggrundssession og arbejd videre her | C |
| `/goal [condition\|clear]` | Sæt et mål som Claude arbejder mod på tværs af ture | C · D · W |
| `/heapdump` `[H]` | Skriv heap-snapshot + hukommelsesopgørelse til `~/Desktop` | C |
| `/help` | Vis hjælp og tilgængelige kommandoer | C · D · W · I |
| `/hooks` | Se hook-konfigurationer for tool-events | C · D · W |
| `/ide` | Administrér IDE-integrationer og vis status | C |
| `/import [codex\|gemini] [--dry-run] [--yes]` | Importér konfiguration fra OpenAI Codex / Google Gemini CLI | C |
| `/init` | Initialisér projektet med en `CLAUDE.md`-guide | C · D · W |
| `/insights` | Generér HTML-rapport om dine seneste sessioner på denne maskine | C · D · **ikke W** |
| `/install-github-app` | Installér Claude GitHub App for et repository | C · D |
| `/install-slack-app` | Installér Claude Slack-appen via OAuth i browseren | C · D |
| `/keybindings` | Åbn din keyboard-shortcuts-fil | C · D |
| `/list-agents` | List subagents, teammates og andre sessioner Claude kan skrive til (alias `/peers`) | C · D · W |
| `/login` | Log ind på din Anthropic-konto | C · D · W |
| `/logout` | Log ud af din Anthropic-konto | C · D · W |
| `/loop [interval] [prompt]` `[S]` | Kør en prompt gentagne gange mens sessionen er åben (alias `/proactive`) | C · D · W |
| `/mcp [reconnect\|enable\|disable …]` | Administrér MCP-serverforbindelser og OAuth-godkendelse | C · D · W |
| `/memory` | Redigér `CLAUDE.md`-filer, slå auto-memory til/fra, se auto-memory-poster | C · D · W |
| `/mobile` | Vis QR-kode til download af Claude-mobilappen (alias `/ios`, `/android`) | C · D |
| `/model [model]` | Skift model og gem som standard for nye sessioner | C · D · W |
| `/passes` | Del en gratis uge med Claude Code — kun synlig hvis kontoen er berettiget | C · D · W |
| `/permissions` | Administrér allow/ask/deny-regler for værktøjstilladelser (alias `/allowed-tools`) | C · D\* · W\* |
| `/plan [description]` | Gå direkte i plan mode, evt. med en opgave med det samme | C · D · W |
| `/plugin [subcommand]` | Administrér plugins (`list`, `install`, `enable`, `disable` …) | C · D\*\* · **ikke W** |
| `/powerup` | Lær Claude Code-features via korte interaktive lektioner | C |
| `/pr-comments [PR]` | **Fjernet i v2.1.91** — bed i stedet Claude direkte om at se PR-kommentarer | — |
| `/privacy-settings` | Se og opdatér privatlivsindstillinger — kun Pro/Max | C · D · W |
| `/radio` | Åbn Claude FM lo-fi-radio i browseren | C · D |
| `/release-notes` | Vis hvad der er nyt i Claude Code | C · D · W |
| `/reload-plugins` | Genindlæs alle installerede plugins uden genstart | C · D · W |
| `/remote-control` | Fortsæt denne lokale session fra en anden enhed | C |
| `/resume` | Vend tilbage til en tidligere samtale | C · D · **ikke W** |
| `/rewind` | Rul kode og samtale tilbage til et checkpoint | C · D · W |
| `/run <command>` `[S]` | Kør en shell-kommando eller et script og opfang outputtet | C · D · W |
| `/schedule [beskrivelse\|list\|update\|run]` | Opret og administrér routines (planlagte cloud-kørsler) — alias `/routines` | C · D · **ikke W** |
| `/security-review [--fix] [mål]` `[S]` | Gennemgå diff/PR/branch/sti for sikkerhedssårbarheder | C · D · W |
| `/simplify [--fix] [file\|path]` `[S]` | Forenkl kode for læsbarhed og vedligeholdbarhed | C · D · W |
| `/skills` | Administrér skills: se, opret, redigér, aktivér, deaktivér, slet | C · D · W |
| `/status` | Vis model, effort-niveau, arbejdsmappe og sessionsstatus | C · D · W |
| `/subtask <instruction>` | Overdrag en sideopgave til en subagent der rapporterer tilbage i samtalen | C · D · W |
| `/tasks` | List baggrundsarbejde i sessionen (subagents, baggrundssessioner) | C · D · W |
| `/teleport` | Træk en web-session ind i denne terminal (alias `/tp`) | C |
| `/terminal-setup` `[S]` | Konfigurér shell med Claude Code-completions og aliases | C |
| `/theme [theme\|default]` | Åbn temavælger eller sæt tema direkte (`dark`, `light`, `auto`) | C · D · W |
| `/todos` `[S]` | Find TODO-kommentarer i kodebasen og opsummér dem pr. fil og status | C · D · W |
| `/upgrade` | Opgradér Claude Code til nyeste version på din release-kanal | C |
| `/usage` | Vis API-forbrug, omkostning og rate limits (alias `/cost`) | C · D · W |
| `/verify` `[S]` | Kør en verifikationskommando (som standard `npm test` eller `pytest`) | C · D · W |
| `/vim` | Åbn `vim`/`nano` for at redigere en fil, med resultatet i kontekst | C |
| `/web-setup` | Synkronisér din lokale `gh`-token til Claude-kontoen for cloud-sessioner | C |
| `/workflow [list\|enable\|disable] [<name>\|all]` | Administrér dynamiske workflows | C · D · W |

\* `/config` og `/permissions` opfører sig anderledes uden for terminalen — se [Appendiks A](#appendiks-a--forskelle-desktop-vs-cli-vs-web).
\*\* Desktop bruger en grafisk plugin-manager i stedet for `/plugin`-dialogen.
\*\*\* VS Code-extensionen har sin egen Focus-toggle i kommandomenuen, gemt som en extension-indstilling uafhængigt af `viewMode`.

**Kommandoer dokumenteret uden for kommandotabellen** (bekræftet på deres egne feature-sider, men ikke bekræftet som rækker i tabellen): `/schedule` · `/routines` · `/web-setup` · `/rename` · `/skill-doctor` · `/tui` · `/voice`. Se [Ikke-verificerede punkter](#ikke-verificerede-punkter).

**Hovedkilder til tabellen:** [Commands](https://code.claude.com/docs/en/commands) · [Skills](https://code.claude.com/docs/en/skills) · [Interactive mode](https://code.claude.com/docs/en/interactive-mode) · [Desktop application](https://code.claude.com/docs/en/desktop) · [Claude Code on the web](https://code.claude.com/docs/en/claude-code-on-the-web) · [Routines](https://code.claude.com/docs/en/routines)

---

## 2. Session og kontekst

Kilde for hele afsnittet: [Commands](https://code.claude.com/docs/en/commands), suppleret hvor angivet.

### `/clear [name]`
- **Argumenter:** valgfrit navn, der mærker den forrige samtale i `/resume`-vælgeren.
- **Gør:** starter en ny samtale med tom kontekst.
- **Overflade:** CLI, Desktop. **Ikke tilgængelig i web-sessioner** — start en ny session fra sidebaren i stedet ([web](https://code.claude.com/docs/en/claude-code-on-the-web)).
- **Eksempel:** `/clear auth-refaktorering`
- **Faldgrube:** rydder konteksten helt. Vil du kun frigøre plads og fortsætte samme samtale, brug `/compact`.
- **Relateret:** `/compact`, `/resume`, rewind-menuens “previous session”-post (kræver v2.1.191+).
- **Aliaser:** `/reset`, `/new`.

### `/compact [instructions]`
- **Argumenter:** valgfri fokusinstruktion til opsummeringen.
- **Gør:** frigør kontekst ved at opsummere samtalen indtil nu.
- **Overflade:** CLI, Desktop, Web.
- **Eksempel:** `/compact behold testoutputtet`
- **Faldgrube:** i Agent SDK/`-p` returnerer `/compact` blot en begrundelse (fx `Not enough messages to compact.`) hvis der ikke er nok historik — det er ikke en fejl ([SDK](https://code.claude.com/docs/en/agent-sdk/slash-commands)).
- **Relateret:** `/autocompact`, `/context`, env-var `CLAUDE_CODE_AUTO_COMPACT_WINDOW`.

### `/autocompact [auto|<tokens>]`
- **Argumenter:** en størrelse som `500k`, eller `auto` for modellens indstillede vindue. Uden argument åbnes en dialog med det aktuelle vindue.
- **Gør:** sætter hvor fyldt kontekstvinduet bliver, før Claude Code komprimerer automatisk.
- **Krav:** **v2.1.221+**. Værdien gemmes i user settings og anvendes på den aktuelle session.
- **Faldgrube:** i web-sessioner sætter Claude Code selv `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE`, og den værdi **overskriver** en du selv lægger i miljøvariablerne. Brug `CLAUDE_CODE_AUTO_COMPACT_WINDOW` eller `/autocompact` i stedet ([web](https://code.claude.com/docs/en/claude-code-on-the-web)).

### `/context [all]`
- **Argumenter:** `all` udvider per-element-opdelingen i fullscreen.
- **Gør:** visualiserer kontekstforbrug som et farvet gitter med optimeringsforslag.
- **Faldgrube:** når samtalen overskrider kontekstvinduet, viser outputtet hvor langt over grænsen du er, og hvilken kommando der frigør plads.

### `/resume`
- **Argumenter:** samtalenavn eller -nummer; uden argument åbnes en vælger.
- **Krav:** argumentformen kræver **v2.1.212+**.
- **Overflade:** ikke tilgængelig i web-sessioner (kun terminal-UI).
- **Relateret:** `/clear [name]` navngiver samtaler til vælgeren; `--resume` er CLI-flaget. `--teleport` er noget andet — det henter *cloud*-sessioner.

### `/branch [name]` · `/fork [prompt]` · `/background [prompt]` · `/subtask <instruction>`
Fire måder at dele arbejdet op på. Vælg efter hvor du selv vil være bagefter:

| Kommando | Hvad der sker | Hvor ender du |
|---|---|---|
| `/branch [name]` | Forgrener samtalen på dette punkt; originalen bevares | **I forgreningen** (tilbage via `/resume`) |
| `/fork [prompt]` | Kopierer samtalen til en ny **baggrundssession** | Bliver hvor du er |
| `/background [prompt]` | Frigør terminalen; **denne** session kører videre som baggrundsagent | Terminalen frigives |
| `/subtask <instruction>` | Giver en sideopgave til en subagent der **rapporterer tilbage i samtalen** | Bliver hvor du er |

- `/fork` kræver **v2.1.212+**. På v2.1.161–v2.1.211, og når agent view er slået fra, starter `/fork` i stedet en *forked subagent*. Isolationsinstruktionen (at kopien laver sin egen worktree før kodeændringer) kræver **v2.1.221+**.
- `/background` har aliasset `/bg`; overvåg med `claude agents`.

### `/rewind`
- **Gør:** ruller kode og samtale tilbage til et checkpoint, eller springer til et tidligere punkt og opsummerer det oversprungne.
- **Genvej:** dobbelt-`Esc` på tom prompt åbner samme menu ([interactive-mode](https://code.claude.com/docs/en/interactive-mode)).
- **Relateret:** [Checkpointing](https://code.claude.com/docs/en/checkpointing).

### `/export [filename]` · `/copy [N]`
- `/export` uden filnavn åbner en dialog (kopiér til udklipsholder eller gem); med filnavn skrives direkte.
- `/copy 2` kopierer det næstsidste svar. Er der kodeblokke, vises en vælger; tryk **`w`** i vælgeren for at skrive til fil i stedet for udklipsholder — nyttigt over SSH.

### `/btw [question]`
- **Gør:** stiller et sidespørgsmål om sessionen uden at tilføje til samtalen.
- **Faldgrube/ændring:** uden spørgsmål vises dit seneste sidespørgsmål, så du kan bladre i tidligere svar. **Før v2.1.212 krævede `/btw` et spørgsmål.**
- **Genvej:** `Cmd+;` åbner side-chat i desktop-appen.

### `/goal [condition|clear]`
- **Gør:** sætter et mål Claude arbejder mod på tværs af ture, indtil betingelsen er opfyldt.
- **Ryd med:** `clear`, `stop`, `off`, `reset`, `none` eller `cancel`. Uden argument vises det aktuelle/senest opnåede mål.

### `/plan [description]`
- **Gør:** går i plan mode direkte fra prompten, evt. med opgaven med det samme: `/plan fix the auth bug`.
- **Relateret:** `Shift+Tab` cykler tilstande; CLI-flaget `--permission-mode plan`.

### `/tasks` · `/focus` · `/diff` · `/exit`
- `/tasks`: lister baggrundsarbejde, viser status og fejl, og lader dig attache til en kørende subagent eller baggrundssession. Tryk **`t`** for at teleporte ind i en cloud-session. Kører **med det samme** selv mens Claude svarer.
- `/focus`: viser kun sidste prompt, et etlinjes værktøjsresumé med diffstats, og slutsvaret. **Kun i fullscreen-rendering.** Valget huskes på tværs af sessioner; overstyr med `viewMode` i settings.
- `/diff`: gennemgår ændringer i working tree. **Tilføjet i v2.1.260** til fullscreen-visning.
- `/exit` (alias `/quit`): i en attached baggrundssession **detacher** den blot — sessionen kører videre.

### `/teleport` · `/remote-control` · `/desktop` · `/list-agents`
Kilde: [Claude Code on the web](https://code.claude.com/docs/en/claude-code-on-the-web), [desktop](https://code.claude.com/docs/en/desktop).

- **`/teleport`** (alias `/tp`): åbner sessionvælgeren og trækker en cloud-session ind i terminalen — inkl. branch og fuld samtalehistorik. Krav: ren git-state, samme repository (ikke en fork), branch pushet til remote, samme claude.ai-konto. Kører du `/teleport` *inde i* en cloud-session, svarer Claude Code med den præcise `claude --teleport <session-id>`-kommando (kræver **v2.1.223+** i sessionens miljø).
- **`/remote-control`**: eksponerer den lokale session, så du kan styre den videre fra en anden enhed. Bemærk: `--cloud` og `--remote-control` er **ikke** det samme — `--cloud` opretter cloud-sessioner.
- **`/desktop`** (alias `/app`): gemmer sessionen, åbner den i desktop-appen og afslutter CLI'en. **Kun macOS og x64 Windows med et Claude-abonnement.** Ikke tilgængelig med API-nøgle-autentificering eller på Amazon Bedrock, Google Cloud's Agent Platform eller Microsoft Foundry.
- **`/list-agents`** (alias `/peers`): kræver **v2.1.224+** (tidligere versioner svarer `Unknown command: /list-agents`). Teammate-rækker og linjen med sessionens eget navn kræver **v2.1.239+**. Kun i sessioner hvor cross-session messaging er slået til.

---

## 3. Konfiguration og model

### `/config [key=value ...]`
- **Argumenter:** ét eller flere `key=value`-par. Kør `/config --help` for at se accepterede nøgler.
- **Gør:** åbner indstillinger, eller sætter en værdi direkte uden at åbne panelet.
- **Version:** `key=value` fra **v2.1.181**; navngivne kortformer (`theme=dark`, `model=sonnet`) fra **v2.1.182**.
- **Faldgruber:**
  - `key=value` kan **ikke slå en indstilling til** der kræver din bekræftelse i panelet (fx `autoContinueAtUsageLimit`) — men kan godt slå den fra.
  - **I desktop-appen og på web** åbner `/config` blot Settings → Claude Code, og **tekst efter kommandoen ignoreres**: `/config theme=dark` sætter ikke temaet.
- **Overflade:** `key=value`-formen virker også i `-p` og fra Claude-mobilappen via Remote Control.
- **Alias:** `/settings`.

### `/model [model]`
- **Argumenter:** modelnavn; uden argument åbnes en vælger.
- **Gør:** skifter model og gemmer den som standard for nye sessioner.
- **Detaljer:** venstre/højre piletast justerer effort-niveau for modeller der understøtter det; tryk **`s`** på en række for kun at skifte i den aktuelle session.
- **Version:** i `-p` (ikke-interaktiv) tager den et modelargument i stedet for vælgeren, gælder kun sessionen og gemmes ikke som standard — kræver **v2.1.205+**. Før **v2.1.242** afgjorde et feature-flag om kommandoen kunne køre midt i en tur; på tredjepartsudbydere blev den altid sat i kø.
- **Relateret:** `--model`-flaget, `ANTHROPIC_MODEL`, hook-events `PreModelSwitch`/`PostModelSwitch` (tilføjet i **2.1.251**).

### `/effort [level|auto|status]`
- **Argumenter:** `low` … `xhigh`, `max`, `ultracode`, `auto`, eller `status`.
- **Detaljer:** `max` og `ultracode` er session-only; `ultracode`-nøglen persisterer. Sessionsspecifikt effort-skift med **`s`** blev tilføjet i **2.1.257**.
- **Faldgruber:** virker i `-p` **uden for** effort-hold. I web-sessioner rapporterer `/effort` `Not applied` mens en models launch-default effort-hold er i kraft.

### `/fast [on|off]`
- **Gør:** slår fast mode til/fra. Den kørende tur afsluttes i sin oprindelige hastighed.
- **Krav:** **v2.1.205+**. I web-sessioner virker den **kun i en session der startede med fast mode slået til**.
- **Genvej:** `Option+O` (macOS) / `Alt+O` (Windows/Linux).

### `/advisor [model|off]`
- **Argumenter:** `fable`, `opus`, `sonnet`, et fuldt model-ID, eller `off`. Uden argument åbnes en vælger.
- **Gør:** konsulterer en anden model for vejledning på nøglepunkter i en opgave.
- **Krav:** `fable` kræver Fable-adgang. En tekstform til desktop og headless blev tilføjet i **2.1.260**.

### `/theme [theme|default]` · `/color [color|default]` · `/status`
- `/theme`: `dark`, `light`, `auto`, `default`. Synkroniseres til claude.ai/code når Remote Control er forbundet. Virker i `-p` (**v2.1.205+**).
- `/color`: `red`, `blue`, `green`, `yellow`, `purple`, `orange`, `pink`, `cyan`; uden argument vælges en tilfældig. Synkroniseres også via Remote Control. Virker i `-p` (**v2.1.205+**).
- `/status`: viser model, effort-niveau, arbejdsmappe og sessionsstatus. **2.1.261** tilføjede en linje med organisationens policy-status (også i `claude doctor`). Kører **med det samme** mens Claude svarer.

### `/memory` · `/init` · `/hooks` · `/keybindings`
- `/memory`: redigér `CLAUDE.md`-filer, slå auto memory til/fra, se auto-memory-poster.
- `/init`: opretter `CLAUDE.md`. Sæt `CLAUDE_CODE_NEW_INIT=1` for et interaktivt flow der også gennemgår skills, hooks og personlige memory-filer. Finder `/init` konfiguration fra en coding agent som `/import` understøtter, tilbyder den at overføre den.
- `/hooks`: viser hook-konfigurationer for tool-events. Se [Hooks](https://code.claude.com/docs/en/hooks).
- `/keybindings`: åbner `~/.claude/keybindings.json`. Bemærk: readline-genvejene (`Alt+B`, `Ctrl+W` m.fl.) kan **ikke** ombindes — der findes ingen actions for dem.

---

## 4. Filer og kode

### `/add-dir <path>` vs. `/cd <path>`
Begge udvider hvor Claude må arbejde, men forskelligt:

| | `/add-dir <path>` | `/cd <path>` |
|---|---|---|
| Effekt | **Tilføjer** en arbejdsmappe til sessionen | **Flytter** sessionen, samtalen bevares |
| `.claude/`-konfiguration | Det meste opdages **ikke** fra den tilføjede mappe | Anvendes fra den nye mappe |
| Version | Sti-forslag med `Tab`; kører nu straks midt i en tur (**før v2.1.234** blev den sat i kø) | Kræver **v2.1.169+**; sti-forslag kræver **v2.1.206+** |

- **Faldgrube ved `/add-dir`:** de fleste netværksstier (fx `\\server\share`) kan ikke tilføjes. Efter en vellykket tilføjelse kører dine `DirectoryAdded`-hooks. Kører du den midt i en tur, beder Claude Code dig bekræfte straks, og Claudes næste tool-kald i samme tur kan bruge mappen.
- **Relateret:** `--add-dir`-flaget, `additionalDirectories`-indstillingen. MCP `roots/list` besvares med launch-mappen plus alle tilføjede mapper (**v2.1.203+** sender også `notifications/roots/list_changed`).

### `/vim [file]`
- **Gør:** åbner `vim` (eller `nano` hvis `vim` mangler), og returnerer dig til Claude Code med det redigerede indhold i kontekst.
- **Eksempel:** `/vim src/app.ts`
- **Krav:** **v2.1.207+**.
- **Forveksl ikke med:** vim *editor mode* i prompten, som slås til under `/config` → Editor mode.

### `/run <command>` `[S]` · `/verify` `[S]` · `/todos` `[S]`
- `/run`: kører en shell-kommando eller et script og opfanger outputtet.
- `/verify`: kører som standard `npm test` eller `pytest` afhængigt af repoet; giv en kommando for noget andet. **Kører kun når du selv kalder den.**
- `/todos`: gennemsøger kodebasen for TODO-kommentarer og opsummerer pr. fil og status.
- **Relateret:** `/run-skill-generator` optager en build/launch-opskrift til `/run` og `/verify` ([skills](https://code.claude.com/docs/en/skills)).

### `/code-review [low|medium|high|xhigh|max|ultra] [--fix] [--comment] [pr#|branch|path]` `[S]`
- **Argumenter:** effort-niveau (valgfrit — genbruger dit seneste hvis udeladt), mål (diff som standard), og flag.
- **Flag:** `--fix` anvender fundene på working tree · `--comment` poster dem som inline-kommentarer på GitHub-PR'en · `ultra` kører en dyb cloud-review · med `ultra` på et `github.com`-PR-mål forvælger `--post` at poste de færdige fund i launch-dialogen.
- **Eksempel:** `/code-review high --comment 1423`
- **Alias:** `/review`. **Relateret:** `/simplify` (kun kvalitet, ingen bug-jagt), `/security-review`.

### `/security-review [--fix] [pr#|branch|path]` `[S]` · `/simplify [--fix] [file|path]` `[S]`
- `/security-review`: gennemgår mål for sikkerhedssårbarheder.
- `/simplify`: forenkler for læsbarhed og vedligeholdbarhed; rammer diffen som standard.

### `/batch <instruction>` `[S]`
- **Gør:** undersøger kodebasen, dekomponerer arbejdet i 5–30 uafhængige enheder, præsenterer en plan, og — efter godkendelse — starter én baggrunds-subagent pr. enhed i sin egen git-worktree. Hver subagent implementerer, kører tests og åbner en pull request.
- **Krav:** **et git-repository.**
- **Eksempel:** `/batch migrate src/ from JavaScript to TypeScript`

### `/design [brief]` `[S]` · `/design-sync [hint]` `[S]` · `/design-login` · `/dataviz [request]` `[S]` · `/deep-research <question>` `[F]`
- `/design`: kræver **v2.1.234+** og en session hvor artifacts er tilgængelige. **Ikke tilgængelig** på Amazon Bedrock, Google Cloud's Agent Platform, Microsoft Foundry eller Claude Platform on AWS (artifacts findes ikke der). Eksempel: `/design a settings screen for a mobile banking app`.
- `/design-sync`: konverterer repoets React-designsystem og uploader det til Claude Design, så designs bruger dine rigtige komponenter. **Faldgrube:** en førstegangssynkronisering verificerer hver komponent og **kan tage flere timer på et stort repo.** Samme udbyderbegrænsning som `/design`.
- `/design-login`: autoriserer design-system-adgang for `/design-sync` med din claude.ai-konto.
- `/dataviz`: kræver **v2.1.198+**. Validerer paletten for farveblindhed og kontrast med et medfølgende script.
- `/deep-research`: fan-out af websøgninger, krydstjek af kilder, syntese til en citeret rapport.

---

## 5. Git og GitHub

### `/install-github-app`
- **Gør:** installerer Claude GitHub App for et repository, med et valgfrit trin der sætter GitHub Actions-workflows og secrets op.

### `/autofix-pr [prompt]`
- **Argumenter:** valgfri prompt der erstatter standardinstruktionen.
- **Gør:** starter en Claude Code-web-session, der overvåger den aktuelle branchs PR og pusher fixes når CI fejler eller reviewers kommenterer.
- **Krav:** **`gh` CLI** og adgang til Claude Code on the web. Claude GitHub App skal være installeret på repoet for at auto-fix kan modtage webhooks ([web](https://code.claude.com/docs/en/claude-code-on-the-web)).
- **Faldgruber:**
  - Registrerer PR'en fra din **udcheckede branch** via `gh pr view` — vil du overvåge en anden PR, skal du checke dens branch ud først.
  - GitHub udsender **ingen webhook** når base-branchen rykker og skaber en merge-konflikt, så auto-fix kan ikke selv reagere på konflikter. Åbn sessionen og bed Claude rebase.
  - Claude kan svare på review-tråde **med din GitHub-konto**. Bruger repoet kommentar-udløst automatisering (Atlantis, Terraform Cloud, `issue_comment`-actions), kan det trigge de workflows.
- **Eksempel:** `/autofix-pr only fix lint and type errors`

### `/pr-comments [PR]` — **fjernet**
Fjernet i **v2.1.91**. Bed i stedet Claude direkte om at se PR-kommentarer. På ældre versioner hentede den kommentarer fra en GitHub-PR (auto-detektion fra branch, eller URL/nummer som argument) og krævede `gh` CLI.

---

## 6. MCP og integrationer

### `/mcp [reconnect <server>|enable|disable [<server>|all]]`
- **Argumenter:** uden argument åbnes den interaktive liste; `reconnect <server>` genforbinder én afbrudt server; `enable`/`disable` med servernavn eller `all` ændrer forbindelsestilstand uden dialog.
- **Gør:** administrerer MCP-serverforbindelser og OAuth-godkendelse.
- **Version:** virker også i `-p`, hvor den uden argument printer et tekstresumé i stedet for at åbne listen — kræver **v2.1.205+**.
- **Panelet viser:** serverstatus, autentificeringsvalg, antal værktøjer, samt mulighed for at genforbinde eller rydde autentificering.
- **Statusværdier du kan møde:** `✔ Connected` · `! Connected · tools fetch failed` · `! Needs authentication` · `✘ Failed to connect` · `✘ Connection error` · `⏸ Pending approval` · `⊘ Disabled for this project`. Ældre Windows-konsoller viser `√`/`×` i stedet for `✔`/`✘`.
- **Relateret:** `claude mcp add|list|get|remove`, `MCP_TIMEOUT`, `.mcp.json`, `~/.claude.json`, `disabledMcpServers`, `managedMcpServers` (managed setting for HTTP/SSE, tilføjet i **2.1.259**).
- **Kilder:** [MCP](https://code.claude.com/docs/en/mcp), [MCP quickstart](https://code.claude.com/docs/en/mcp-quickstart)

### MCP-prompt-kommandoer — `/mcp__<server>__<prompt>`

**Bekræftet mekanik.** `/`-menuen indeholder “commands contributed by [plugins](https://code.claude.com/docs/en/plugins) and [MCP servers](https://code.claude.com/docs/en/mcp#use-mcp-prompts-as-commands)” ([interactive-mode](https://code.claude.com/docs/en/interactive-mode)), og MCP-quickstarten linker til “**Run MCP prompts as commands** from the `/` menu” samt “**Reference MCP resources** in prompts with @ mentions” ([mcp-quickstart](https://code.claude.com/docs/en/mcp-quickstart)). Agent SDK-dokumentationen bekræfter det samme: “Sessions that configure MCP servers can also expose **MCP prompts as commands**” ([SDK](https://code.claude.com/docs/en/agent-sdk/slash-commands)).

**Navngivningsmønsteret** følger MCP-navngivningen dokumenteret på samme side for værktøjer:

```
mcp__<server-name>__<tool-name>
```

og for en server pakket i et plugin:

```
mcp__plugin_<plugin-name>_<server-name>__<tool-name>
```

hvor “any character outside `A-Z`, `a-z`, `0-9`, `_`, and `-` is replaced with `_`”. Eksempel fra dokumentationen: et `query`-værktøj i serveren `database-tools` i pluginet `my-plugin` kaldes `mcp__plugin_my-plugin_database-tools__query` ([MCP](https://code.claude.com/docs/en/mcp)).

> **[ikke verificeret]** Selve afsnittet *“Use MCP prompts as commands”* kunne **ikke hentes** i denne session: MCP-siden er for stor, og hentningsværktøjet afkortede den før afsnittet i alle forsøg (inkl. direkte anker-URL). Derfor er følgende **ikke** bekræftet mod primærkilden: at prompt-kommandoer bruger nøjagtig samme `mcp__server__prompt`-form som værktøjer; hvordan mellemrum og versaler i prompt-navne normaliseres; og hvordan argumenter overleveres. **Opfind ikke syntaksen** — kør `/` i din session og se de faktiske navne dine servere bidrager med, eller åbn afsnittet direkte: <https://code.claude.com/docs/en/mcp#use-mcp-prompts-as-commands>.
>
> **Konflikt mellem kilder:** overskriftslisten jeg kunne udtrække fra MCP-siden indeholder hverken “Use MCP prompts as commands” eller “Use MCP resources”, selvom **to andre officielle sider linker til præcis de ankre**. Enten ligger afsnittene efter afkortningspunktet (mest sandsynligt), eller også er linkene forældede. Nyeste kilde er i begge tilfælde MCP-siden selv.

- **Dynamisk opdagelse:** kommandoerne kommer og går med serverforbindelsen — der er ingen statisk liste at slå op.
- **Krav:** serveren skal være tilføjet og forbundet (`/mcp`), evt. efter OAuth-login.
- **Relateret:** `mcp__<server>__<tool>`-navne bruges også i permission-regler, en skills `allowed-tools`, en subagents `tools`-felt og hook-matchere. **Faldgrube:** en hook-matcher skrevet mod den bare servernøgle (`mcp__database-tools__.*`) **rammer aldrig** en plugin-pakket server.

### `/chrome` · `/ide` · `/install-slack-app` · `/artifacts` · `/import` · `/mobile`
- `/chrome`: konfigurerer Claude in Chrome-indstillinger.
- `/ide`: administrerer IDE-integrationer og viser status.
- `/install-slack-app`: åbner browseren for at gennemføre OAuth-flowet.
- `/artifacts`: lister artifacts du ejer eller er delt med, og lader dig vedhæfte én til sessionen, åbne den i browseren eller kopiere linket. Kræver **v2.1.208+**; vedhæftning med `Enter` kræver **v2.1.216+**.
- `/import [codex|gemini] [--dry-run] [--yes]`: henter konfiguration fra OpenAI Codex og Google Gemini CLI — instruktionsfiler, MCP-servere, kommandoer, subagents og skills. Kræver **v2.1.213+**. **Ikke tilgængelig** på Amazon Bedrock, Google Cloud's Agent Platform, Microsoft Foundry eller Claude Platform on AWS, og heller ikke når feature-flag-hentning er slået fra. I `-p` lister den fundene og giver dig kommandoen der bekræfter importen.
- `/mobile` (alias `/ios`, `/android`): viser QR-kode til mobilappen.

### `/schedule [beskrivelse]` (alias `/routines`)
Kilde: [Routines](https://code.claude.com/docs/en/routines).

- **Subkommandoer:** `/schedule list` · `/schedule update` · `/schedule run`. Uden argument kører den samtalebaseret.
- **Gør:** opretter og administrerer routines — gemte Claude Code-konfigurationer der kører på cloud-infrastruktur efter tidsplan, API-kald eller GitHub-events.
- **Eksempler:**
  - `/schedule daily PR review at 9am`
  - `/schedule tomorrow at 9am, summarize yesterday's merged PRs`
  - `/schedule add a GitHub trigger to my nightly review for pull requests opened in acme/webapp` (kræver **v2.1.225+**)
  - `/schedule why did my nightly review do nothing this morning?` (kræver **v2.1.227+**)
- **Krav:** **claude.ai-abonnementslogin** (Pro, Max, Team eller Enterprise). Ikke API-nøgler.
- **Faldgruber:**
  - Kommandoen **skjules** når krav ikke er opfyldt: menuen viser `No commands match "/schedule"` og indsendelse giver `Unknown command: /schedule`. Årsager: Console API-nøgle, Anthropic-profil/federation-credential, eller cloud-udbyder (Bedrock/GCP/Foundry); at du er **inde i en web-session** (brug web-UI'et); at organisationens policy slår Claude Code on the web fra; eller at en Owner har slået Routines fra.
  - `ANTHROPIC_API_KEY`, `ANTHROPIC_AUTH_TOKEN` og `apiKeyHelper` i `settings.json` **tager forrang** over et claude.ai-login — fjern dem først.
  - Minimum-interval er **én time**; hyppigere cron-udtryk afvises.
  - API-triggere kan **ikke** oprettes eller tilbagekaldes fra CLI'en — kun fra web.
- **Relateret:** `/loop` (in-session), [Desktop scheduled tasks](https://code.claude.com/docs/en/desktop-scheduled-tasks) (lokale, kører på din maskine).

### `/web-setup`
- **Gør:** synkroniserer din lokale `gh` CLI-token til din Claude-konto, så cloud-sessioner kan klone dine repositories.
- **Faldgruber:** giver **kun** klone-adgang. Den installerer **ikke** Claude GitHub App og aktiverer **ikke** webhook-levering — så auto-fix og GitHub-triggere virker ikke af den grund alene. Organisationer med **Zero Data Retention** kan ikke bruge `/web-setup`.
- **Synlighed:** styres af organisationsindstillingen **Quick web setup**, som på Team- og Enterprise-planer er **slået fra som standard** — hvilket skjuler kommandoen. En Owner slår den til under Admin settings → Claude Code.
- **Kilde:** [Claude Code on the web](https://code.claude.com/docs/en/claude-code-on-the-web)

---

## 7. Plugins og skills

Kilder: [Skills](https://code.claude.com/docs/en/skills), [Plugins](https://code.claude.com/docs/en/plugins), [Commands](https://code.claude.com/docs/en/commands).

### Hvordan plugins og skills bliver til slash commands

Alt hvad du kan køre med `/<navn>` hører til én af fire kategorier ([SDK](https://code.claude.com/docs/en/agent-sdk/slash-commands)):

| Type | Hvad der ligger bag | Eksempel |
|---|---|---|
| **Built-in commands** | Logik kodet ind i Claude Code-processen | `/compact` |
| **Bundled skills** | Prompt-artefakter der følger med Claude Code | `/code-review` |
| **Dine egne skills** | En mappe med en `SKILL.md` | `/security-check` |
| **Custom command files** | Ældre form: flade Markdown-filer i `.claude/commands/` | `/deploy` |

**Navngivning af plugin-skills:**

| Placering | Kommandonavn kommer fra | Resultat |
|---|---|---|
| `~/.claude/skills/deploy/SKILL.md` | Mappenavn | `/deploy` |
| `.claude/skills/deploy/SKILL.md` | Mappenavn | `/deploy` |
| `.claude/skills/subdir/deploy/SKILL.md` (nested, konflikt) | Sti + navn | `/subdir:deploy` |
| `my-plugin/skills/review/SKILL.md` | Frontmatter `name` (namespaced) | `/my-plugin:review` |
| `my-plugin/skills/review/SKILL.md` med `name: fancy` | Eget navn (namespaced) | `/my-plugin:fancy` |

Plugin-skills er **altid** namespaced (`/plugin-name:skill-name`) for at undgå konflikter; namespacet ændres via `name`-feltet i `plugin.json`. Konsekvensen ved migrering: den oprindelige `/skill-name` og plugin-kopien **eksisterer begge** — den ene overskriver ikke den anden.

**Indlæsningsrækkefølge (prioritet):** Enterprise → Personal (`~/.claude/skills/`) → Project (`.claude/skills/`) → Plugin (namespaced) → ekstra mapper (`--add-dir` / `/add-dir`). Højere prioritet vinder ved navnesammenfald.

**Skills afløser commands:** både `.claude/commands/deploy.md` og `.claude/skills/deploy/SKILL.md` giver `/deploy` — **skillen har forrang.**

### Bundled skills der følger med som standard

| Skill | Formål |
|---|---|
| `/run` | Start og driv appen for at verificere ændringer |
| `/verify` | Byg og kør appen for at bekræfte kodeændringer |
| `/run-skill-generator` | Optag build/launch-opskrift til `/run` og `/verify` |
| `/doctor` | Opsætningstjek — **tilgængelig selv med `disableBundledSkills: true`** |
| `/code-review` | Gennemgå kodeændringer (alias `/review`) |
| `/debug` | Fejlsøg problemer |
| `/batch` | Kør batch-operationer |
| `/loop` | Orkestrér iterative løkker |
| `/claude-api` | Claude API-integration |
| `/workflow-authoring` | Dynamiske workflows — kun når aktiveret |

Slå dem alle fra (undtagen `/doctor`) med `"disableBundledSkills": true`, eller enkeltvis via `skillOverrides`.

### `/plugin [subcommand]` · `/reload-plugins` · `/skills` · `/workflow` · `/loop`
- **`/plugin`**: uden argument åbnes plugin-menuen; ellers `list`, `install`, `enable`, `disable`. Claude Code kan aktivere et plugin under installationen — installationsresuméet fortæller om det skete, eller om du skal køre `/reload-plugins`. **Desktop bruger en grafisk plugin-manager i stedet; web-sessioner understøtter ikke kommandoen.**
- **`/reload-plugins`**: genindlæser plugins, skills, agents, hooks, plugin-MCP-servere og plugin-LSP-servere uden genstart. **Faldgrube:** live-ændringsdetektion dækker `SKILL.md`-ændringer i overvågede mapper, men **genstart kræves** for oprettelse af en ny top-level skills-mappe og for plugin-ændringer i hooks, MCP, agents og output-styles.
- **`/skills`**: interaktiv liste over alle skills grupperet efter kilde. Naviger med markøren, **`Space`** slår synlighed til/fra, **`Esc`** gemmer.
- **`/workflow [list|enable|disable] [<name>|all]`**: administrerer dynamiske workflows. Åbner nu straks midt i en tur (**før v2.1.234** blev den sat i kø).
- **`/loop [interval] [prompt]`** (alias `/proactive`): kører en prompt gentagne gange mens sessionen er åben. Udelad intervallet, så selvregulerer Claude tempoet; udelad prompten, så køres den indbyggede vedligeholdelsesprompt eller din `loop.md`. Eksempel: `/loop 5m check if the deploy finished`.

### `/skill-doctor`
- **Gør:** rapporterer skill-forbrug, kontekstomkostning og identificerer ubrugte skills.
- **Krav:** **v2.1.252+**. Tilføjet ifølge changelogen i **2.1.261** ([CHANGELOG](https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md): “Added `/skill-doctor` to identify unused skills and their context costs”).
- **Hvor:** åbner i **Stats**-fanen i `/plugin`-menuen; i `-p` udskrives tekst.
- **Kilde:** [Skills → Troubleshooting](https://code.claude.com/docs/en/skills). **[delvist verificeret]** — dokumenteret på skills-siden og i changelogen, men kunne ikke bekræftes som række i kommandotabellen.

### Skill-synlighed og tilladelser

Styr hvem der må kalde en skill via frontmatter:

| Frontmatter | Bruger kan kalde | Claude kan kalde | Beskrivelse i kontekst |
|---|---|---|---|
| (standard) | ✓ | ✓ | ✓ |
| `disable-model-invocation: true` | ✓ | ✗ | ✗ |
| `user-invocable: false` | ✗ | ✓ | ✓ |

Eller via `skillOverrides` i `.claude/settings.local.json`:

| Værdi | Vist til Claude | I `/`-menuen |
|---|---|---|
| `"on"` | Navn + beskrivelse | ✓ |
| `"name-only"` | Kun navn | ✓ |
| `"user-invocable-only"` | Skjult | ✓ |
| `"off"` | Skjult | ✗ |

Permission-regler bruger `Skill(...)`-syntaksen: `Skill(commit)`, `Skill(review-pr *)`, `Skill(deploy *)` under deny, eller bare `Skill` for at nægte alle.

---

## 8. Permissions og sikkerhed

### `/permissions`
- **Gør:** åbner en interaktiv dialog til allow-, ask- og deny-regler for værktøjstilladelser — se regler pr. scope, tilføj/fjern regler, administrér arbejdsmapper, og gennemgå seneste auto mode-afvisninger. Auto mode-classifier-regler redigeres i dialogens **Auto mode**-fane (tilføjet i **2.1.246**).
- **Version:** åbner nu straks midt i en tur, og dine ændringer gælder fra Claudes næste tool-kald i samme tur. **Før v2.1.234** blev kommandoen sat i kø.
- **Faldgrube:** `/permissions` har **ingen argumentform**, og derfor svarer den i desktop-appens Code-fane og i cloud-sessioner `isn't available in this environment`. Redigér [settings-filer](https://code.claude.com/docs/en/settings) direkte, eller kør kommandoen fra den selvstændige CLI.
- **Alias:** `/allowed-tools`.

### `/auto-mode-setup`
- **Gør:** udarbejder `autoMode.environment`-poster ud fra dit projekt og seneste sessioner, og gemmer dem i dine user settings efter din gennemgang.
- **Krav:** **Pro-, Max- eller Team-plan** og **v2.1.228+**. På native Windows kræves **v2.1.233+**.

### `/fewer-permission-prompts` `[S]`
- **Gør:** scanner dine transskripter for hyppige read-only Bash- og MCP-kald og tilføjer en prioriteret allowlist til projektets `.claude/settings.json`.

### `/privacy-settings`
- **Gør:** se og opdatér privatlivsindstillinger. **Kun Pro- og Max-abonnenter.**

### `/security-review [--fix] [pr#|branch|path]` `[S]`
Se [afsnit 4](#4-filer-og-kode).

---

## 9. Diagnostik og fejlsøgning

### `/doctor` `[S]` (alias `/checkup`)
- **Gør:** kører et opsætningstjek der diagnosticerer problemer og kan reparere dem.
- **Tjekker:** installationssundhed (dubletter og efterladte installationer, `PATH`-problemer, ikke-parsbare settings-filer); ubrugte skills, MCP-servere og plugins målt mod deres kontekstomkostning; langsomme hooks; nyere version på din release-kanal. Deduplikerer lokale `CLAUDE.md`-filer mod de indtjekkede, trimmer indtjekkede `CLAUDE.md`-filer ved at fjerne indhold Claude kan udlede af kodebasen, og migrerer den resterende altid-indlæste vejledning til skills og nested `CLAUDE.md`-filer der indlæses efter behov. Tilbyder også at gøre auto mode til din standard og at forhåndsgodkende ofte afviste read-only-kommandoer.
- **Sikkerhed:** rapporterer fund **først** og beder om bekræftelse før noget ændres.
- **Fra terminalen:** `claude doctor` printer read-only installationsdiagnostik uden at starte en session. **2.1.261** tilføjede organisationens policy-status til outputtet.
- **Version:** `CLAUDE.md`-trimtjekket kræver **v2.1.206+**. **Før v2.1.205** åbnede `/doctor` en read-only diagnostikskærm, hvor **`f`** sendte rapporten til Claude.

### `/debug [description]` `[S]`
- **Gør:** slår debug-logning til for sessionen og fejlsøger ved at læse sessionens debug-log.
- **Faldgrube:** debug-logning er **slået fra som standard** medmindre du startede med `claude --debug` — kører du `/debug` midt i sessionen, opsamles logs først **fra det tidspunkt og frem**.

### `/bug [report]` (alias `/share`) · `/feedback [report]`
- **Gør:** rapportér en fejl eller del samtalen / send produktfeedback. Du vælger hvor meget sessionshistorik der medtages og bekræfter på en samtykkeskærm, før noget sendes.
- **Hvor rapporten havner:** er du logget ind hos Anthropic på en first-party-forbindelse, går den til Anthropic. På en tredjepartsudbyder, eller uden Anthropic-credentials, skriver Claude Code i stedet rapporten til et lokalt arkiv under `~/.claude/feedback-bundles/`, som du selv videresender.
- **IDE:** i **VS Code-extensionen** åbner `/bug` extensionens egen feedback-dialog — kræver **v2.1.229+**.
- **Version:** åbner nu dialogen straks midt i en tur (**før v2.1.232** blev den sat i kø). **Før v2.1.212** var `/bug` og `/share` aliaser for `/feedback`.
- **`/feedback` uden argument** åbner i sessioner med Claude-udkastet feedback i stedet **udkastkøen**, hvor du gennemgår, redigerer, sender eller kasserer. `SendFeedback`-værktøjet (så Claude selv kan skrive udkast) blev tilføjet i **2.1.247**.

### `/heapdump` `[H]`
- **Gør:** skriver et JavaScript-heap-snapshot og en hukommelsesopgørelse til `~/Desktop` (eller hjemmemappen på Linux uden Desktop-mappe).
- **⚠️ Sikkerhed:** vedhæft **kun** `-diagnostics.json`-filen når du rapporterer et hukommelsesproblem. **`.heapsnapshot` indeholder hele din samtale og dine credentials — del den ikke.**
- **Synlighed:** skjult fra kommandomenuen; skriv navnet fuldt ud.

### `/insights`
- **Gør:** genererer en HTML-rapport over dine seneste sessioner på denne maskine: hvilke projekter du arbejder i, hvordan du bruger Claude Code, hvor tingene går galt, og features at prøve.
- **Faldgrube:** **ikke tilgængelig i cloud-sessioner.**

### `/release-notes` · `/upgrade` · `/terminal-setup` `[S]` · `/help`
- `/release-notes`: viser hvad der er nyt.
- `/upgrade`: opgraderer til nyeste version på din [release-kanal](https://code.claude.com/docs/en/setup). Kræver **v2.1.236+**. **Ikke tilgængelig på Enterprise-planer** — kontakt din admin.
- `/terminal-setup`: konfigurerer din shell med Claude Code-completions og aliaser, inkl. `/`-completion for kommandoer og shell-integration. Kræver et interaktivt terminalmiljø (vises ikke i SDK'ens `slash_commands`-liste).
- `/help`: viser hjælp og tilgængelige kommandoer. **Dette er den autoritative liste for netop din installation** — brug den frem for enhver skrevet oversigt. Fanen **Custom commands** viser dine egne og plugin-leverede kommandoer under deres namespace.

---

## 10. Konto og billing

### `/login` · `/logout`
- Logger ind/ud af din Anthropic-konto. `/login` er også løsningen når `--teleport`, `--cloud` eller `/schedule` klager over autentificering: de kræver et **claude.ai-abonnementslogin**, ikke en API-nøgle.

### `/usage` (alias `/cost`)
- **Gør:** viser API-forbrug, omkostning og rate limits for den aktuelle session og alle sessioner på denne maskine.
- **Version:** **2.1.251** tilføjede en spend limit-bjælke til `/usage` og en per-session prompt-cache-linje til `/cost`.
- **Desktop:** klik på forbrugsringen ved siden af modelvælgeren for kontekstvindue- og planforbrug. Kontekstforbrug er pr. session; planforbrug deles på tværs af alle dine Claude Code-overflader.

### `/passes`
- **Gør:** del en gratis uge med Claude Code med venner. **Kun synlig hvis din konto er berettiget.**

### `/powerup` · `/radio`
- `/powerup`: korte interaktive lektioner med animerede demoer.
- `/radio`: åbner Claude FM lo-fi-radio i browseren; printer stream-URL'en når ingen browser er tilgængelig.

---

## Appendiks A — Forskelle: desktop vs. CLI vs. web

### A.1 Grundreglen

Desktop-appens **Code**-fane og web-sessioner kører den samme motor som CLI'en og læser de samme konfigurationsfiler. Forskellen ligger i, at **kommandoer der åbner et interaktivt terminalpanel ikke har noget panel at åbne** uden for terminalen. Dokumentationen formulerer det sådan ([desktop](https://code.claude.com/docs/en/desktop)):

> **Terminal-dialog commands**: built-in commands that open an interactive panel in the terminal behave differently in the Code tab. Edit settings files directly to manage permission rules and configuration, or run the commands from the standalone CLI.
> * Commands with no argument form, such as `/permissions`, reply with `isn't available in this environment`.
> * `/config` opens Settings → Claude Code. Text after the command is ignored, so `/config theme=dark` doesn't set the theme.

Web-sessioner følger samme princip ([web](https://code.claude.com/docs/en/claude-code-on-the-web)):

> Cloud sessions support built-in commands that produce text output. Commands that only run in the terminal interface, such as `/plugin` or `/resume`, aren't available.

### A.2 Konkret kommandotabel

| Kommando | CLI | Desktop (Code-fanen) | Web (cloud) |
|---|---|---|---|
| `/permissions` | Fuld dialog, åbner straks midt i en tur | `isn't available in this environment` | `isn't available in this environment` |
| `/config` | Panel **og** `key=value` | Åbner Settings → Claude Code; **tekst efter ignoreres** | Åbner Claude Code-indstillinger; **`key=value` ignoreres** |
| `/model` | Vælger + `s` for session-only | Modeldropdown ved send-knappen | **Argumentform:** `/model sonnet` (kræver v2.1.205+) |
| `/effort` | Vælger/slider | Menu (`Cmd+Shift+E`) | **Argumentform**; rapporterer `Not applied` under effort-hold |
| `/fast` | Toggle | Toggle | **Argumentform**; virker kun hvis sessionen startede med fast mode til |
| `/color`, `/theme`, `/rename` | Vælger | Vælger/UI | **Argumentform** (v2.1.205+) |
| `/plugin` | Menu + subkommandoer | **Grafisk plugin-manager** | **Ikke tilgængelig** |
| `/resume` | Vælger + argument | Klik en session i sidebaren | **Ikke tilgængelig** |
| `/clear` | ✓ | ✓ | **Ikke tilgængelig** — start ny session fra sidebaren |
| `/compact`, `/context` | ✓ | ✓ | ✓ |
| `/insights` | ✓ | ✓ | **Ikke tilgængelig i cloud-sessioner** |
| `/desktop` | ✓ (macOS + x64 Windows, abonnement) | — (du er der allerede) | — |
| `/teleport`, `/remote-control` | ✓ | — | Inde i en cloud-session svarer `/teleport` med kommandoen at køre lokalt |
| `/schedule` | ✓ (kræver claude.ai-login) | ✓ (også Routines i sidebaren) | **Ikke tilgængelig** — brug web-UI'et |
| `/bug` | Dialog | Dialog | Dialog |
| `/heapdump`, `/terminal-setup`, `/vim`, `/exit`, `/ide`, `/powerup` | ✓ | Terminal-orienterede — begrænset/ikke relevant | Ikke tilgængelige (kræver interaktiv terminal) |

### A.3 Feature-forskelle ud over slash commands

| Feature | CLI | Desktop |
|---|---|---|
| Permission modes | Alle, inkl. `dontAsk` | Manual, Accept edits, Plan, Auto. Bypass vises når aktiveret (Pro/Max: Settings-toggle; Team/Enterprise: org-policy) |
| Tredjepartsudbydere | Bedrock, Google Cloud's Agent Platform, Microsoft Foundry | Anthropics API som standard |
| MCP-servere | Settings-filer | Connectors-UI (lokale og SSH-sessioner) eller settings-filer |
| Plugins | `/plugin` | Plugin-manager-UI |
| @mention af filer | Tekstbaseret | Med autocomplete; kun lokale og SSH-sessioner |
| Filvedhæftninger | Ikke tilgængeligt | Billeder, PDF'er |
| Sessionsisolation | `--worktree`-flaget | Automatiske worktrees |
| Flere sessioner | Separate terminaler | Sidebar-faner |
| Tilbagevendende opgaver | Cron-jobs, CI-pipelines | Scheduled tasks |
| Computer use | Via `/mcp` på macOS | App- og skærmstyring på macOS og Windows (research preview, kræver Pro/Max, **ikke** Team/Enterprise) |
| Agent teams | ✓ | **Ikke tilgængeligt** — brug dynamiske workflows |
| Scripting/automatisering | `--print`, Agent SDK | **Ikke tilgængeligt** — desktop er kun interaktiv |

**Ikke tilgængeligt i Desktop:** tredjepartsudbydere (som standard), Computer Use på Linux-beta'en, inline kodeforslag, agent teams, og `--print`/`--output-format`.

### A.4 IDE-extensions

Dokumentationen indeholder **ingen dedikeret slash command-matrix for IDE-extensions**. De bekræftede punkter er:
- **`/bug`** åbner i VS Code-extensionen extensionens egen feedback-dialog (kræver **v2.1.229+**).
- **Focus-visningen** findes i VS Code-extensionen som en toggle i kommandomenuen, gemt som en extension-indstilling, **uafhængigt af `viewMode`** — altså ikke via `/focus`.
- MCP-servere tilføjes i VS Code via extensionens egen vej ([vs-code](https://code.claude.com/docs/en/vs-code)).

**[ikke verificeret]** En fuldstændig liste over hvilke kommandoer JetBrains- og VS Code-extensionerne understøtter er ikke publiceret.

---

## Appendiks B — Udgåede, omdøbte og ændrede kommandoer

| Kommando | Status | Erstatning / detaljer | Kilde |
|---|---|---|---|
| `/pr-comments [PR]` | **Fjernet i v2.1.91** | Bed Claude direkte om at se PR-kommentarer | [commands](https://code.claude.com/docs/en/commands) |
| `/agents` | **Adfærd ændret i v2.1.198** | Printer nu blot en påmindelse om at bede Claude oprette/administrere subagents, eller redigere `.claude/agents/` og `~/.claude/agents/` direkte. På **v2.1.197 og tidligere** åbnede den en interaktiv grænseflade | [commands](https://code.claude.com/docs/en/commands) |
| `/bug`, `/share` | **Omdefineret i v2.1.212** | **Før v2.1.212** var de aliaser for `/feedback`. Nu er `/share` alias for `/bug`, og `/feedback` er sin egen kommando med udkastkø | [commands](https://code.claude.com/docs/en/commands) |
| `/doctor` | **Omskrevet i v2.1.205** | **Før v2.1.205** åbnede den en read-only diagnostikskærm hvor `f` sendte rapporten til Claude. Nu er den en bundled skill der kan reparere | [commands](https://code.claude.com/docs/en/commands) |
| `/btw` | **Ændret i v2.1.212** | **Før v2.1.212** krævede den et spørgsmål; nu viser den seneste sidespørgsmål uden argument | [commands](https://code.claude.com/docs/en/commands) |
| `/resume` | **Udvidet i v2.1.212** | **Før v2.1.212** kunne du ikke give den et argument | [commands](https://code.claude.com/docs/en/commands) |
| `/fork` | **Omdefineret i v2.1.212** | På **v2.1.161–v2.1.211** (og når agent view er slået fra) starter den en *forked subagent* i stedet for en baggrundssession | [commands](https://code.claude.com/docs/en/commands) |
| `.claude/commands/*.md` | **Erstattet, men understøttet** | Skills (`.claude/skills/<navn>/SKILL.md`) er den anbefalede efterfølger. Kommandofiler virker fortsat; ved navnesammenfald **vinder skillen** | [skills](https://code.claude.com/docs/en/skills) |
| `--remote` | **Udgået alias** | Brug `--cloud`. Den ældre stavemåde virker stadig | [web](https://code.claude.com/docs/en/claude-code-on-the-web) |
| `keybindingFlavor` (setting) | **Udgået i v2.1.261** | Readline-konventionerne er nu standard; indstillingen har ingen effekt | [interactive-mode](https://code.claude.com/docs/en/interactive-mode) |
| Kø-adfærd midt i en tur | **Ændret i v2.1.234** | `/add-dir`, `/permissions`, `/workflow` og dialogkommandoer i fullscreen (`/theme`, `/help`) åbner nu **straks** i stedet for at blive sat i kø | [commands](https://code.claude.com/docs/en/commands) |
| Kø-adfærd for `/model`, `/effort`, `/fast` | **Ændret i v2.1.242** | **Før v2.1.242** afgjorde et feature-flag hentet fra Anthropic om kommandoen kørte midt i turen; i sessioner uden feature-flag-hentning (fx tredjepartsudbydere) blev den altid sat i kø | [commands](https://code.claude.com/docs/en/commands) |

**Kommandoer der ikke længere findes i den aktuelle dokumentation** — søgt uden resultat: `/output-style`, `/statusline`, `/migrate-installer`, `/sandbox`, `/connect`. **[ikke verificeret]** Om de er fjernet eller blot udokumenterede kan ikke afgøres fra kilderne; de optræder ikke i den nuværende kommandotabel, og der findes ingen deprecation-note om dem.

### Nyt siden tidligere versioner (fra changelogen)

| Version | Ændring vedr. kommandoer |
|---|---|
| **2.1.261** | `/skill-doctor` tilføjet (identificerer ubrugte skills og deres kontekstomkostning); organisationens policy-status tilføjet til `/status` og `claude doctor` |
| **2.1.260** | **`/diff` tilføjet** til visning af ikke-committede ændringer i fullscreen; tekstform af `/advisor` til desktop- og headless-sessioner |
| **2.1.257** | Sessionsspecifikt effort-skift med `s` i `/effort` |
| **2.1.251** | Spend limit-bjælke i `/usage`; per-session prompt-cache-linje i `/cost` |
| **2.1.247** | `/claude-api cost-optimize` tilføjet |
| **2.1.246** | **Auto mode**-fane tilføjet til `/permissions` |
| **2.1.236** | `/upgrade` tilføjet; `/claude-api upgrade` tilføjet |
| **2.1.234** | `/design` tilføjet; kø-adfærd ændret for flere kommandoer |
| **2.1.221** | `/autocompact` tilføjet; `/claude-api prompt-audit` tilføjet |

Kilde: [CHANGELOG.md](https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md)

---

## Appendiks C — Tastaturgenveje der løser samme opgave

Kilder: [Interactive mode](https://code.claude.com/docs/en/interactive-mode) (terminal), [Desktop application](https://code.claude.com/docs/en/desktop) (Code-fanen).

### C.1 Genveje der erstatter en slash command (terminal)

| Genvej | Svarer til | Note |
|---|---|---|
| `Esc` `Esc` (tom prompt) | `/rewind` | Åbner rewind-menuen. Med tekst i prompten ryddes den i stedet og gemmes i historikken |
| `Ctrl+O` | Transskript-viser | Ikke `/context` — viser detaljeret værktøjsbrug, tidsstempel og model pr. svar |
| `Shift+Tab` (eller `Alt+M` på Windows uden VT input mode) | `/plan` m.fl. | Cykler `default` (Manual) → `acceptEdits` → `plan` → evt. `bypassPermissions` → `auto` |
| `Option+P` / `Alt+P` | `/model` | Skift model uden at rydde prompten |
| `Option+O` / `Alt+O` | `/fast` | Slå fast mode til/fra |
| `Option+T` / `Alt+T` | — | Slå extended thinking til/fra. **Ingen effekt på Fable 5.1 eller Fable 5**, som altid bruger extended thinking |
| `Ctrl+B` | `/background` (delvist) | Backgrounder Bash-kommandoer og agents. Tmux-brugere trykker to gange |
| `Ctrl+T` | — | Slår Claudes to-do-checkliste til/fra. **Ikke** baggrundsopgave-visningen — brug `/tasks` til kørende shells og subagents |
| `Ctrl+X` `Ctrl+K` | — | Stopper alle kørende baggrunds-subagents og slår artifact-autosvar fra. Tryk to gange inden for 3 sekunder for at bekræfte |
| `Ctrl+R` | — | Reverse search i kommandohistorik |
| `Ctrl+G` eller `Ctrl+X` `Ctrl+E` | `/vim` (beslægtet) | Åbner prompten i din standard-teksteditor |
| `Ctrl+S` | — | Gemmer prompten væk og rydder feltet; tryk igen på tom prompt for at hente den tilbage |
| `?` på tom prompt | `/help` (delvist) | Slår genvejshjælpepanelet til/fra |

### C.2 Præfikser i prompten (ikke slash commands)

| Præfiks | Betydning |
|---|---|
| `/` i starten | Kommando eller skill |
| `!` i starten | **Shell mode** — kør en kommando direkte, læg outputtet i sessionen, og lad Claude svare på det |
| `@` | Fil-sti-autocomplete. I sessioner med cross-session messaging foreslås også dine andre live-sessioner når du skriver mindst ét bogstav efter `@` (kræver **v2.1.232+**) |
| `#` | Memory (se `/memory`) |
| `:` | Emoji-shortcode — skriv `:navn:` eller to+ tegn for forslag (kræver **v2.1.217+**) |

### C.3 Desktop-appens genveje (Code-fanen)

`Cmd+/` (macOS) / `Ctrl+/` (Windows) viser alle genveje. På Windows bruges `Ctrl` i stedet for `Cmd`; sessionscykling, terminal-toggle og view-mode-toggle bruger `Ctrl` på alle platforme.

| Genvej | Handling |
|---|---|
| `Cmd` `/` | Vis tastaturgenveje |
| `Cmd` `N` / `Cmd` `W` | Ny / luk session |
| `Ctrl` `Tab` / `Ctrl` `Shift` `Tab` | Næste / forrige session |
| `Cmd` `Shift` `]` / `Cmd` `Shift` `[` | Næste / forrige session |
| `Esc` | Stop Claudes svar |
| `Cmd` `Shift` `D` | Slå diff-panelet til/fra (svarer til `/diff`) |
| `Cmd` `Shift` `B` | Slå Browser-panelet til/fra |
| `Cmd` `Shift` `S` | Vælg et element i Browseren |
| `Ctrl` `` ` `` | Slå terminal-panelet til/fra |
| `Cmd` `\` | Luk det fokuserede panel |
| `Cmd` `;` | Åbn side-chat (svarer til `/btw`) |
| `Ctrl` `O` | Cykl view modes (Normal / Verbose / Summary) |
| `Cmd` `Shift` `M` | Åbn permission mode-menuen (erstatter `Shift+Tab`) |
| `Cmd` `Shift` `I` | Åbn modelmenuen (svarer til `/model`) |
| `Cmd` `Shift` `E` | Åbn effort-menuen (svarer til `/effort`) |
| `1`–`9` | Vælg punkt i en åben menu |

> **Vigtigt:** desktop-genvejene gælder **kun** Code-fanen, og de terminalbaserede interactive mode-genveje — heriblandt `Shift+Tab` til at cykle permission modes — **gælder ikke i Desktop.**

---

## Appendiks D — Sådan laver du dine egne kommandoer

Kilde: [Skills](https://code.claude.com/docs/en/skills), [Plugins](https://code.claude.com/docs/en/plugins), [SDK](https://code.claude.com/docs/en/agent-sdk/slash-commands).

### D.1 Vælg form: skill (anbefalet) eller kommandofil (ældre)

| | Skill | Kommandofil |
|---|---|---|
| Sti | `.claude/skills/<navn>/SKILL.md` | `.claude/commands/<navn>.md` |
| Status | **Anbefalet** | Ældre form, virker fortsat |
| Ved navnesammenfald | **Vinder** | Taber |
| Kan have hjælpefiler/scripts | Ja | Nej (flad fil) |

Begge giver `/<navn>`.

### D.2 Placering og scope

| Niveau | Sti | Gælder for |
|---|---|---|
| Enterprise | Managed settings | Alle i organisationen |
| Personlig | `~/.claude/skills/<navn>/SKILL.md` | Alle dine projekter |
| Projekt | `.claude/skills/<navn>/SKILL.md` | Kun dette projekt |
| Plugin | `<plugin>/skills/<navn>/SKILL.md` | Hvor pluginet er aktiveret (namespaced) |
| Ekstra mapper | `.claude/skills/` i `--add-dir`-mapper | Sessionen |

**Namespacing:** nested skills under en undermappe får et sti-kvalificeret navn ved konflikt (`/subdir:deploy`). Plugin-skills er altid `/plugin-navn:skill-navn`.

### D.3 Frontmatter-felter

| Felt | Påkrævet | Beskrivelse |
|---|---|---|
| `name` | Nej | Vist navn. Standard: mappenavnet. Plugin-skills bruger det til kommandonavnet |
| `description` | Anbefalet | Hvornår skillen skal bruges — Claude bruger den til auto-invokering. Afkortes ved 1.536 tegn tilsammen med `when_to_use` |
| `when_to_use` | Nej | Ekstra trigger-kontekst, tilføjes til `description`. Tæller med i 1.536-tegnsgrænsen |
| `argument-hint` | Nej | Hint i autocomplete, fx `[issue-number]` eller `[filename] [format]` |
| `arguments` | Nej | Navngivne positionsargumenter til `$navn`-substitution. Mellemrumssepareret streng eller YAML-liste |
| `disable-model-invocation` | Nej | `true` forhindrer Claude i at auto-invokere. Kun brugeren kan kalde med `/navn` |
| `user-invocable` | Nej | `false` skjuler fra `/`-menuen. Kun Claude kan kalde. Standard: `true` |
| `allowed-tools` | Nej | Forhåndsgodkend værktøjer for den tur skillen kaldes. **Godkendelsen ryddes ved næste brugerbesked** |
| `disallowed-tools` | Nej | Fjern værktøjer mens skillen er aktiv. Kan ikke fjerne `EndConversation` hvis andre værktøjer består |
| `model` | Nej | Modeloverstyring mens skillen er aktiv. Samme værdier som `/model`, eller `inherit` |
| `effort` | Nej | `low`, `medium`, `high`, `xhigh`, `max` |
| `context` | Nej | `fork` kører i isoleret subagent-kontekst |
| `agent` | Nej | Subagent-type ved `context: fork`: `Explore`, `Plan`, `general-purpose` eller en custom agent |
| `background` | Nej | Med `context: fork`: `false` venter på resultatet (standard `true`). **v2.1.218+** |
| `hooks` | Nej | Hook-konfigurationer der registreres når skillen kaldes |
| `paths` | Nej | Glob-mønstre der begrænser hvornår skillen auto-aktiveres, fx `src/** tests/**` |
| `shell` | Nej | Shell til `` !`kommando` ``-blokke: `bash` (standard) eller `powershell` |
| `metadata` | Nej | Fri YAML key-value-map til eget værktøj. Claude Code ignorerer den |
| `license` | Nej | Licens (Agent Skills-spec) |
| `compatibility` | Nej | Miljøkrav (Agent Skills-spec). Maks. 500 tegn |

**Kun disse felter virker uden for Claude Code** (claude.ai-uploads, Skills API): `name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`.

### D.4 Argumenter

| Variabel | Betydning |
|---|---|
| `$ARGUMENTS` | Alle argumenter. **Tilføjes automatisk til sidst** hvis ingen placeholder modtager dem |
| `$ARGUMENTS[N]` | Argument efter 0-baseret indeks |
| `$N` | Kortform for `$ARGUMENTS[N]` — `$0` er første, `$1` er andet |
| `$navn` | Navngivet argument deklareret i `arguments`-frontmatter, mappet efter position |
| `${CLAUDE_SESSION_ID}` | Sessionens ID |
| `${CLAUDE_EFFORT}` | Aktuelt effort-niveau |
| `${CLAUDE_SKILL_DIR}` | Mappen med skillens `SKILL.md` |
| `${CLAUDE_PROJECT_DIR}` | Projektroden (**v2.1.196+**) |
| `${CLAUDE_PLUGIN_ROOT}` / `${CLAUDE_PLUGIN_DATA}` | Plugin-installations- og datamappe (kun plugin-skills) |

**Escaping:** brug `\$` foran cifre eller `ARGUMENTS` for et bogstaveligt dollartegn — `\$1.00` bliver til `$1.00`.

> ⚠️ **Bemærk om `$0`.** Dokumentationen er **selvmodsigende** på dette punkt: substitutionstabellen siger `$0` = første argument, `$1` = andet, mens `$1`/`$2` i mange ældre eksempler ude i verden betyder første/andet. Substitutionstabellen (`$0` = første) er den aktuelle primærkilde. Test din egen kommando med et konkret kald før du stoler på nummereringen.

### D.5 Dynamisk kontekst med `` !`kommando` ``

Inline:

```markdown
## Aktuelle ændringer
!`git diff HEAD`
```

Flerlinje:

````markdown
## Miljø
```!
node --version
git status --short
```
````

**Detaljer:** kører i sessionens arbejdsmappe (følger `cd`) · stderr flettes ind i stdout · timeout **2 minutter** · output afkortes ved et loft, resten leveres som filsti · **exit-kode ≠ 0 afbryder invokeringen**, dog behandles exit-kode 1 fra `grep`, `git diff`, `find` og `diff` som normalt output i bash; exit-koder 2+ fejler altid.

**Slå fra:** `"disableSkillShellExecution": true` i settings erstatter kommandoer med `[shell command execution disabled by policy]`. Påvirker ikke bundled eller managed skills.

**Faldgrube:** i cloud-sessioner og for skills synkroniseret fra claude.ai **kører kommandoerne ikke** — teksten sendes bogstaveligt til Claude.

### D.6 Fungerende eksempel

Opret `.claude/skills/fix-issue/SKILL.md`:

```markdown
---
name: fix-issue
description: Retter et GitHub-issue på en navngiven branch, med diff og tests.
argument-hint: [issue-number] [branch]
arguments: issue branch
allowed-tools: Read Grep Bash(git status) Bash(git diff *)
disable-model-invocation: true
---

# Ret issue $issue

## Aktuel tilstand
!`git status --short`

## Ændringer indtil nu
!`git diff HEAD`

Ret GitHub-issue **$issue** på branchen **$branch**:

1. Find den relevante kode med Grep.
2. Foreslå den mindst mulige rettelse.
3. Kør projektets tests og rapportér resultatet ærligt.
4. Opsummer ændringen i to sætninger.
```

Kald den:

```
/fix-issue 123 feature-fix
```

Her bliver `$issue` = `123` og `$branch` = `feature-fix`.

**Hvad eksemplet demonstrerer:** navngivne argumenter via `arguments`, autocomplete-hint, forhåndsgodkendte værktøjer (så du ikke får tilladelsesprompter for `git status`/`git diff`), dynamisk kontekst med `` !` ``, og `disable-model-invocation: true` så kun *du* kan udløse den.

### D.7 Test, fejlfinding og fjernelse

- **Live-ændringer** opdages i `~/.claude/skills/`, projektets `.claude/skills/` og `.claude/skills/` i `--add-dir`-mapper. **Genstart kræves** for en ny top-level skills-mappe og for plugin-ændringer.
- **Validér YAML:** `claude plugin validate ~/.claude/skills` eller `claude plugin validate .claude/skills` (kræver **v2.1.233+**; `--json` tilføjet i **2.1.259**). Eller kør med `--debug`.
- **Afkortede beskrivelser:** budgettet er 1 % af kontekstvinduet, justerbart med `skillListingBudgetFraction` eller `SLASH_COMMAND_TOOL_CHAR_BUDGET`; hver beskrivelse er kappet ved 1.536 tegn (`skillListingMaxDescChars`). Sæt lavprioritets-skills til `"name-only"` for at frigøre budget.
- **Stak op til 6 skills:** `/write-tests /fix-issue 123` — stopper ved en skill der ikke kan køre inline (forked subagent eller `/loop`).
- **Fjern:** slet mappen (personlig/projekt), deaktivér pluginet, eller sæt `skillOverrides: {"navn": "off"}`. Vil du beholde den men forhindre auto-invokering: `disable-model-invocation: true`.
- **Best practice:** hold `SKILL.md` under 500 linjer og flyt detaljer til separate filer i skill-mappen.

### D.8 Pak det som plugin

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json        ← kun manifestet ligger her
├── skills/
│   └── fix-issue/
│       └── SKILL.md
├── agents/
├── hooks/hooks.json
├── .mcp.json
└── commands/              ← ældre flad form; brug skills/ til nye plugins
```

```json
{
  "name": "my-plugin",
  "description": "Interne udviklingsværktøjer",
  "version": "1.0.0"
}
```

Test lokalt: `claude --plugin-dir ./my-plugin` (accepterer også en `.zip`), derefter `/my-plugin:fix-issue 123 feature-fix`. Kør `/reload-plugins` efter ændringer.

> ⚠️ **Den hyppigste fejl:** læg **ikke** `commands/`, `agents/`, `skills/` eller `hooks/` inde i `.claude-plugin/`. **Kun `plugin.json` hører til der** — alle andre mapper ligger i plugin-roden. Plugin-roden er pluginets egen mappe, **aldrig `~/.claude/`**.

---

## Ikke-verificerede punkter

Til sidst, eksplicit, hvad jeg **ikke** kunne bekræfte og hvorfor:

1. **Desktop-appens versionsnummer.** Anthropic publicerer det ikke i dokumentationen — kun minimumskrav som “v1.2581.0 or later”. **Løsning:** macOS → Claude → About Claude; Windows → Help → About.

2. **Fuldstændigheden af kommandolisten.** Kommandotabellen på [/docs/en/commands](https://code.claude.com/docs/en/commands) blev afkortet af hentningsværktøjet ved hvert forsøg (`[Content truncated due to length...]`), og gentagne hentninger gav indbyrdes modstridende svar for tabellens sidste del. Rækkerne ovenfor er samlet over to hentninger der dækkede `/add-dir` → `/workflow` og er krydstjekket hvor muligt, men **jeg kan ikke garantere at ingen række mangler.** `/help` i din app er den autoritative liste.

3. **Kommandoer dokumenteret uden for kommandotabellen.** `/schedule`, `/routines`, `/web-setup`, `/rename`, `/skill-doctor`, `/tui` og `/voice` er alle bekræftet på andre officielle sider ([routines](https://code.claude.com/docs/en/routines), [web](https://code.claude.com/docs/en/claude-code-on-the-web), [skills](https://code.claude.com/docs/en/skills), [interactive-mode](https://code.claude.com/docs/en/interactive-mode)), men **kunne ikke bekræftes som rækker i kommandotabellen**. De findes; deres fulde syntaks og overfladetilgængelighed er delvist ubekræftet. `/rename`, `/tui` og `/voice` har jeg kun ét kildebelæg for hver, og derfor ingen fuld feltbeskrivelse ovenfor.

4. **MCP-prompt-kommandoernes præcise syntaks.** Afsnittet “Use MCP prompts as commands” kunne ikke hentes — MCP-siden afkortes før det, også ved direkte anker-URL. Tre officielle sider bekræfter at **MCP-servere bidrager med kommandoer til `/`-menuen**, og `mcp__<server>__<tool>`-navnekonventionen er verificeret for *værktøjer*. At prompts bruger samme form, hvordan navne normaliseres, og hvordan argumenter overleveres, er **[ikke verificeret]**. Åbn <https://code.claude.com/docs/en/mcp#use-mcp-prompts-as-commands> eller skriv `/` i en session med en forbundet prompt-server.

5. **Kildekonflikt på MCP-siden.** Overskriftslisten jeg kunne udtrække indeholder hverken “Use MCP prompts as commands” eller “Use MCP resources”, selvom [mcp-quickstart](https://code.claude.com/docs/en/mcp-quickstart) og [interactive-mode](https://code.claude.com/docs/en/interactive-mode) begge linker til de ankre. Mest sandsynligt ligger afsnittene efter afkortningspunktet; alternativt er linkene forældede. Begge sider er hentet samme dag, så nyhedsgrad kan ikke adskille dem.

6. **Per-kommando-overfladematrix.** Anthropic publicerer **ingen** tabel over hvilke kommandoer der virker i CLI vs. desktop vs. web vs. IDE. Kolonnen “Overflade” i oversigtstabellen er **udledt af de dokumenterede regler** (argumentform vs. dialogform) plus de eksplicit navngivne undtagelser. Enkelte celler kan derfor være forkerte for kommandoer dokumentationen ikke nævner ved navn.

7. **IDE-extensions.** Der findes ingen dokumenteret slash command-liste for VS Code- eller JetBrains-extensionerne. Kun `/bug` og Focus-visningen er bekræftet som extension-specifikke afvigelser.

8. **`$0` vs. `$1`.** Substitutionstabellen siger `$0` = første argument. Dette afviger fra shell-konventionen og fra megen tredjepartsdokumentation. Jeg gengiver primærkilden, men anbefaler at teste.

9. **Fjernede kommandoer uden deprecation-note.** `/output-style`, `/statusline`, `/migrate-installer` og `/sandbox` findes ikke i den aktuelle dokumentation. Om de er fjernet, omdøbt eller blot udokumenterede, fremgår ikke — der er ingen note om dem.

---

## Alle kilder

| Emne | URL | Hentet |
|---|---|---|
| Kommandoreference | https://code.claude.com/docs/en/commands | 2026-09-05 |
| Skills (inkl. custom commands, frontmatter, argumenter) | https://code.claude.com/docs/en/skills | 2026-09-05 |
| Slash commands (omdirigerer nu til skills) | https://code.claude.com/docs/en/slash-commands | 2026-09-05 |
| Desktop-app | https://code.claude.com/docs/en/desktop | 2026-09-05 |
| Claude Code on the web | https://code.claude.com/docs/en/claude-code-on-the-web | 2026-09-05 |
| Routines (`/schedule`) | https://code.claude.com/docs/en/routines | 2026-09-05 |
| MCP | https://code.claude.com/docs/en/mcp | 2026-09-05 |
| MCP quickstart | https://code.claude.com/docs/en/mcp-quickstart | 2026-09-05 |
| Plugins | https://code.claude.com/docs/en/plugins | 2026-09-05 |
| Interactive mode (genveje) | https://code.claude.com/docs/en/interactive-mode | 2026-09-05 |
| Agent SDK — skills og kommandoer | https://code.claude.com/docs/en/agent-sdk/slash-commands | 2026-09-05 |
| Changelog (versionsanker) | https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md | 2026-09-05 |
| Dokumentationsindeks | https://code.claude.com/docs/llms.txt | 2026-09-05 |

Den gamle adresse `https://docs.anthropic.com/en/docs/claude-code/slash-commands` svarer **301 Moved Permanently** → `https://code.claude.com/docs/en/slash-commands`. Brug `code.claude.com` som primærkilde.
