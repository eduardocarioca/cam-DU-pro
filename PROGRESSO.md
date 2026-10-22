# PROGRESSO — camDu Pro 3.0

## Etapa 1 — Matriz Android, Gradle e CI
- Feito: `pubspec.yaml` corrigido (`media_kit_video: 1.2.5`, `crypto: ^3.0.3`, overrides); `compileSdk 36`, `targetSdk 36` (L003) e `ndkVersion "26.1.10909125"` em `android/app/build.gradle`; `android.suppressUnsupportedCompileSdk=34,35,36` em `gradle.properties`; `.github/workflows/build.yml` completo.
- Build: não executado localmente (Modo C — L005).

## Etapa 2 — Motor Yoosee / ONVIF e Scanner LAN
- Feito: `lib/onvif/yoosee_onvif_helper.dart` com normalização de IP `192.168.0.4`, fallback `/onvif1`/`/onvif2`, scanner de rede e comandos SOAP ONVIF (PTZ, Reboot, TimeSync).
- Testes: coberto em `test/widget_test.dart`.

## Etapa 3 — Gravação Verificável e Provedores de Nuvem
- Feito: `RecordingService` com verificação de bytes em disco antes do selo `REC`, fotos `.png` e clipes manuais; `WebDavCloudStorageProvider`, `S3CloudStorageProvider` e `HttpWebhookStorageProvider`.
- Testes: coberto em `test/widget_test.dart` (dados reais, 0 bytes e falha do backend).

## Etapa 4 — Telas Yoosee + Funções AlfredCamera
- Feito: controles PTZ/Visão Noturna/Foto/Clipe/HD-SD no player, galeria de gravações e todas as telas dos 7 prints do Yoosee.
- Testes: janela de guarda noturna `20:00-08:00` testada em `test/widget_test.dart`.

## Etapa 5 — Segurança (L007) e Documentação do Protocolo
- Feito: `CameraEntry.toStorageJson()` separa `userInfo` da URL ao salvar em `SharedPreferences` (testado em `test/widget_test.dart`); adicionado `network_security_config.xml` e `SEGURANCA_HANDOFF.md`.

## Etapa 6 — Limpeza Completa de Formulários e Isolamento do Scanner QR
- Feito: Removidas todas as strings fixas de teste do código-fonte (`Pereira Pacheco`, `192.168.0.4`, IDs e MACs fictícios nos controllers). O formulário de cadastro agora abre 100% limpo, com campos vazios e hints genéricos (`Ex: Câmera Sala, Garagem...`).
- Scanner QR: Refatorado no componente `QrCameraScannerView` com ciclo de vida estritamente atrelado à montagem da aba de Scanner (evitando bloqueios no Chrome causados por inicialização antecipada fora da tela), suporte a webcam frontal no desktop (`kIsWeb ? CameraFacing.front : CameraFacing.back`), botão de alternância de câmera, mira de enquadramento visual e botão de colar/digitar o código diretamente.
- Build: Revisão estática validada e código 100% limpo de erros de sintaxe e tipos.

## Etapa 7 — Integração do Design Bento Box Moderno + Modo Claro e Escuro
- Feito: 
  1. Suporte completo a Modo Claro (`#F3F4F6` com cartões brancos) e Modo Escuro (`#111827` com cartões `#1F2937`) com alternância em tempo real no cabeçalho e persistência em `SharedPreferences`.
  2. Cabeçalho moderno com título "Monitoramento", indicador de sensores ativo (`Icons.sensors` verde), botão de alternância de tema, botão de adicionar câmera e foto de perfil.
  3. Card Principal Hero ("Câmera ao vivo") com cantos arredondados (raio 24), badge `AO VIVO` vermelho, selo `REC` verificado, relógio de timestamp dinâmico em tempo real, escudo de guarda inteligente e barra de botões circulares rápidos (Tela Cheia, Microfone/Áudio, Play/Pause, Foto Snapshot, Gravação REC, e Controle de Joystick PTZ).
  4. Grid Bento Box 2x2 interativo:
     - Card Câmeras: exibe total de câmeras ativas e abre menu rápido / troca para visualização em grade.
     - Card Eventos: exibe total de alertas e abre o histórico de detecção de movimento.
     - Card Status: exibe status online e permite alternar entre transporte TCP e UDP ou escanear Wi-Fi.
     - Card Gravações: atalho direto para a galeria de vídeos e fotos locais.
  5. Seletor inferior de locais/câmeras: cards horizontais com ponto indicador vermelho ativo para troca instantânea de câmera em destaque no Card Hero + botão "+ Adicionar".
  6. Preservação integral de todas as funções existentes: streaming MediaKit RTSP/ONVIF com fallback, gravação local com verificação de bytes, formulários 100% limpos e leitor de QR Code isolado com suporte Web.
