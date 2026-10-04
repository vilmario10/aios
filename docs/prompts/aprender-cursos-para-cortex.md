# Prompt — Pipeline "Curso → Cérebro (Cortex)"

> **Como usar:** abra uma sessão nova do Claude Code **no seu Mac** (onde ficam o `xquads-chief-iaqv` e os 11 squads; não use a nuvem, a VPS nem o AIAN) e cole o bloco abaixo inteiro.
> **Modelo da sessão principal:** `/model opus` (Claude Opus 5.5) com esforço `high`.
> **Antes de colar:** preencha os campos `{{...}}`.

---

## Modelos por fase (resumo)

| Fase | Quem executa | Modelo | Por quê |
|------|--------------|--------|---------|
| 0. Inventário e plano | Sessão principal | **Opus 5.5** (`claude-opus-5-5`), esforço high | Decisões de prioridade e de arquitetura |
| 1. Captura do bruto | Sessão principal + ferramentas (Drive, Chrome, Whisper) | Opus 5.5, esforço low | É trabalho mecânico; o modelo só coordena |
| 2. Destilação por aula | Subagentes em paralelo | **Sonnet 5.5** (`claude-sonnet-5-5`) | Volume alto, extração estruturada, custo ~50% do Opus |
| 3. Síntese por curso | Sessão principal | **Opus 5.5**, esforço high/xhigh | Cruzar aulas, detectar contradições, gerar playbooks |
| 4. Validação (gate) | Subagente QA + você | Sonnet 5.5 (checklist) + aprovação humana | Rastreabilidade e utilidade prática |
| 5. Classificação na ontologia | Subagente | **Haiku 4.5** (`claude-haiku-4-5`) | Tarefa curta e repetitiva de tagueamento |
| 6. Atualizar skills dos squads | Sessão principal via `xquads-chief-iaqv` → squad dono | Opus 5.5 | Mexe em ativos que já geram receita |

**Fable 5.1** (`claude-fable-5-1`): use só se a síntese de um curso grande e denso ficar rasa no Opus. Custa 2,5x o Opus.

---

## PROMPT (copiar daqui para baixo)

