# Guia de Build e Configuração — camDu Pro 3.0

## 1. Correções Aplicadas na Matriz Android / MediaKit

- **Flutter 3.24.3** e **Java 17**.
- **`pubspec.yaml`**: corrigida a versão do `media_kit_video` para `1.2.5` (a versão `^2.0.1` anterior não existe no pub.dev e quebrava o `flutter pub get`), mantido `wakelock_plus: 1.3.3` e `package_info_plus: 8.3.1` em `dependency_overrides`, e adicionado `crypto: ^3.0.3` para assinatura AWS Signature V4 (Cloudflare R2 / Backblaze B2).
- **`android/app/build.gradle`**: atualizado para `compileSdk 36` e `ndkVersion "26.1.10909125"` (exigidos pelo `media_kit_libs_android_video 1.3.8`), mantendo `minSdk 21` e `targetSdk 34`.
- **`android/gradle.properties`**: corrigida a flag `android.suppressUnsupportedCompileSdk=34,35,36` para que o AGP 8.4.2 aceite o `compileSdk 36` sem falhar.
- **`.github/workflows/build.yml`**: configurado para gerar automaticamente o Gradle Wrapper (`gradle-8.6`), instalar `platforms;android-36`, `build-tools;36.0.0` e `ndk;26.1.10909125`, rodar `flutter analyze` e `flutter test`, e gerar os arquivos `app-release.apk` e `app-release.aab`.

## 2. Como Conectar sua Câmera Yoosee ("Pereira Pacheco" — 192.168.0.4)

1. No aplicativo Yoosee original (uma única vez), abra **Configurações > Mais configurações > Conexão NVR** (ou **Conexões > RTSP**) e ative a chave RTSP, definindo uma senha RTSP.
2. Abra o **camDu Pro 3.0** no celular conectado ao mesmo Wi-Fi.
3. Toque em **Configurar Yoosee (Pereira Pacheco • 192.168.0.4)** ou no botão `+`.
4. Informe a senha RTSP no campo **Assistente Yoosee / ONVIF** e salve.
5. O aplicativo montará automaticamente `rtsp://admin:SENHA@192.168.0.4:554/onvif1` e abrirá o vídeo ao vivo sem anúncios.
