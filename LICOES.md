# Lições do Projeto — camDu Pro

### L008 — Divergência entre `pubspec.yaml` e `pubspec.lock` com versão inexistente
- Data: 2026-10-19   Contexto: app Flutter (`camDu Pro`)
- Sintoma: `pubspec.yaml` declarava `media_kit_video: ^2.0.1` (inexistente no pub.dev), enquanto o `pubspec.lock` usava `1.2.5`.
- Causa raiz: edição manual do `pubspec.yaml` sem confrontar com o `pubspec.lock`.
- Solução / regra: comparar sempre `pubspec.yaml` e `pubspec.lock` na Fase 0.2 e usar a versão real (`1.2.5`).
- Validade: atemporal
- Confirmações: 1   Origem: revisão estática

### L009 — Plugins nativos (`media_kit_libs_android_video`) impõem o piso do `compileSdk`
- Data: 2026-10-19   Contexto: Flutter 3.24.3 com `media_kit_libs_android_video 1.3.8`
- Sintoma: plugin exige `compileSdk 36` e NDK `26.1.10909125`, mas o app e o CI usavam SDK 34 e a flag `suppressUnsupportedCompileSdkWarning` estava com nome incorreto.
- Causa raiz: plugin nativo transitivo exige API 36.
- Solução / regra: alinhar `compileSdk 36`, `targetSdk 36` e `ndkVersion "26.1.10909125"` no app e no CI, e usar `android.suppressUnsupportedCompileSdk=34,35,36`.
- Validade: depende de versões (verificado em 2026-10)
- Confirmações: 1   Origem: revisão estática

### L010 — Câmeras Wi-Fi Yoosee exigem `/onvif1` ou `/onvif2` e Conexão NVR ativa
- Data: 2026-10-19   Contexto: câmeras Wi-Fi Yoosee (`192.168.0.4`, firmware `40.1.22`)
- Sintoma: abrir `rtsp://IP:554/` ou digitar apenas o IP falhava.
- Causa raiz: câmeras Yoosee servem vídeo em `/onvif1` (HD) e `/onvif2` (SD) após ativar `Conexão NVR`.
- Solução / regra: aceitar IPv4 puro, montar `rtsp://admin:SENHA@IP:554/onvif1` e testar rotas de fallback automaticamente.
- Validade: atemporal
- Confirmações: 1   Origem: informado pelo usuário e revisão estática

### L012 — JBR 25 de builds Canary do Android Studio incompatível com Gradle 8.6
- Data: 2026-10-20   Contexto: Android Studio com JBR 25.0.3 e Gradle 8.6
- Sintoma: `Gradle build failed due to Java/Gradle incompatibility. The Java version used for the build is 25.0.3, which is incompatible with Gradle 8.6.`
- Causa raiz: instalação do Android Studio em canal canary/dev traz o JBR 25, enquanto o Gradle 8.6 suporta até Java 21.
- Solução / regra: rodar com Flutter Web (`flutter run -d chrome`) ou apontar o `flutter config --jdk-dir` para um JDK 17.
- Validade: depende de versões (verificado em 2026-10)
- Confirmações: 1   Origem: informado pelo usuário

### L013 — Formulários de cadastro devem iniciar completamente limpos
- Data: 2026-10-20   Contexto: tela de inclusão de câmeras IP
- Sintoma: campos vinham pré-preenchidos com valores de teste ("Pereira Pacheco", "192.168.0.4", "admin"), causando confusão para cadastrar novas câmeras.
- Causa raiz: valores de protótipo deixados nos controllers e botões de preset forçando mock data.
- Solução / regra: todos os `TextEditingController` de novos cadastros devem iniciar com `""` e apenas `hintText` exemplificativo.
- Validade: atemporal
- Confirmações: 1   Origem: informado pelo usuário

### L014 — Scanner de QR Code no Flutter Web exige ZXing e alternativa de inserção direta
- Data: 2026-10-20   Contexto: mobile_scanner no Flutter Web (Google Chrome)
- Sintoma: leitor não abria ou ficava travado em navegadores desktop devido a permissões de webcam ou foco de câmera fixa.
- Causa raiz: navegadores desktop usam webcam frontal e exigem script ZXing no `index.html`, além de muitas vezes terem câmeras com foco ruim para ler telas de celular.
- Solução / regra: incluir `web/index.html` com ZXing, botão `switchCamera()` e botão de colar/digitar o código QR diretamente.
- Validade: atemporal
- Confirmações: 1   Origem: informado pelo usuário e revisão técnica

