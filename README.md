<div align="center">
  <img src="assets/gtune_logo.png" alt="gTune Logo" width="136" height="136" />
  <h1>gTune</h1>
  <p><strong>Lightweight, Anti-Cheat Safe Desktop & In-Game Media Overlay for Windows 10/11</strong></p>
  <p><em>Conceived, Designed, and Created by <strong>Guilherme Orlandi</strong></em></p>
  <p><em>Pensado, Projetado e Criado por <strong>Guilherme Orlandi</strong></em></p>
  <p>
    <a href="https://github.com/GuilheermeDev/gTune-Release/releases/latest/download/gTune.exe">
      <img src="https://img.shields.io/badge/Download-gTune.exe%20(Windows%2010%2F11%20x64)-1DB954?style=for-the-badge&logo=windows&logoColor=white" alt="Download gTune.exe" />
    </a>
    <a href="https://github.com/GuilheermeDev/gTune-Release/releases">
      <img src="https://img.shields.io/github/v/release/GuilheermeDev/gTune-Release?style=for-the-badge&color=0D1017&label=Official%20Releases" alt="GitHub Releases" />
    </a>
  </p>
</div>

---

## 🇺🇸 English — Product Guide & Patch Notes

### About gTune
**gTune** is a sleek, ultra-lightweight media and volume overlay designed for Windows 10 and Windows 11. Conceived, designed, and created by **Guilherme Orlandi**, it integrates natively with Windows Media Controls (GSMTC) and Core Audio to display real-time song progress, album art, and interactive media/volume controls for **Spotify**, **YouTube** (Chrome, Edge, Brave, Firefox — even when minimized), **Apple Music**, and **VLC**.

