# FUC Community — Fate/unlimited codes, 100% native

[English](#english) · [Español](#español) · [Português](#português)

**EN** — A 100% native Windows version of the PS2 release of *Fate/unlimited codes*: the game's own code,
statically recompiled, with a native renderer that does not depend on PS2 hardware or software.

**ES** — Una versión nativa al 100% para Windows de la edición de PS2 de *Fate/unlimited codes*: el código del propio
juego, recompilado estáticamente, con un renderizador nativo que no depende del hardware ni del software de PS2.

**PT** — Uma versão 100% nativa para Windows da edição de PS2 de *Fate/unlimited codes*: o código do próprio jogo,
recompilado estaticamente, com um renderizador nativo que não depende do hardware nem do software do PS2.

> This repository contains **no game files and no code**: only the project's description and its progress figures.
> The PSP-based version remains at [fuc-recomp](https://github.com/elprogramadorloco-arch/fuc-recomp).

<!-- fuc-observer:progress:start -->
## Progress

![route frames](badges/route_frames.svg)
![first failure subsystems](badges/first_failure_subsystems.svg)
![layer vm](badges/layer_vm.svg)
![regression tests](badges/regression_tests.svg)

![FUC progress](progress.svg)

| Figure | Value | As of |
|---|---|---|
| Route frames native | 3226 / 3232 (99.81 %) | 2026-10-29 |
| Effect programs statically eligible | 277 / 443 (62.53 %) | 2026-10-29 |
| Unknown closure | 41 / 128 (32.03 %) | 2026-10-29 |
| Effect VM semantic support | 441 / 443 (99.55 %) | 2026-10-29 |
| Catalog support | 432 / 443 (97.52 %) | 2026-10-29 |
| Regression tests passing | 379 / 379 (100.00 %) | 2026-10-29 |
| Frozen milestones | 76 | 2026-10-29 |

### How to read the panels

Each panel is a treemap: every large box is a group whose **area is proportional to how many items it holds**, and
every small square is one item (or a fixed number of items, noted as "one cell = N"), coloured by its state:

| Colour | Meaning |
|---|---|
| 🟩 green | supported / native / pass |
| 🟨 yellow | ready or static only (not yet proven at run time) |
| 🟧 orange | fallback / needs review |
| 🟥 red | blocked / fail |
| 🟪 purple | unknown or not classified by the project's data |
| ⬜ grey | not evaluated |

The percentage in each label is the share of green squares in that box. The chips at the top summarise the panels.

**1. Route frames: native vs fallback.** Each box is a measured game route (a fixed sequence the project runs, e.g.
GSH, GCU, GGA). Squares are frames from the newest run of that route: green frames were drawn entirely by the native
renderer, orange frames fell back to the legacy path. This is the most direct measure of run-time progress. The squares
show proportions, not the order of the frames.

**2. Effect programs by semantic layer.** Every box holds the same effect programs of the game, measured against a
different layer of the port (VM semantics, resource catalog, element registers R01, K01/K02 persistence, late binding
LB01, end-of-pass texture rule EOPT, local lights LT01, and static eligibility). The layers are independent, so they are
never added together; a program is only fully native when every layer it needs is green. Purple usually means that a
layer was never evaluated for that program, not that it failed. Per program and layer, the newest project census that
reports it is used.

**3. Effect programs by first-failure subsystem.** The same programs, grouped by the subsystem that blocks each one
first (NONE = no static blocker left). Green squares are programs that are statically eligible for the native path.
The biggest non-green box is the subsystem whose fix would unlock the most programs.

**Table.** Headline figures exactly as the project records them (catalog completeness, VM semantic support, closed
unknowns, regression tests, frozen milestones). When a figure appears both here and in a panel, the table is
authoritative: some milestones publish totals without updating every program.

_Updated 2026-10-29T23:28:44Z. Aggregate figures only (counts and percentages recorded by the project's own artifacts), exported by FUC Observer. No game data, file names or paths are published. The per-program layer panel composes the newest project census per program; where a milestone published only totals, the table above is authoritative. Raw data: [progress.json](progress.json)._
<!-- fuc-observer:progress:end -->

---

## English

### Development status

The native PS2 port is in active development. There is **no public build yet**: this repository only publishes the
project's description and its progress, which is exported automatically from the project's own records (see
**Progress** above). When the port is ready, releases will be announced here.

The PSP-based Windows version, with its build instructions and releases, remains available at
[fuc-recomp](https://github.com/elprogramadorloco-arch/fuc-recomp) and is not affected by this repository.

### What is this project?

FUC (*Fate/unlimited codes*) Community tracks a native Windows version of the PS2 release of the game. The goal is a
build that runs the original game's own logic as native code, with no emulator, no PS2 hardware model and no PS2
software in the loop.

- **Static recompilation.** The game's executable is translated ahead of time into native code with **PS2Recomp**
  (an open-source static recompiler, GPLv3), so the original game logic runs as compiled code instead of being
  interpreted.
- **Native rendering.** Frames are drawn by a native renderer that reproduces what the game asks for: models, stages,
  fighters, effects, lights and fog. The original PS2 graphics path is kept only as a safety net while each part is
  rebuilt.
- **Fail closed.** A frame is drawn natively only when every part of it is proven to match the original game's
  behaviour. If anything is unknown, that frame safely falls back to the original path instead of being drawn wrong.
- **Semantic recovery.** Each subsystem (the effect virtual machine, resource catalogs, material and texture state,
  late-bound resources, local lights, colour and fog rules…) is reconstructed from the game's own behaviour and only
  then made native.
- **Verification.** Results are checked against the original game's behaviour, using PCSX2 as a read-only reference,
  and by large regression suites. Finished work is frozen and protected by guards so later work cannot alter it.

### How to read the progress

- **Route frames native** is the most direct measure: the share of frames of real battles drawn entirely by the native
  renderer.
- **Effect programs** shows how many of the game's visual effect programs are supported layer by layer (VM semantics,
  catalog, persistence rules, late binding, lights) and what blocks the rest.
- **Regression tests** shows that what already works keeps working.
- The colour legend and panel guide are in the **Progress** section above. All figures are aggregates (counts and
  percentages); no game data is published.

### Requirements

None yet: there is nothing to download. When a build is released, it will require your own lawfully obtained copy of
the PS2 game, and the private build will be generated locally from it, as with the PSP version.

### Roadmap

| Step | Status | Note |
| --- | --- | --- |
| Static recompilation of the game executable | In progress | Boots and runs the measured battle routes as native code. |
| Native renderer for battles | In progress | See "Route frames native" above; the remaining frames fall back safely. |
| Effect programs (VM, catalog, persistence, lights) | In progress | Tracked program by program in the Progress panels. |
| Menus, loading screens and other scenes | In progress | Covered progressively after battles. |
| Removal of the PS2 graphics fallback | Planned | When every measured route reaches 100% native frames. |
| Public build and instructions | Planned | Only when everything is ready; it will be announced here. |

### Legal notice

This is an interoperability and preservation project and this text is not legal advice. It is not affiliated with or
endorsed by Capcom, Type-Moon, Cavia, Sony, or any other rightsholder. All trademarks and copyrighted game material
belong to their respective owners.

This repository does not and will not contain or redistribute game discs or images, executables, BIOS files, extracted
game files, audio, video, textures, models, fonts, save data, generated recompiled code or any build incorporating
game-derived code. The project works only with the user's own legitimately obtained copy; owning a copy does not grant
redistribution rights. PS2Recomp is GPLv3 and keeps its own license and notices. PCSX2 is used only as a read-only
reference for verification and is not part of the project.

---

## Español

### Estado del desarrollo

El port nativo de la versión de PS2 está en desarrollo activo. **Todavía no hay una versión pública**: este repositorio
solo publica la descripción del proyecto y su progreso, que se exporta automáticamente desde los propios registros del
proyecto (ver **Progress** arriba). Cuando el port esté listo, las versiones se anunciarán aquí.

La versión para Windows basada en PSP, con sus instrucciones y releases, sigue disponible en
[fuc-recomp](https://github.com/elprogramadorloco-arch/fuc-recomp) y este repositorio no la afecta.

### ¿Qué es este proyecto?

FUC (*Fate/unlimited codes*) Community sigue el desarrollo de una versión nativa para Windows de la edición de PS2 del
juego. El objetivo es una versión que ejecute la lógica original del juego como código nativo, sin emulador, sin un
modelo del hardware de PS2 y sin software de PS2 de por medio.

- **Recompilación estática.** El ejecutable del juego se traduce por adelantado a código nativo con **PS2Recomp**
  (un recompilador estático de código abierto, GPLv3), de modo que la lógica original se ejecuta como código compilado
  en lugar de interpretarse.
- **Renderizado nativo.** Los fotogramas los dibuja un renderizador nativo que reproduce lo que el juego pide: modelos,
  escenarios, luchadores, efectos, luces y niebla. El camino gráfico original de PS2 se conserva solo como red de
  seguridad mientras se reconstruye cada parte.
- **Fallo seguro (fail closed).** Un fotograma se dibuja de forma nativa solo cuando está demostrado que todas sus
  partes coinciden con el comportamiento original. Si algo es desconocido, ese fotograma vuelve al camino original en
  lugar de dibujarse mal.
- **Recuperación semántica.** Cada subsistema (la máquina virtual de efectos, los catálogos de recursos, el estado de
  materiales y texturas, los recursos enlazados tarde, las luces locales, las reglas de color y niebla…) se reconstruye
  a partir del comportamiento del propio juego y solo entonces pasa a ser nativo.
- **Verificación.** Los resultados se comparan con el comportamiento del juego original, usando PCSX2 como referencia
  de solo lectura, y con grandes baterías de pruebas de regresión. El trabajo terminado se congela y queda protegido
  por guardas para que el trabajo posterior no pueda alterarlo.

### Cómo leer el progreso

- **Route frames native** es la medida más directa: el porcentaje de fotogramas de combates reales que dibuja por
  completo el renderizador nativo.
- **Effect programs** muestra cuántos programas de efectos visuales del juego están soportados capa por capa (semántica
  de la VM, catálogo, reglas de persistencia, enlace tardío, luces) y qué bloquea al resto.
- **Regression tests** muestra que lo que ya funciona sigue funcionando.
- La leyenda de colores y la guía de los paneles están en la sección **Progress** de arriba. Todas las cifras son
  agregadas (conteos y porcentajes); no se publica ningún dato del juego.

### Requisitos

Por ahora ninguno: no hay nada que descargar. Cuando se publique una versión, hará falta tu propia copia legítima del
juego de PS2 y la versión privada se generará localmente a partir de ella, igual que con la versión de PSP.

### Mapa de ruta

| Paso | Estado | Nota |
| --- | --- | --- |
| Recompilación estática del ejecutable del juego | En curso | Arranca y ejecuta como código nativo las rutas de combate medidas. |
| Renderizador nativo para los combates | En curso | Ver "Route frames native" arriba; los fotogramas restantes vuelven al camino seguro. |
| Programas de efectos (VM, catálogo, persistencia, luces) | En curso | Se siguen programa a programa en los paneles de Progress. |
| Menús, pantallas de carga y otras escenas | En curso | Se cubren progresivamente después de los combates. |
| Retirada del camino gráfico de PS2 | Previsto | Cuando todas las rutas medidas lleguen al 100% de fotogramas nativos. |
| Versión pública e instrucciones | Previsto | Solo cuando todo esté listo; se anunciará aquí. |

### Aviso legal

Este es un proyecto de interoperabilidad y preservación, y este texto no es asesoramiento legal. No está afiliado ni
respaldado por Capcom, Type-Moon, Cavia, Sony ni ningún otro titular de derechos. Todas las marcas y el material del
juego protegido por derechos de autor pertenecen a sus respectivos propietarios.

Este repositorio no contiene ni contendrá ni redistribuirá discos o imágenes del juego, ejecutables, BIOS, archivos
extraídos del juego, audio, vídeo, texturas, modelos, fuentes, partidas guardadas, código recompilado generado ni
ninguna versión que incorpore código derivado del juego. El proyecto trabaja solo con la copia propia y legítima del
usuario; tener una copia no concede derechos de redistribución. PS2Recomp es GPLv3 y conserva su propia licencia y
avisos. PCSX2 se usa solo como referencia de solo lectura para verificar y no forma parte del proyecto.

---

## Português

### Estado do desenvolvimento

O port nativo da versão de PS2 está em desenvolvimento ativo. **Ainda não há uma versão pública**: este repositório
publica apenas a descrição do projeto e o seu progresso, exportado automaticamente a partir dos próprios registros do
projeto (veja **Progress** acima). Quando o port estiver pronto, as versões serão anunciadas aqui.

A versão para Windows baseada no PSP, com as suas instruções e releases, continua disponível em
[fuc-recomp](https://github.com/elprogramadorloco-arch/fuc-recomp) e não é afetada por este repositório.

### O que é este projeto?

FUC (*Fate/unlimited codes*) Community acompanha o desenvolvimento de uma versão nativa para Windows da edição de PS2
do jogo. O objetivo é uma versão que execute a lógica original do jogo como código nativo, sem emulador, sem um modelo
do hardware do PS2 e sem software do PS2 no meio.

- **Recompilação estática.** O executável do jogo é traduzido antecipadamente para código nativo com o **PS2Recomp**
  (um recompilador estático de código aberto, GPLv3), de modo que a lógica original é executada como código compilado
  em vez de ser interpretada.
- **Renderização nativa.** Os quadros são desenhados por um renderizador nativo que reproduz o que o jogo pede:
  modelos, cenários, lutadores, efeitos, luzes e névoa. O caminho gráfico original do PS2 é mantido apenas como rede de
  segurança enquanto cada parte é reconstruída.
- **Falha segura (fail closed).** Um quadro só é desenhado de forma nativa quando está provado que todas as suas partes
  coincidem com o comportamento original. Se algo for desconhecido, esse quadro volta ao caminho original em vez de ser
  desenhado errado.
- **Recuperação semântica.** Cada subsistema (a máquina virtual de efeitos, os catálogos de recursos, o estado de
  materiais e texturas, os recursos ligados tardiamente, as luzes locais, as regras de cor e névoa…) é reconstruído a
  partir do comportamento do próprio jogo e só então passa a ser nativo.
- **Verificação.** Os resultados são comparados com o comportamento do jogo original, usando o PCSX2 como referência
  somente leitura, e com grandes baterias de testes de regressão. O trabalho concluído é congelado e protegido por
  verificações para que o trabalho posterior não possa alterá-lo.

### Como ler o progresso

- **Route frames native** é a medida mais direta: a porcentagem de quadros de combates reais desenhados inteiramente
  pelo renderizador nativo.
- **Effect programs** mostra quantos programas de efeitos visuais do jogo são suportados camada por camada (semântica
  da VM, catálogo, regras de persistência, ligação tardia, luzes) e o que bloqueia o restante.
- **Regression tests** mostra que o que já funciona continua funcionando.
- A legenda de cores e o guia dos painéis estão na seção **Progress** acima. Todos os números são agregados (contagens
  e porcentagens); nenhum dado do jogo é publicado.

### Requisitos

Por enquanto nenhum: não há nada para baixar. Quando uma versão for publicada, será necessária a sua própria cópia
legítima do jogo de PS2, e a versão privada será gerada localmente a partir dela, como na versão de PSP.

### Roteiro

| Etapa | Estado | Nota |
| --- | --- | --- |
| Recompilação estática do executável do jogo | Em andamento | Inicia e executa como código nativo as rotas de combate medidas. |
| Renderizador nativo para os combates | Em andamento | Veja "Route frames native" acima; os quadros restantes voltam ao caminho seguro. |
| Programas de efeitos (VM, catálogo, persistência, luzes) | Em andamento | Acompanhados programa a programa nos painéis de Progress. |
| Menus, telas de carregamento e outras cenas | Em andamento | Cobertos progressivamente depois dos combates. |
| Remoção do caminho gráfico do PS2 | Planejado | Quando todas as rotas medidas chegarem a 100% de quadros nativos. |
| Versão pública e instruções | Planejado | Apenas quando tudo estiver pronto; será anunciado aqui. |

### Aviso legal

Este é um projeto de interoperabilidade e preservação, e este texto não é aconselhamento jurídico. Não é afiliado nem
endossado pela Capcom, Type-Moon, Cavia, Sony ou qualquer outro titular de direitos. Todas as marcas e o material do
jogo protegido por direitos autorais pertencem aos seus respectivos proprietários.

Este repositório não contém nem conterá nem redistribuirá discos ou imagens do jogo, executáveis, BIOS, arquivos
extraídos do jogo, áudio, vídeo, texturas, modelos, fontes, dados salvos, código recompilado gerado ou qualquer versão
que incorpore código derivado do jogo. O projeto trabalha apenas com a cópia própria e legítima do usuário; possuir uma
cópia não concede direitos de redistribuição. O PS2Recomp é GPLv3 e mantém a sua própria licença e avisos. O PCSX2 é
usado apenas como referência somente leitura para verificação e não faz parte do projeto.