- Build: Revisão estática aprovada, balanço de chaves e sintaxe validado.
- Testes: Suíte completa de testes unitários preservada e validada.

## Etapa 8 — Refinamento de UX, Central de 6 Câmeras na Tela Principal, Perfil de Conta e Varredura Wi-Fi Real
- Feito:
  1. **Adicionar Câmera (`_showCameraDialog`)**: Campo de IP configurado com formato genérico `000.000.0.00` visível (`floatingLabelBehavior: FloatingLabelBehavior.always`); removido o nome "Yoosee" do bloco de autenticação RTSP e do campo de caminho/perfil, deixando todos os espaços 100% livres para preenchimento.
  2. **Identificador de Login / Avatar no Topo (`_buildProfileAvatarButton` + `_showAccountProfileDialog`)**: Removido o `CircleAvatar` que dependia de URL externa bloqueada por CORS no Chrome (que aparecia como círculo branco). Implementado avatar interativo com suporte a **Ícone Genérico**, **Masculino**, **Feminino** ou **Foto URL**, além de nome/login e e-mail persistidos em `SharedPreferences`.
  3. **Inserir / Colar QR Code (`_showManualQrInputDialog`)**: Removidos os exemplos do `hintText`, mantendo o campo limpo e pronto para colagem.
  4. **Buscar Câmeras no Wi-Fi (`_discoverCamerasOnLan` + `YooseeOnvifHelper.discoverLocalCameras`)**: Removido o IP fictício `192.168.1.50`, adicionado campo vazio para digitar IP/faixa de sub-rede e implementada sondagem ativa de portas RTSP (`554`/`8554`) e ONVIF (`5000`/`8899`) + teste direto do IP informado.
  5. **Botão "Voltar para Tela Inicial" (`_buildBackToHomeBanner`)**: Adicionado no topo da tela de **Configurações Gerais**, **Gravações** e **Leitor de QR Code**.
  6. **Painel Principal de até 6 Câmeras (`_buildSixCamerasMainPanel`)**: Integrado diretamente na tela inicial com 6 slots (grade 2x3), permitindo visualizar até 6 vídeos ao vivo simultaneamente, destacar qualquer câmera no Card Hero ou adicionar novas câmeras nos slots livres (`Câmera 1` a `Câmera 6`).
- Build: Revisão estática e verificação de unicidade de métodos aprovadas em todas as 24 classes.

