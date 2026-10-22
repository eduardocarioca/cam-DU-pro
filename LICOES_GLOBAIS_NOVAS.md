# Novas Lições Globais para Incorporar ao Kit (`aprendizados/LICOES_GLOBAIS.md`)

Copie as entradas abaixo para o final de `C:\Users\SEU_NOME\android-builder-ia\aprendizados\LICOES_GLOBAIS.md`:

### L008 — Divergência entre `pubspec.yaml` e `pubspec.lock` quebra builds limpos
- Data: 2026-10-19   Contexto: projetos Flutter / Dart
- Sintoma: `flutter pub get` falha no CI porque o `pubspec.yaml` pede uma versão inexistente (`^2.0.1`) diferente da versão real travada no `pubspec.lock` (`1.2.5`).
- Causa raiz: edição manual do `pubspec.yaml` sem validar contra o `pubspec.lock`.
- Solução / regra: na varredura inicial (Fase 0.2), sempre comparar as versões do `pubspec.yaml` com o `pubspec.lock`.
- Validade: atemporal
- Confirmações: 1   Origem: revisão estática

### L009 — O `compileSdk` do app deve acompanhar o maior `compileSdk` exigido pelos plugins
- Data: 2026-10-19   Contexto: projetos Android / Flutter com plugins nativos
- Sintoma: falha no Gradle quando um plugin transitivo exige `compileSdk 36` e o `app/build.gradle` ou o CI está em `compileSdk 34`.
- Causa raiz: plugins nativos exigem APIs Android mais novas que o projeto raiz.
- Solução / regra: alinhar o `compileSdk` do `app/build.gradle` e do CI ao maior SDK exigido pelos plugins e usar `android.suppressUnsupportedCompileSdk=34,35,36` no `gradle.properties`.
- Validade: depende de versões (verificado em 2026-10)
- Confirmações: 1   Origem: revisão estática