```
Você vai operar o pipeline "Curso → Cérebro". O objetivo é transformar cursos que comprei em
conhecimento acionável que melhore nossos squads e skills para gerar receita com:
(1) infoprodutos, (2) serviços de IA para PMEs, (3) afiliados/tráfego, (4) agentes de IA que vendem.

REGRAS INEGOCIÁVEIS
- Faça um piloto antes do lote: processe 1 módulo de 1 curso de ponta a ponta, me mostre o resultado e
  espere meu OK antes de escalar.
- Não invente. Cada afirmação no material destilado precisa de fonte: curso › módulo › aula › minuto/página.
  O que for interpretação sua vai marcado como [INFERÊNCIA].
- Nada sobe para o Cortex sem passar pelo gate de validação (Fase 4) e sem minha aprovação explícita.
- Não altere skills nem squads existentes sem me mostrar o diff e receber aprovação.
- Direitos autorais: o cérebro guarda SÍNTESE, frameworks e aplicação, nunca cópia integral de aulas,
  apostilas ou transcrições. O material bruto fica só local, para uso pessoal, e não vai para o git
  nem para o Cortex.
- Não peça, não guarde e não digite minhas senhas. Login em área de membros eu faço no Chrome.
- Se faltar informação, pergunte (no formato 1, 2, 3). Se for dúvida técnica, pesquise na web antes.

CONTEXTO
- Cursos a processar: {{lista de cursos + plataforma de cada um; ex.: "Curso X — Hotmart", "Curso Y — Drive/pasta Z"}}
- Prioridade: {{ordem dos cursos ou "você propõe com base no impacto em receita"}}
- Pasta de trabalho: {{caminho, ex.: ~/hermes-iaquevende-os/cursos/}}
- Segundo cérebro: Cortex, com a ontologia em {{caminho/URL da ontologia do Cortex}}.
- Roteamento de squads: xquads-chief-iaqv (diagnostica e escolhe o squad; ele não executa).
  Squads conhecidos: copy-master, hormozi-squad-iaqv, traffic-masters-iaqv, shopee-squad-iaqv e os demais em ~/.claude/skills/.

PASSO 0 — RECONHECIMENTO (antes de criar qualquer coisa)
1. Liste ~/.claude/skills/ e verifique se já existe skill ou squad de ingestão de conhecimento, curadoria
   ou Cortex. Se existir, reutilize. Não crie duplicata.
2. Leia a ontologia do Cortex inteira e me resuma em até 10 linhas: entidades, relações, onde entram
   frameworks, playbooks, swipe files e fontes.
3. Monte {{pasta}}/_inventario.yaml com: curso, plataforma, formato (vídeo com/sem transcrição, PDF,
   slides, link público), nº de módulos/aulas, foco de monetização (1-4), prioridade e status.
4. Me mostre o inventário e o plano do piloto. ESPERE APROVAÇÃO.

ESTRUTURA DE PASTAS (por curso)
{{pasta}}/{slug-curso}/
  00-meta.yaml            # fonte, autor, data de compra, status, foco de monetização
  01-bruto/               # transcrições, PDFs, slides (LOCAL, adicionar ao .gitignore)
  02-aulas/mXX-aYY.md     # 1 arquivo destilado por aula (Fase 2)
  03-sintese/
    playbook.md           # passo a passo aplicável
    frameworks.md         # modelos mentais e frameworks nomeados
    swipe.md              # modelos, roteiros, checklists reescritos com nossas palavras
    metricas.md           # números, benchmarks e KPIs citados (com fonte)
  04-propostas-squad.md   # o que muda em qual squad/skill (Fase 6)
  05-validacao.md         # checklist do gate + decisão
Status do curso: inventariado → capturado → destilado → sintetizado → validado → no-cortex → aplicado

FASE 1 — CAPTURA DO BRUTO (por tipo de fonte)
- Google Drive: use o conector do Drive para ler PDFs, slides e docs.
- YouTube/links públicos: baixe só as legendas (yt-dlp --write-auto-subs --skip-download). Se não houver
  legenda, transcreva o áudio localmente.
- Vídeos sem transcrição: transcreva LOCAL no Mac com mlx-whisper ou whisper.cpp (sem custo, sem enviar
  áudio para terceiros). Salve .txt com timestamps.
- Área de membros (Hotmart/Kiwify/Eduzz etc.): eu faço login no Chrome. Use o Claude in Chrome para
  navegar e copiar texto, materiais complementares e transcrições que a própria plataforma exibe. Antes de
  baixar vídeo, verifique os termos da plataforma e me avise se houver restrição. Não contorne DRM.
- Registre no 00-meta.yaml o que foi capturado e o que faltou.

FASE 2 — DESTILAÇÃO POR AULA (subagentes Sonnet 5.5, em paralelo, 1 aula por subagente)
Template obrigatório de cada 02-aulas/mXX-aYY.md:
  ## Fonte            curso › módulo › aula › duração
  ## Tese central     1-2 frases
  ## Ideias-chave     bullets com [mm:ss] ou [p. N]
  ## Frameworks       nome, passos, quando usar, quando NÃO usar
  ## Ações práticas   verbos no imperativo, ordenados
  ## Modelos/roteiros reescritos com nossas palavras (não copiar)
  ## Números citados  métrica, valor, contexto, fonte
  ## Aplicação        em qual foco (1-4) e em qual squad isso ajudaria
  ## Dúvidas/lacunas  o que ficou vago ou contraditório

FASE 3 — SÍNTESE POR CURSO (sessão principal, Opus 5.5)
- Cruze todas as aulas, elimine repetição e gere os arquivos de 03-sintese/.
- Liste as contradições internas do curso e os conflitos com o que já existe no Cortex.
- Classifique cada framework: [NOVO] / [REFORÇA EXISTENTE] / [CONTRADIZ EXISTENTE].

FASE 4 — GATE DE VALIDAÇÃO (05-validacao.md)
Checklist (nota 0-2 por item; aprovar com ≥ 12/16 e nenhum item crítico zerado):
  1. [crítico] Toda afirmação tem fonte rastreável
  2. [crítico] Sem cópia integral de conteúdo do curso
  3. Ações são executáveis por PME sem consultoria extra
  4. Sem duplicata do que já está no Cortex
  5. Contradições sinalizadas, não escondidas
  6. Números com contexto (nicho, ticket, período)
  7. Mapeado para ao menos 1 foco de monetização e 1 squad
  8. Classificado na ontologia do Cortex (subagente Haiku 4.5)
Decisão: APROVADO / AJUSTAR / REPROVADO. Me apresente e ESPERE MEU OK.

FASE 5 — PROMOÇÃO AO CORTEX
Só após meu OK: suba 03-sintese/ + metadados no formato da ontologia. Confirme o que foi criado
(IDs/caminhos) e atualize o status para no-cortex.

FASE 6 — APLICAÇÃO NOS SQUADS (onde o dinheiro aparece)
- Para cada proposta em 04-propostas-squad.md, passe pelo xquads-chief-iaqv para identificar o squad
  dono (copy-master, hormozi-squad-iaqv, traffic-masters-iaqv...).
- Proponha a mudança concreta: nova task, novo checklist, prompt ajustado, novo swipe. Mostre o diff.
- Cada mudança aprovada vem com 1 teste real e 1 métrica. Ex.: "nova estrutura de oferta → aplicar no
  produto X → medir conversão da página em 14 dias contra a versão atual".
- Lembrete: depois de editar em hermes-iaquevende-os/skills/, rode sync-pontes.sh. O AIAN e o Neo estão
  desatualizados desde 09/08 e não recebem a mudança sozinhos.

ENTREGA AO FINAL DE CADA CURSO
Resumo de 1 tela: o que aprendemos (top 5), o que mudou em quais squads, testes agendados com métrica,
lacunas e próximo curso da fila. Atualize _inventario.yaml.

Comece pelo PASSO 0.
```
