# portal-academico-palmas-dados

Dados **oficiais** dos Contatos e Setores do Portal de Serviços Acadêmicos — IFTO Campus Palmas (contrato `diretorio.v1`).

| Ramo | Uso | Quem grava |
|---|---|---|
| `previa` | conteúdo publicado pela planilha oficial para conferência na prévia (`previa-contatos.portal-academico-palmas.pages.dev`) | Apps Script: "Publicar prévia" (1 commit por publicação) |
| `main` | conteúdo exibido no Portal oficial | Apps Script: "Promover para o Portal" (avança até o commit conferido, sem reescrita) |

## Regras
- Só dados institucionais públicos e conferidos. Nenhum contato fictício (`teste-…`, `[TESTE]`). O validador recusa.
- Sem segredos. O token de publicação fica só nas propriedades privadas do Apps Script de quem publica.
- `main` e `previa` protegidos contra força-push e exclusão. `main` só avança por fast-forward até um commit de `previa`.
- O build do Portal valida de novo (`--oficial`, mínimo de metade da reserva). Se o arquivo for inválido, o build falha e o Portal mantém a versão anterior.

## Reversão
- **Rápida:** Cloudflare → Deployments → "Rollback" do deployment anterior.
- **Controlada:** `git revert <commit>` em `previa`, depois "Verificar prévia" e "Promover" na planilha.
- **Contingência:** remover `DIRETORIO_FONTE_OFICIAL` do ambiente Production. O Portal volta à reserva estática da `main` do Portal.

## Arquivos
- `diretorio.json`: criado pela primeira publicação da planilha.
- `validar-diretorio.py`: cópia de `scripts/validar-diretorio.py` do Portal.
- `.github/workflows/validar.yml`: validação automática, somente leitura.
