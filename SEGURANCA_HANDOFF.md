# SEGURANCA_HANDOFF — Preparação para Vistoria de Segurança (camDu Pro 3.0)

> Este documento prepara o projeto para a vistoria de segurança aprofundada (Seção 8 do `android-builder`). O app não é declarado "seguro" antes da auditoria formal.

## 1. Segredos e Credenciais (Lição L007)
- Nenhuma senha, chave ou token está fixado no código.
- `CameraEntry.toStorageJson()` separa o `userInfo` (`usuario:senha`) da URL RTSP antes de salvar em `SharedPreferences`, gravando a URL limpa (`rtsp://192.168.0.4:554/onvif1`) e o token separado/ofuscado em `authToken`.
- `YooseeOnvifHelper.redactCredentials()` oculta usuário e senha (`***:***@`) em todas as mensagens de erro e telas.
- **Para a vistoria**: migrar o armazenamento ofuscado de `SharedPreferences` para `flutter_secure_storage` (Android Keystore / `EncryptedSharedPreferences`).

## 2. Permissões e Componentes Exportados
- Permissões restritas ao mínimo necessário (`INTERNET`, `ACCESS_NETWORK_STATE`, `ACCESS_WIFI_STATE`, `CHANGE_WIFI_MULTICAST_STATE`, `CAMERA` para QR Code, `WAKE_LOCK`, `VIBRATE`).
- `android:allowBackup="false"` ativo no `AndroidManifest.xml`.
- Apenas `MainActivity` possui `android:exported="true"`.

## 3. Rede (`network_security_config.xml`)
- Configurado `@xml/network_security_config` com `cleartextTrafficPermitted="false"` no `base-config` (exigindo HTTPS para provedores de nuvem pública) e exceção para IPs de câmeras na rede local.

## 4. Assinatura de Release
- `buildTypes.release` usa `signingConfigs.debug` temporariamente para permitir instalação direta do APK gerado no GitHub Actions. Antes de publicar na Google Play, configurar keystore própria fora do repositório (`key.properties` já ignorado no `.gitignore`).
