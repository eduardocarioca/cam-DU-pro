# VERSOES — Matriz de Versões Anti-Conflito (camDu Pro 3.0)

Status: **Revisão estática concluída (Modo C) — validar via GitHub Actions ou PowerShell local**.

| Item | Versão Fixada | Observação |
| :--- | :--- | :--- |
| **JDK** | `17` (Temurin) | Compatível com Gradle 8.6 e AGP 8.4.2 |
| **Gradle** | `8.6` | Configurado em `gradle-wrapper.properties` e no CI |
| **AGP** | `8.4.2` | Com `android.suppressUnsupportedCompileSdk=34,35,36` |
| **Kotlin** | `1.9.24` | `jvmTarget = '17'` |
| **Flutter** | `3.24.3` (stable) | Dart SDK `>=3.0.0 <4.0.0` |
| **compileSdk / targetSdk / minSdk** | `36` / `36` / `21` | `targetSdk 36` alinhado ao requisito Google Play 2026-10 (L003) |
| **Android NDK** | `26.1.10909125` | Exigido pelos plugins nativos Android |
| **media_kit / media_kit_video / libs** | `1.2.6` / `1.2.5` / `1.0.7` | Corrigido `media_kit_video` de `^2.0.1` (inexistente) para `1.2.5` |
| **Overrides (`wakelock_plus` / `package_info_plus`)** | `1.3.3` / `8.3.1` | Mantêm compatibilidade com Gradle 8.6 / AGP 8.4.2 |
| **crypto** | `^3.0.3` (`3.0.7`) | Assinatura AWS Signature V4 (`HMAC-SHA256`) para S3 R2/B2 |
