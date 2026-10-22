# camDu Pro 3.0

Aplicativo Flutter completo e sem anúncios para monitoramento de câmeras Wi-Fi IP (**Yoosee / ONVIF / RTSP / HTTP**), com **Controle PTZ**, **Detecção Inteligente (Guarda 20:00–08:00)**, **Gravação Contínua Verificável (.mkv)**, **Compartilhamento com Permissões de Visitante** e **Armazenamento em Nuvem Gratuito Plugável (WebDAV, Cloudflare R2 / Backblaze B2 S3 e Servidor HTTP)**.

## Principais Recursos (Versão 3.0)

- **Conexão direta com câmeras Yoosee e ONVIF**:
  - Aceita apenas o IP da câmera (ex: `192.168.0.4`) e monta automaticamente `rtsp://admin:SENHA@192.168.0.4:554/onvif1`.
  - Fallback automático entre os caminhos RTSP mais usados (`/onvif1`, `/onvif2`, `/live/ch0`, `/stream1`, Intelbras e Hikvision).
  - Alternância rápida **HD (`/onvif1`) ↔ SD (`/onvif2`)** diretamente no cartão da câmera.
  - **Scanner Wi-Fi LAN**: descobre automaticamente câmeras na rede local nas portas `554`, `8554`, `5000`, `8899` e `8080`.
- **Controles Ao Vivo e Tela Cheia (estilo Yoosee + AlfredCamera)**:
  - **Controle PTZ ONVIF**: movimentação direcional (Cima, Baixo, Esquerda, Direita) via protocolo ONVIF.
  - **Filtro de Visão Noturna / Ajuste de Luz**: realce de brilho e contraste em tempo real para ambientes escuros.
  - **Foto Instantânea (Snapshot PNG)** e botão **Salvar Clipe Agora**.
  - **Controle de Áudio (Mudo/Som)** e ocultação automática de senhas nas mensagens de erro.
- **Central de Configurações da Câmera (fiel ao Yoosee)**:
  - **Informações do Dispositivo**: Nome (`Pereira Pacheco`), ID (`5493631173`), Firmware (`40.1.22`), IP Local (`192.168.0.4`), MAC (`84:fc:14:89:54:88`), Fuso Horário (`America/Sao_Paulo GMT-03:00` com sincronização ONVIF) e Código QR.
  - **Compartilhar Dispositivo**: configuração das permissões de visitante (*Ver a imagem da câmera*, *Controlar câmera PTZ*, *Conteúdo gerado pela câmera*) com geração de QR Code.
  - **Detecção Inteligente**: período de guarda configurável (`20:00 - Segundo dia 08:00`), sensibilidade (`Baixa`, `Média`, `Alta`), detecção/rastreamento de pessoas, luz branca e alarme de voz/sirene.
  - **Mais Configurações**: detalhes de `Conexão NVR`, chave `Registro de Nuvem` e comando `Reiniciar a Câmera` via ONVIF (`SystemReboot`).
- **Armazenamento em Nuvem Gratuito**:
  - **WebDAV**: Nextcloud, ownCloud, Koofr ou NAS doméstico.
  - **S3-Compatível (AWS Signature V4)**: Cloudflare R2 (10 GB grátis) e Backblaze B2 (10 GB grátis).
  - **Servidor HTTP / Webhook Próprio**.
  - **Somente Local**: grava segmentos MKV de 5 minutos e fotos PNG no aparelho, com reprodução integrada na aba **Gravações**.