### How to Download & Use (Releases)
1. Click the [**Download gTune.exe**](https://github.com/GuilheermeDev/gTune-Release/releases/latest/download/gTune.exe) button at the top of this page (or visit the [**Releases**](https://github.com/GuilheermeDev/gTune-Release/releases) tab).
2. Download the standalone **`gTune.exe`** executable.
3. Double-click **`gTune.exe`** to launch immediately — **no installation, no Python, and no extra dependencies required**.

---

### 🛡️ Why gTune is 100% Anti-Cheat Safe (Riot Vanguard, Valve VAC, EAC & BattlEye)

Players of competitive titles like **Valorant / League of Legends (Riot Vanguard)**, **Counter-Strike 2 (Valve VAC)**, **Apex Legends / Fortnite (EasyAntiCheat)**, and **Rainbow Six Siege (BattlEye)** can use **gTune** with complete peace of mind. By architectural design, **gTune** never interacts with any game process:

1. **Zero Process Access (`No OpenProcess / No Memory Reading`)**:
   - Anti-cheats flag programs that open handles to game processes (`OpenProcess`), enumerate process memory (`ReadProcessMemory` / `WriteProcessMemory`), or scan system process tables. **gTune** does not contain any process-scanning libraries and never opens a handle to any external process.
2. **Zero Graphics / DLL Injection (`No DirectX, OpenGL, or Vulkan Hooks`)**:
   - Unlike invasive overlays that inject a `.dll` into the game's graphics swapchain (`IDXGISwapChain::Present`), **gTune** is a standard Windows desktop window (`WS_EX_TOPMOST | WS_EX_NOACTIVATE`) managed strictly by the Windows Desktop Window Manager (DWM) — the exact same mechanism used by **Windows Task Manager ("Always on Top")** and browser **Picture-in-Picture** video players.
3. **Zero Low-Level Input Hooks (`No WH_KEYBOARD_LL / No WH_MOUSE_LL`)**:
   - **gTune** never intercepts or hooks your keyboard or mouse stream (`SetWindowsHookEx`). It uses only the official Windows `RegisterHotKey` (`WM_HOTKEY`) notification API.
4. **Zero Input Simulation or Macros (`No SendInput / No keybd_event / No mouse_event`)**:
   - Pressing a shortcut or button in **gTune** only communicates with the **Windows Media Service (Spotify / Browser)** and the **Windows Master Volume Mixer**. It never sends or simulates keystrokes or mouse clicks into any game.

---

### 🚀 Patch Notes — `v1.2.0` (Per-Player Audio Control, Desktop OSD Toggle & 20.3 MB Build)

- **Per-Player Volume Control (100% Anti-Cheat Safe via Pure WASAPI)**:
  - Adjusting volume via the speaker button (click, drag, or mouse scroll) or keyboard shortcuts (`Ctrl+Alt+Up/Down/M`) now controls **only the active music player's volume** (Spotify, YouTube in Chrome/Edge/Brave/Firefox/Opera, Apple Music, VLC, etc.) — leaving your **game audio and Discord untouched**.
  - Includes a toggle in **Settings** and the right-click menu (**Control Active Player Volume Only**) with automatic fallback to Windows Master Volume whenever no media session is active.
  - Implemented using pure Windows Core Audio **WASAPI** (`IAudioSessionManager2` → `GetSessionIdentifier()` → `ISimpleAudioVolume`), talking exclusively to Windows `Audiosrv` with **zero `OpenProcess`, zero `GetProcessId`, and zero `psutil`**.
- **Show Top-Left OSD Outside Game Mode**:
  - New toggle in **Settings** and the right-click menu to display the Top-Left OSD card on the desktop whenever tracks or volume change, even while the main player is visible.
- **UX & Stability Improvements**:
  - Right-click context menu now closes immediately when clicking anywhere outside the menu.
  - Fixed standalone volume scroll/drag by restoring Python's `decimal` module required by `comtypes`, and prevented micro-jumps while dragging the volume button.
- **34% Smaller Executable (`31.0 MB` → `20.3 MB`, `-55%` vs `v1.0.0`) & Lower Background CPU/RAM**:
  - Migrated from the monolithic `winsdk` binary (`37.6 MB` uncompressed) to modular `winrt-*` packages (`~316 KB`), cutting over **10 MB** from `gTune.exe`.
  - Stripped unused Qt plugins and stdlib binaries (`qoffscreen.dll`, `qminimal.dll`, `qtuiotouchplugin.dll`, `qsvgicon.dll`, `_lzma.pyd`, `_bz2.pyd`, `_wmi.pyd`).
  - Added adaptive media polling (`0.40s` playing / `0.90s` paused or idle), album art memory cleanup when idle, and 60% fewer passive `Audiosrv` COM queries.

### 📦 Previous Release — `v1.1.0` (Ultra-Low RAM & 31% Smaller Executable Update)

- **31% Smaller Executable & 39 MB of Unused Libraries Stripped**:
  - Reduced standalone `gTune.exe` download size from `44.9 MB` down to **`30.8 MB`** by stripping 114 unused Qt/Python binaries (`opengl32sw.dll`, `libcrypto-3.dll`, `libssl-3.dll`, `Qt6Network.dll`, `Qt6Svg.dll`, and 78 unused `.qm` localization packs).
- **Aggressive RAM & Texture Memory Optimization (~44 MB Private RAM)**:
  - **Dialog Garbage Collection**: Closing the **How to Use**, **Settings**, or **Credits** dialogs now immediately destroys and frees 100% of their widget trees from RAM (`deleteLater()` + `gc.collect()`), consuming `0 bytes` during gameplay.
  - **Lazy Album Art Decoding**: Album art is now decoded only for the currently active view (`Taskbar` or `Floating Popup`), freeing the inactive view's pixmap and releasing the Top-Left OSD's album art buffer as soon as its fade-out animation finishes.
  - **Volume Icon Bucket Cache**: Quantized volume icon cache keys prior to `@lru_cache` lookup, reducing cached volume textures across `0%–100%` from `64` duplicate icons down to **`3` shared icons**.
- **Zero-Idle CPU & 75% Fewer Audio COM Calls**:
  - Automatically pauses the `200ms` timeline timer whenever media is paused or the bottom player is hidden (System Tray or Game Mode 1 Stealth).
  - Kept `WM_HOTKEY` polling at `30ms` for instant shortcut response while throttling passive Core Audio COM status checks to `120ms` (`-75%` COM allocations) and explicitly closing WinRT media stream handles immediately after reading thumbnails.

### 📦 Previous Release — `v1.0.0`

- **Monotonic Timeline Engine (YouTube Minimized & Spotify Fix)**:
  - Fixed YouTube playback timer freezing when the browser window is minimized or in a background tab using UTC `last_updated_time` extrapolation.
  - Eliminated 1–2 second backward/forward jumps during Spotify playback with a smooth `200ms` monotonic clock.
  - Automatic session prioritization: prefers any currently playing media session over paused background apps.
- **Dual Configurable Fullscreen Game Modes** *(Configurable in **Settings** or System Tray)*:
  - **Game Mode 1 — Stealth (Show OSD Only on Action)**: The bottom player stays completely invisible during fullscreen gameplay and only displays the Top-Left OSD card for ~3 seconds when a track changes or a command is used.
  - **Game Mode 2 — In-Game HUD (Show Transparent Inside Game)**: The bottom player appears inside fullscreen games in a transparent, click-through state (`WS_EX_TRANSPARENT`) so mouse clicks pass straight through into the game while all keyboard commands work. Includes an optional **"Make Overlay Clickable Inside Game"** toggle in Settings.
  - Added smart DWM Z-Order promotion (`HWND_NOTOPMOST -> HWND_TOPMOST`) exclusively on window focus transitions (zero `SetWindowPos` spam during gameplay).
- **100% Clickable Controls (Main Player & Top-Left OSD)**:
  - Transport buttons (`Previous`, `Play/Pause`, `Next`) and the speaker button are directly clickable on both the bottom player and the Top-Left OSD card without stealing focus from your active game (`WS_EX_NOACTIVATE`).
  - Hovering the mouse over the Top-Left OSD automatically pauses its auto-hide timer.
- **Click, Drag & Scroll Volume Control**:
  - **Single Click** on the speaker button toggles **Mute / Unmute**.
  - **Click & Drag** (up/down or left/right) smoothly adjusts master volume from `0%` to `100%` with a live floating percentage slider.
  - **Mouse Wheel Scroll** over the speaker button adjusts volume by `±2%` per notch.
- **Custom Vector UI & Memory Optimization**:
  - Replaced all system emojis with custom anti-aliased `QPainter` vector icons (including the tactical **Crosshair** for Game Mode, **Pin / Pop-Out**, **Minimize `—`**, and discreet dark **Close `✕`**) with `@lru_cache` and album-art buffer deduplication for minimal RAM and CPU usage.
  - Added built-in bilingual **How to Use**, **Settings**, and **Credits** dialogs (`English` & `Português pt-BR`).
  - Full Windows 11 **Smart App Control** compatibility.

### Keyboard Shortcuts

| Shortcut | Action |
| :--- | :--- |
| `Ctrl + Alt + O` | Show or Minimize the main player to the System Tray |
| `Ctrl + Alt + P` | Switch between Pinned Taskbar Mode and Floating Popup Mode |
| `Ctrl + Alt + G` | Activate or Deactivate Game Mode (applies Mode 1 or Mode 2) |
| `Ctrl + Alt + I` | Show the Top-Left OSD Card on demand |
| `Ctrl + Alt + Space` | Play / Pause current media |
| `Ctrl + Alt + Right / Left` | Next Track / Previous Track |
| `Ctrl + Alt + Up / Down` | Volume Up (`+5%`) / Volume Down (`-5%`) |
| `Ctrl + Alt + M` | Mute / Unmute Master Audio |

---

## 🇧🇷 Português (pt-BR) — Guia do Produto e Patch Notes

### Sobre o gTune
O **gTune** é um overlay leve, discreto e preciso de mídia e volume para Windows 10 e Windows 11. Pensado, projetado e criado por **Guilherme Orlandi**, ele se conecta nativamente ao sistema de mídia (GSMTC) e áudio do Windows para exibir o tempo real da música, capa do álbum e controles interativos para **Spotify**, **YouTube** (Chrome, Edge, Brave, Firefox — mesmo com o navegador minimizado), **Apple Music** e **VLC**.

### Como Baixar e Usar (Releases)
1. Clique no botão [**Download gTune.exe**](https://github.com/GuilheermeDev/gTune-Release/releases/latest/download/gTune.exe) no topo desta página (ou acesse a aba [**Releases**](https://github.com/GuilheermeDev/gTune-Release/releases)).
2. Baixe o executável portátil **`gTune.exe`**.
3. Dê dois cliques no **`gTune.exe`** para abrir — **não precisa instalar Python nem nenhuma dependência na máquina**.

---

### 🛡️ Por que o gTune é 100% Seguro contra Anti-Cheats (Riot Vanguard, Valve VAC, EAC e BattlEye)

Jogadores de títulos competitivos como **Valorant / League of Legends (Riot Vanguard)**, **Counter-Strike 2 (Valve VAC)**, **Apex Legends / Fortnite (EasyAntiCheat)** e **Rainbow Six Siege (BattlEye)** podem utilizar o **gTune** com total segurança. Por arquitetura, o **gTune** jamais interage com o processo de nenhum jogo:

1. **Zero Acesso a Processos (`Sem OpenProcess / Sem Leitura de Memória`)**:
   - Anti-cheats banem programas que abrem *handles* para o executável do jogo (`OpenProcess`) ou leem/escrevem na memória de outros processos (`ReadProcessMemory`). O **gTune** não possui bibliotecas de varredura de processos e nunca acessa a memória ou o processo de nenhum jogo.
2. **Zero Injeção de DLL ou Hook Gráfico (`Sem Hook em DirectX, OpenGL ou Vulkan`)**:
   - Diferente de overlays invasivos que injetam DLLs dentro do motor gráfico do jogo, o **gTune** é apenas uma janela nativa da área de trabalho do Windows (`WS_EX_TOPMOST | WS_EX_NOACTIVATE`) gerenciada pelo próprio DWM do Windows — exatamente o mesmo mecanismo usado pelo **Gerenciador de Tarefas ("Sempre visível")** e pelo modo **Picture-in-Picture** dos navegadores.
3. **Zero Hooks de Teclado ou Mouse (`Sem WH_KEYBOARD_LL / Sem WH_MOUSE_LL`)**:
   - O **gTune** não intercepta nem monitora a fila global de digitação ou cliques do mouse (`SetWindowsHookEx`). Ele utiliza apenas a API oficial `RegisterHotKey` (`WM_HOTKEY`) do próprio Windows.
4. **Zero Simulação de Teclas ou Macros (`Sem SendInput / Sem keybd_event / Sem mouse_event`)**:
   - Apertar qualquer atalho ou botão do **gTune** conversa **exclusivamente com o serviço de mídia do Windows (Spotify / Navegador)** e com o **Mixer de Volume do Windows**. Nenhum comando, tecla ou clique é enviado ou simulado para dentro do jogo.

---

### 🚀 Patch Notes — `v1.2.0` (Volume Por Player, OSD Fora do Modo Jogo e Executável de 20.3 MB)

- **Controle de Volume Específico do Player Ativo (100% Seguro contra Anti-Cheats via WASAPI Puro)**:
  - Ajustar o volume na caixinha de som (clique, arraste ou scroll) ou pelos atalhos (`Ctrl+Alt+Cima/Baixo/M`) agora altera **apenas o volume do player de música ativo** (Spotify, YouTube no Chrome/Edge/Brave/Firefox/Opera, Apple Music, VLC, etc.) — deixando o **som do seu jogo e do Discord intactos**.
  - Pode ser ligado ou desligado em **Configurações** ou no menu de botão direito (**Controlar Volume Apenas do Player de Música**), com fallback automático para o volume geral do Windows quando nenhuma mídia estiver ativa.
  - Implementado com **WASAPI COM puro** (`IAudioSessionManager2` → `GetSessionIdentifier()` → `ISimpleAudioVolume`), conversando apenas com o serviço `Audiosrv` do Windows — **zero `OpenProcess`, zero `GetProcessId` e zero `psutil`**.
- **Nova Opção — Mostrar Overlay OSD Fora do Modo Jogo**:
  - Opção ligável/desligável nas **Configurações** e no menu de botão direito para exibir o card rápido do canto superior esquerdo no desktop ao trocar de música ou ajustar o som, mesmo com o player principal visível.
- **Melhorias de UX e Correções**:
  - O menu de botão direito agora fecha imediatamente ao clicar fora em qualquer lugar da tela.
  - Corrigido o scroll/arraste de volume no executável portátil (restaurado o módulo `decimal` exigido pelo `comtypes`) e eliminados micro-saltos durante o arraste.
- **Executável 34% Menor (`31.0 MB` → `20.3 MB`, `-55%` vs `v1.0.0`) e Menos CPU/RAM em Jogos**:
  - Migração do `winsdk` monolítico (`37.6 MB` descomprimido) para os pacotes modulares `winrt-*` (`~316 KB`), cortando mais de **10 MB** do `gTune.exe`.
  - Remoção de plugins Qt e binários não utilizados (`qoffscreen.dll`, `qminimal.dll`, `qtuiotouchplugin.dll`, `qsvgicon.dll`, `_lzma.pyd`, `_bz2.pyd`, `_wmi.pyd`).
  - Poll adaptativo de mídia (`0.40s` tocando / `0.90s` pausado ou ocioso), liberação do cache de capa quando sem mídia e 60% menos chamadas COM passivas ao `Audiosrv`.

### 📦 Versão Anterior — `v1.1.0` (Atualização Ultra-Leve: Menos RAM e Executável 31% Menor)

- **Executável 31% Menor & Remoção de 39 MB de DLLs Não Utilizadas**:
  - O tamanho do `gTune.exe` caiu de `44.9 MB` para **`30.8 MB`** com a remoção de 114 arquivos binários que o PyQt6 incluía por padrão mas que o gTune não utilizava (`opengl32sw.dll`, `libcrypto-3.dll`, `libssl-3.dll`, `Qt6Network.dll`, `Qt6Svg.dll` e 78 pacotes `.qm`).
- **Otimização Agressiva de Memória RAM (~44 MB de RAM Privada)**:
  - **Liberação Automática de Janelas Fechadas**: Ao fechar as janelas **Como Usar**, **Configurações** ou **Créditos**, todos os widgets são destruídos e liberados imediatamente da memória RAM (`deleteLater()` + `gc.collect()`), consumindo `0 bytes` enquanto você joga.
  - **Decodificação Sob Demanda da Capa do Álbum**: A capa da música agora só fica alocada no modo visível atual (*Barra de Tarefas* ou *Popup Flutuante*), liberando o pixmap da aba inativa e limpando também a miniatura do card OSD superior esquerdo assim que ele desaparece.
  - **Cache Inteligente de Ícones de Volume**: Reduzido o cache de ícones de volume ao arrastar de `0%` a `100%` de `64` ícones duplicados na RAM para apenas **`3` ícones compartilhados**.
- **0% de CPU Quando Pausado/Oculto & -75% de Chamadas COM de Áudio**:
  - O timer de `200ms` da barra de progresso agora pausa automaticamente quando a música está pausada ou quando o player inferior está oculto (na Bandeja ou no Modo Jogo 1 Invisível).
  - Os atalhos `WM_HOTKEY` continuam respondendo instantaneamente a cada `30ms`, mas as consultas passivas de volume via COM foram reduzidas em `75%`, além do fechamento explícito imediato dos *streams* WinRT após carregar a capa.

### 📦 Versão Anterior — `v1.0.0`

- **Motor Monotônico de Tempo (Correção para YouTube Minimizado e Spotify)**:
  - Corrigido o contador de tempo do YouTube quando o navegador está minimizado ou em segundo plano através de extrapolação UTC via `last_updated_time`.
  - Eliminados os pulos de 1 a 2 segundos na barra de progresso do Spotify com relógio monotônico de `200ms`.
  - Priorização inteligente de sessão: prioriza automaticamente a aba/aplicativo que está tocando ativamente.
- **Dois Modos de Jogo em Fullscreen Configuráveis** *(Em **Configurações** ou no menu da bandeja)*:
  - **Modo Jogo 1 — Invisível (Só Aparece o Overlay Quando Há Ação)**: O player inferior fica 100% invisível e não entra na tela do jogo; apenas o card rápido superior esquerdo aparece por ~3 segundos quando você troca de música ou altera o volume.
  - **Modo Jogo 2 — Mostrar Dentro do Jogo (Transparente embaixo)**: O player aparece lá embaixo dentro da tela do jogo em fullscreen, ficando transparente e não-clicável (*click-through*, o clique do mouse passa direto para o jogo) enquanto todos os comandos funcionam. Inclui a opção **"Tornar Clicável Dentro do Jogo"** nas Configurações caso você queira clicar nos botões com o mouse dentro do jogo.
  - Promoção inteligente de janela sobre o DWM (`HWND_NOTOPMOST -> HWND_TOPMOST`) executada apenas na troca de foco da janela (zero chamadas repetitivas durante a partida).
- **Botões 100% Clicáveis (Player Principal e Card OSD Superior Esquerdo)**:
  - Os botões `Anterior`, `Play/Pause`, `Próxima` e `Volume` são clicáveis tanto no player inferior quanto no card OSD superior esquerdo sem minimizar nem roubar o foco do jogo (`WS_EX_NOACTIVATE`).
  - Passar o mouse sobre o card OSD superior esquerdo pausa automaticamente o timer de desaparecer.
- **Controle de Volume na Caixinha de Som (Clique, Arraste e Scroll)**:
  - **Clique Simples**: Muta ou desmuta o áudio.
  - **Clicar e Arrastar** (para cima/baixo ou esquerda/direita): Aumenta ou diminui o volume de `0%` a `100%` em tempo real com indicador flutuante.
  - **Roda do Mouse (Scroll)**: Ajusta o volume em `±2%` por giro.
- **Interface Vetorial Personalizada & Otimização de Memória RAM**:
  - Substituídos todos os emojis por ícones vetoriais desenhados via `QPainter` (incluindo a **Mira / Crosshair** do Modo Jogo, **Pin / Destacar**, **Minimizar `—`** e **Fechar `✕`** escuro discreto) com cache em memória (`@lru_cache`) e deduplicação de capa de álbum para consumo mínimo de RAM e CPU.
  - Adicionadas as páginas integradas **Como Usar (How to Use)**, **Configurações (Settings)** e **Créditos (Credits)** em Inglês e Português (`pt-BR`).
  - Compatibilidade total com o **Controle Inteligente de Aplicativos (Smart App Control)** do Windows 11.

### Comandos e Atalhos de Teclado

| Atalho | Função |
| :--- | :--- |
| `Ctrl + Alt + O` | Mostrar ou Minimizar o player principal para a Bandeja |
| `Ctrl + Alt + P` | Alternar entre Modo Fixo na Barra de Tarefas e Modo Popup Flutuante |
| `Ctrl + Alt + G` | Ativar ou Desativar o Modo Jogo (aplica o Modo 1 ou Modo 2 escolhido) |
| `Ctrl + Alt + I` | Exibir o card superior esquerdo (OSD) sob demanda |
| `Ctrl + Alt + Espaço` | Play / Pause na música ou vídeo atual |
| `Ctrl + Alt + Seta Dir / Esq` | Próxima Música / Música Anterior |
| `Ctrl + Alt + Seta Cima / Baixo` | Aumentar Volume (`+5%`) / Diminuir Volume (`-5%`) |
| `Ctrl + Alt + M` | Mutar / Desmutar o áudio do sistema |

---

## Credits & Copyright / Créditos e Direitos Autorais

<div align="center">
  <img src="assets/gtune_logo.png" alt="gTune Logo" width="64" height="64" />
  <p><strong>gTune Media Overlay</strong></p>
  <p><strong>Conceived, Designed, and Created by Guilherme Orlandi</strong><br/>
  <em>Pensado, Projetado e Criado por Guilherme Orlandi</em></p>
</div>

**English**:  
Copyright © 2026 **Guilherme Orlandi**. All rights reserved.  
This software, including its original concept, architecture, custom vector icons, user interface design, and source code, is the exclusive intellectual property of **Guilherme Orlandi**. Unauthorized reproduction, modification, redistribution, or commercial exploitation of this software or any of its components without prior written permission from the author is strictly prohibited.

**Português (pt-BR)**:  
Copyright © 2026 **Guilherme Orlandi**. Todos os direitos reservados.  
Este software, incluindo sua concepção original, arquitetura, ícones vetoriais personalizados, design de interface e código-fonte, é propriedade intelectual exclusiva de **Guilherme Orlandi**. A reprodução, modificação, redistribuição ou exploração comercial não autorizada deste software ou de qualquer um de seus componentes sem permissão prévia por escrito do autor é estritamente proibida.