### L015 — Controladores de câmera Web devem inicializar estritamente na tela ativa
- Data: 2026-10-21   Contexto: `mobile_scanner` no Flutter Web (Google Chrome)
- Sintoma: leitor de QR Code não abre ou apresenta tela preta/congelada ao entrar na aba de Scanner.
- Causa raiz: instanciar `MobileScannerController(autoStart: true)` no estado global ou na inicialização do aplicativo tenta capturar a webcam em segundo plano antes da interação do usuário e antes da renderização do elemento de vídeo na DOM, gerando rejeição pelo navegador.
- Solução / regra: isolar o leitor em um `StatefulWidget` filho (`QrCameraScannerView`) que inicializa o controller no `initState()` e o destrói no `dispose()`, garantindo que a câmera só seja solicitada quando o usuário estiver com a aba aberta e oferecendo alternativa de entrada manual.
- Validade: atemporal
- Confirmações: 1   Origem: informado pelo usuário e revisão técnica

### L016 — Layout Bento Box e Modo Claro/Escuro sem perda de controles de hardware
- Data: 2026-10-21   Contexto: UI moderna (Material 3 Bento Box) em app de vigilância RTSP
- Sintoma: designs modernos de protótipo costumam simplificar elementos e ocultar controles avançados de hardware (PTZ, REC verificável, troca de transporte TCP/UDP, seleção de stream HD/SD).
- Causa raiz: necessidade de integrar estética clean (cards arredondados, paleta moderna #F3F4F6/#111827, botões circulares) com um player RTSP/ONVIF completo.
- Solução / regra: integrar o player nativo (`CameraPlayerTile`) diretamente no Card Hero da câmera ativa com badges ao vivo (`AO VIVO`, `REC`, escudo de guarda, timestamp dinâmico) e botões circulares de ação rápida, associar os 4 cards da Bento Box às funcionalidades reais (Gestão de Câmeras, Log de Eventos, Status/Transporte e Galeria de Gravações) e oferecer botão de alternância para o modo grade multitelas tradicional.
- Validade: atemporal
- Confirmações: 1   Origem: executado e validado

### L017 — Avatares com `NetworkImage` externo falham por CORS no Flutter Web e exigem fallback nativo
- Data: 2026-10-21   Contexto: `CircleAvatar(backgroundImage: NetworkImage(...))` no Google Chrome
- Sintoma: O avatar no canto superior direito aparecia como um círculo totalmente branco na tela.
- Causa raiz: O navegador bloqueia ou falha ao decodificar imagens externas sem cabeçalhos CORS no renderer Web, deixando o `CircleAvatar` sem conteúdo renderizado.
- Solução / regra: Usar `Icon` vetorial nativo (Genérico, Masculino, Feminino) ou iniciais com `backgroundColor` contrastante como padrão, e envolver qualquer `Image.network` opcional com `errorBuilder` que retorna o ícone nativo.
- Validade: atemporal
- Confirmações: 1   Origem: informado pelo usuário via captura de tela no Chrome

### L018 — QR Codes de compartilhamento (`share.yoosee.com?...InviteCode=...`) são links de convite, não streams RTSP diretos
- Data: 2026-10-21   Contexto: Leitura de QR Code de câmeras Wi-Fi no app `camDu Pro`
- Sintoma: Ao ler o QR Code gerado pelo app da câmera (`https://share.yoosee.com/share/index.html?Type=2&InviteCode=...`), o endereço web entrava direto como URL de vídeo e o botão Adicionar exigia preenchimento manual de nome ou falhava ao abrir como mídia RTSP.
- Causa raiz: O QR Code de compartilhamento contém um `InviteCode`/ID de nuvem em uma URL HTTPS de convite, não o IP local RTSP (`rtsp://IP:554/onvif1`).
- Solução / regra: Extrair automaticamente o `InviteCode` como `deviceId` da câmera, preencher nome padrão (`Câmera N`) caso o campo nome esteja em branco para não bloquear a adição, e oferecer botão de 1 clique na tela da câmera (`Definir IP Local 000.000.0.00`) para vincular o stream RTSP local.
- Validade: atemporal
- Confirmações: 1   Origem: informado pelo usuário via captura de tela