## Etapa 9 — Correção de QR Code de Convite + Joystick PTZ Semi-Transparente (PS5/Xbox) + Barra Completa de Recursos
- Feito:
  1. **Leitor de QR Code e Links de Compartilhamento (`share.yoosee.com` / `InviteCode`)**:
     - `YooseeOnvifHelper.isCloudInviteQrUrl` e `extractInviteOrDeviceId` reconhecem automaticamente códigos QR de convite/compartilhamento (`https://share.yoosee.com/share/index.html?Type=2&InviteCode=...`) e IDs de dispositivo.
     - O botão **"Preencher via Leitor de QR Code"** dentro de `Adicionar Câmera` agora abre o leitor em um modal direto sobre o diálogo (`_openInlineQrScannerModal`) sem fechar a janela nem perder os dados digitados.
     - Se o campo `Nome da Câmera` estiver vazio ao clicar em **Adicionar**, o app atribui automaticamente `'Câmera N'` para nunca travar a inclusão.
     - Na tela da câmera vinculada via QR Code, há o botão direto **"Definir IP Local (000.000.0.00)"** para conectar o stream RTSP local em 1 clique.
  2. **Joystick PTZ Semi-Transparente Estilo PS5 DualSense / Xbox (`GamepadPtzJoystick`)**:
     - Acionado com **um clique em qualquer ponto da tela específica da câmera** (ou no botão `PTZ`).
     - Abre **ao lado** da tela da câmera (com botão `⇠ PTZ / PTZ ⇢` para alternar entre o lado direito e esquerdo).
     - Construído com cápsula semi-transparente (`Colors.black.withOpacity(0.38)`), aro neon azul/ciano, gatilhos superiores **`L2 Zoom -`** e **`R2 Zoom +`**, botões direcionais D-Pad (`▲`, `▼`, `◀`, `▶`) e **manche analógico 3D arrastável em 360°** que acompanha o dedo/mouse e retorna ao centro ao soltar.
  3. **Restauração de Todos os Recursos na Tela da Câmera**:
     - Barra completa com ícones e rótulos: **`PTZ`**, **`HD / SD`**, **`Gravar (REC)`**, **`Clipe`**, **`Foto`**, **`Áudio / Mudo`**, **`Play / Pausar`**, **`Noturno`**, **`Guarda`**, **`Reconectar`**, **`Funções`** e **`Tela Cheia`**.
- Build: Revisão estática 100% aprovada.

## Etapa 10 — Expansão para 10 Câmeras + Redução de Tamanho (1/4) + Filtro "Mostrar Câmeras Ativas"
- Feito:
  1. **Capacidade Expandida para 10 Câmeras (`maxSlots = 10`)**: O Painel Multicâmeras na tela principal agora gerencia até 10 câmeras simultâneas com contador dinâmico `Painel Multicâmeras (X/10 Ativas)`.
  2. **Tamanho Compacto Ultra-Otimizado (~1/4 do tamanho anterior)**:
     - Grade alterada de 2 colunas para **3 colunas** (`crossAxisCount: 3`) com formato panorâmico compacto (`childAspectRatio: 1.35`).
     - Ícones reduzidos para tamanho `20` (anteriormente `28`), paddings compactados para `6px/12px`, fontes redimensionadas para `11px/10px/8.5px`.
     - Área vertical ocupada na tela inicial drasticamente reduzida para manter a tela limpa.
  3. **Função "Mostrar Câmeras Ativas" (`_showOnlyActiveCameras`)**:
     - Adicionado botão interativo `FilterChip` no cabeçalho do painel: **`Ativas (N)`** vs **`10 Slots`**.
     - Quando ativo (`_showOnlyActiveCameras = true`), exibe apenas as câmeras cadastradas pelo usuário (ocultando todos os slots vazios que ocupariam espaço desnecessário). Caso nenhuma câmera esteja cadastrada, exibe apenas 1 card compacto para inclusão rápida.
     - Quando desativado (`_showOnlyActiveCameras = false`), expande os 10 slots completos (`Câmera 1` a `Câmera 10`).
- Build: Análise estática aprovada, sem erros de compilação ou tipos.
- Testes: Suíte completa de testes unitários em `test/widget_test.dart` verificada.

## Etapa 11 — Ajustes de UX: Bento Box 2/4 em 2 Colunas + Card Hero Centralizado + Botões de Ação na Câmera + Slots Dinâmicos (+1)
- Feito:
  1. **Bento Box Reduzido em 50% em 2 Colunas Paralelas**:
     - `_buildStatusCard` reestruturado em layout horizontal (`Row`) compacto (`childAspectRatio: 2.5`).
     - Mantidas **2 colunas paralelas** (`crossAxisCount: 2`) ocupando a metade da altura vertical anterior (2/4 do tamanho).
     - Subtítulo do card de Câmeras atualizado para `${_cameras.length} / 10 ativas`.
  2. **Card Hero Centralizado sob "Monitoramento"**:
     - Container da câmera em destaque configurado com `width: double.infinity` e alinhamento `CrossAxisAlignment.center` no estado sem câmera conectada.
  3. **Inclusão Dinâmica de Slots de Câmeras (+1 Slot Livre)**:
     - O Painel Multicâmeras calcula os slots como `(_cameras.length + 1).clamp(1, 10)`. O grid exibe estritamente as câmeras já adicionadas mais **1 slot livre** para inclusão da próxima câmera, evitando poluição visual com 10 cards vazios.
  4. **Adição de Todos os Botões de Controle nas Câmeras**:
     - **Volume / Áudio** (`Icons.volume_up` / `Icons.volume_off`).
     - **Sinal de Alerta / Sirene 🚨** (`Icons.campaign` com feedback tátil/notificação).
     - **PTZ Estilo Joystick** (`Icons.sports_esports` com joystick semi-transparente PS5/Xbox).
     - **Botão REC / Parar REC** (`Icons.fiber_manual_record` vermelho).
     - **Compartilhar Dispositivo** (`Icons.share` com abertura da tela de convite/permissões de visitantes).
     - **Configurações Adicionais** (`Icons.settings` / `Icons.tune` para ajustes completos da câmera).
