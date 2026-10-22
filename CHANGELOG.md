# Changelog

## 3.0.0+4 — Versão Completa (Yoosee + AlfredCamera + Nuvem Real)

- **Build Android & CI Corrigidos**:
  - Corrigida dependência inexistente `media_kit_video: ^2.0.1` para `1.2.5`.
  - Alinhados `compileSdk 36`, `ndkVersion "26.1.10909125"` e `android.suppressUnsupportedCompileSdk=34,35,36` em `android/` e no workflow `.github/workflows/build.yml`.
- **Conexão Yoosee / ONVIF e Descoberta LAN**:
  - Suporte direto a IP puro (`192.168.0.4`) com montagem automática de URL `/onvif1` e `/onvif2`.
  - Fallback automático entre caminhos RTSP (`/onvif1`, `/onvif2`, `/live/ch0`, `/stream1`, Intelbras, Hikvision).
  - Scanner de rede Wi-Fi local para descobrir câmeras nas portas `554`, `8554`, `5000`, `8899` e `8080`.
  - Controle PTZ via ONVIF (`ContinuousMove` / `Stop`), comando `Reiniciar a Câmera` (`SystemReboot`) e sincronização de fuso horário (`America/Sao_Paulo GMT-03:00`).
- **Telas e Funções Finais (baseadas nos prints do Yoosee e no AlfredCamera)**:
  - Tela **Informações do Dispositivo** (Nome, ID `5493631173`, Firmware `40.1.22`, IP `192.168.0.4`, MAC `84:fc:14:89:54:88`, Fuso Horário e Código QR).
  - Tela **Compartilhar Dispositivo** com modal **Configurar Permissões de Novo Visitante** (Ver imagem, Controlar PTZ, Conteúdo gravado).
  - Tela **Detecção Inteligente** com **Período de Guarda (`20:00 - Segundo dia 08:00`)**, sensibilidade (`Baixa`, `Média`, `Alta`), detecção/rastreamento de pessoas, luz branca e alarme de voz.
  - Tela **Mais Configurações** com detalhes de **Conexão NVR** e chave **Registro de Nuvem**.
  - Filtro de **Visão Noturna / Baixa Luz**, captura de **Foto (Snapshot PNG)** e botão **Salvar Clipe Agora**.
  - Aba **Gravações** com reprodutor local de segmentos `.mkv`, visualizador de fotos `.png` e histórico de eventos.
- **Armazenamento em Nuvem Gratuito Real**:
  - Implementados `WebDavCloudStorageProvider` (Nextcloud/ownCloud/Koofr/NAS), `S3CloudStorageProvider` com assinatura AWS V4 (Cloudflare R2 / Backblaze B2) e `HttpWebhookStorageProvider`.
