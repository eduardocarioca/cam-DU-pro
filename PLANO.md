# PLANO — camDu Pro 3.0 (Protocolo android-builder v1.1-U)

## 1. Escopo do Projeto
- **Essencial (Implementado)**:
  1. Player nativo RTSP/RTSPS/HTTP/HTTPS (`media_kit`/MPV) com suporte direto a câmeras Wi-Fi Yoosee/ONVIF (`192.168.0.4`, `/onvif1` HD e `/onvif2` SD), fallback automático de rotas e Scanner Wi-Fi LAN.
  2. Controles ao vivo e tela cheia (estilo Yoosee + AlfredCamera): Joystick PTZ ONVIF, HD/SD, Mudo/Som, Foto (Snapshot PNG), Salvar Clipe Agora e Filtro de Visão Noturna.
  3. Central de Configurações da Câmera fiel aos 7 prints do Yoosee: Informações do Dispositivo (`5493631173`, `192.168.0.4`, `84:fc:14:89:54:88`, `America/Sao_Paulo GMT-03:00`), Compartilhar Dispositivo com Permissões de Visitante, Detecção Inteligente (Guarda `20:00-08:00`), Conexão NVR, Registro de Nuvem e Reiniciar a Câmera (`SystemReboot`).
  4. Gravação contínua verificável em `.mkv` (selo `REC` confirmado por bytes em disco), galeria de gravações/eventos e provedores reais de nuvem gratuita (**WebDAV**, **S3 Cloudflare R2 / Backblaze B2** e **HTTP Webhook**).
- **Desejável (Próxima evolução)**:
  - `ForegroundService` Android para gravação com tela bloqueada e migração do armazenamento ofuscado para `flutter_secure_storage` (Android Keystore).

## 2. Etapas Executadas e Validadas em Revisão Estática
1. **Etapa 1**: Alinhamento da matriz Android (`compileSdk 36`, `targetSdk 36` conforme L003, `ndkVersion 26.1.10909125`, `media_kit_video 1.2.5`).
2. **Etapa 2**: Motor `YooseeOnvifHelper` (normalização de IP, fallback RTSP, Scanner LAN e comandos SOAP ONVIF).
3. **Etapa 3**: `RecordingService` verificável, `UploadQueue` com log de eventos e provedores de nuvem `WebDav`, `S3` (AWS V4) e `HttpWebhook`.
4. **Etapa 4**: Interface completa (telas dos 7 prints do Yoosee, controles estilo AlfredCamera e galeria de gravações locais).
5. **Etapa 5**: Suíte de testes reais em `test/widget_test.dart` (L006), separação de credenciais da URL no `SharedPreferences` (L007) e `SEGURANCA_HANDOFF.md`.