- Build: Revisão estática 100% aprovada.

## Etapa 12 — Sincronização Proporcional da Altura dos Cards Bento Box + Card Hero Vazio Compacto
- Feito:
  1. **Cards Bento Box Proporcionais ao Painel Multicâmeras (`childAspectRatio: 4.2`)**:
     - Reduzida a altura vertical dos cartões de status (`Câmeras`, `Eventos`, `Status`, `Gravações`) pela metade em relação à etapa anterior, ajustando `childAspectRatio` para `4.2` e padronizando o tamanho em relação às pílulas/slots do Painel Multicâmeras acima.
     - Ícones e textos internos reorganizados em cápsulas horizontais ultra-compactas.
  2. **Card Hero "Nenhuma Câmera Conectada" Otimizado e Centralizado**:
     - Mantido alinhamento 100% centralizado (`CrossAxisAlignment.center` e `WrapAlignment.center`).
     - Ícone reduzido de `56px` para `32px`, títulos redimensionados (`15px` e `11px`), paddings compactados (`12px`/`16px`) e botões com `VisualDensity.compact`, reduzindo a altura do card vazio em mais de 50%.
- Build: Revisão estática aprovada com 0 erros.

## Etapa 13 — Formato Quadrado Centralizado com Bordas Suavizadas para o Card Vazio de Câmera
- Feito:
  1. **Container Quadrado/Compacto Centralizado (`BoxConstraints(maxWidth: 360)`)**:
     - O estado vazio "Nenhuma Câmera Conectada" deixa de se estender como uma faixa horizontal de borda a borda e passa a ser exibido em um **card compacto e quadrado centralizado** no meio da tela (`Center` -> `BoxConstraints(maxWidth: 360)`).
  2. **Bordas Suavizadas Arredondadas (`BorderRadius.circular(22)`)**:
     - Canto com curvatura suave e sombra elegante em sintonia com a linguagem visual Bento Box.
  3. **Escrita e Botões Ajustados ao Novo Tamanho**:
     - Círculo com ícone de vídeo (`32px`), título (`15px`), subtítulo conciso (`11px`) e botões de ação (`Adicionar Câmera` e `Buscar no Wi-Fi`) com cantos arredondados (`12px`) perfeitamente ajustados ao formato compacto.
- Build: Análise estática 100% aprovada.

## Etapa 14 — Refinamento Visual do Painel: Sombreado Suave + Botões sob o Título + Slots Quadrados 1:1
- Feito:
  1. **Redução do Sombreado Traseiro**: Sombra do container reduzida para desfoque sutil (`blurRadius: 4`, opacidade `0.02`) com borda fina (`primary.withValues(alpha: 0.08)`), eliminando o aspecto pesado de fundo.
  2. **Relocação dos Botões "Dinâmico" e "Ao Vivo"**: Os chips de controle foram movidos para a linha imediatamente **abaixo do título `Painel Multicâmeras (X/10 Ativas)`**.
  3. **Slots Quadrados 1:1 com Bordas Arredondadas**: Os mostradores das câmeras no painel agora usam proporção perfeitamente quadrada (`childAspectRatio: 1.0`) e cantos suavizados com raio `16px` (`BorderRadius.circular(16)`).
- Build: Análise estática 100% aprovada.

## Etapa 15 — Barra Minimalista de 4 Ícones de Ação Rápida (Câmeras, Status, Eventos, Gravações)
- Feito:
  1. **Substituição dos Cards Retangulares por Barra Minimalista**:
     - Os 4 cartões retangulares grandes foram substituídos por um container limpo de atalhos circulares (`_buildQuickActionButton`).
  2. **4 Ícones Circulares com Rótulos de Texto Básico**:
     - **Câmeras** (`Icons.videocam_outlined` - azul) -> menu rápido de câmeras.
     - **Status** (`Icons.dns_outlined` - verde) -> diálogo rápido de rede e transporte RTSP.
     - **Eventos** (`Icons.notifications_active_outlined` - laranja) -> histórico de alertas.
     - **Gravações** (`Icons.video_library_outlined` - roxo) -> navegação para a galeria local.
- Build: Análise estática 100% aprovada.

## Etapa 16 — Remoção dos Itens Redundantes ("Câmeras" e "Gravações") da Barra de Navegação Inferior
- Feito:
  1. **Remoção dos Botões da BottomNavigationBar**:
     - Os botões redundantes **`Câmeras`** e **`Gravações`** foram removidos da barra de navegação inferior (`BottomNavigationBar`).
  2. **Barra Inferior Otimizada com 2 Ações**:
     - A barra inferior conta agora com **`Scanner`** (`Icons.qr_code_scanner`) e **`Ajustes`** (`Icons.settings`), oferecendo alternância direta para o leitor de QR Code e para as configurações gerais.
  3. **Mapeamento e Navegação Limpa**:
     - Preservada toda a navegação e retorno à tela inicial em 1 clique em qualquer sub-tela através do banner `_buildBackToHomeBanner`.
- Build: Análise estática 100% aprovada com 0 erros.

## Etapa 17 — Atalho Próprio de Compartilhamento (`Icons.share`) + Controles Integrados nos Slots
- Feito:
  1. **Atalho Dedicado com Ícone Próprio para Compartilhamento**:
     - Adicionado o ícone azul claro de compartilhamento (`Icons.share`) no topo de cada slot de câmera e na barra superior/inferior dos players de vídeo. Tocar no atalho abre diretamente a tela `ShareDeviceScreen` de compartilhamento e gestão de permissões de visitantes.
  2. **Controles de Câmeras nos Cards dos Slots**:
     - Cada cartão quadrado no Painel Multicâmeras exibe atalhos diretos para acionar o **Joystick PTZ** (`Icons.sports_esports`), **Ajustes da Câmera** (`Icons.tune`), **Tela Cheia** (`Icons.fullscreen`) e **Destacar no Hero** (`Hero ★`).
- Build: Análise estática 100% aprovada com 0 erros.

## Etapa 18 — Redimensionamento Auto-Escalável do Joystick PTZ (`FittedBox`) + Tratamento do Erro de QR Code
- Feito:
  1. **Ajuste de Tamanho e Eliminação do Transbordamento no Joystick PTZ**:
     - Dimensões do `GamepadPtzJoystick` reduzidas para proporções ideais (`width: 112/138`, `baseSize: 88/108`).
     - Envolvido em `FittedBox(fit: BoxFit.scaleDown)` em todas as telas, eliminando completamente a mensagem `BOTTOM OVERFLOWED BY 37 PIXELS` independente da orientação ou tamanho do player.
  2. **Explicação e Tratamento da Mensagem no QR Code**:
     - **Causa Explicada**: Links de QR Code do Yoosee (`share.yoosee.com`) contêm tokens de convite em nuvem, não URLs RTSP com IP local fixo. Quando o motor de vídeo tentava abrir a URL vazia, o player HTML5 emitia a mensagem bruta `MEDIA_ELEMENT_ERROR: Empty src attribute`.
     - **Solução Implementada**: Filtro no ouvinte de erro do `media_kit` para interceptar e suprimir a mensagem técnica, exibindo um painel orientativo claro com o botão **`Definir IP Local (000.000.0.00)`** para que o usuário informe o IP da câmera e conecte o stream instantaneamente.
- Build: Análise estática 100% aprovada.




