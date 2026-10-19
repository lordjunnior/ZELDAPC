<div align="center">

# 🗡️ LordGames

### Jogue **The Legend of Zelda: Tears of the Kingdom** nativamente no seu Mac com chip Apple Silicon

![Plataforma](https://img.shields.io/badge/plataforma-macOS%2026%2B-000000?logo=apple&logoColor=white)
![Chip](https://img.shields.io/badge/chip-Apple%20Silicon-blue)
![Gráficos](https://img.shields.io/badge/gráficos-Metal%204-orange)
![Versões](https://img.shields.io/badge/versões-1.0.0%20%7C%201.1.2%20%7C%201.4.3-green)
![Licença](https://img.shields.io/badge/licença-MIT-yellow)

**[⚡ Início rápido](#-início-rápido)** · **[📋 Requisitos](#-o-que-você-precisa)** · **[🎮 Controles](#-controles)** · **[🐞 Problemas conhecidos](#-problemas-conhecidos)** · **[🔧 Como funciona](#-como-funciona)**

</div>

---

> [!TIP]
> **Use sua IA favorita para ajudar a montar o app do Mac.** Recomendamos o Claude Code, o Codex ou qualquer outro assistente de sua preferência.

## ✨ O que é isso?

Rode a **sua própria cópia** de *Tears of the Kingdom* diretamente em um Mac com chip Apple. Você entra com o arquivo do jogo (**XCI** ou **NSP**) e com o seu `prod.keys`, e **um único script** transforma tudo em um app para Mac.

| 🧩 | Como é feito |
| --- | --- |
| **Não é emulador** | O código ARM64 do próprio jogo roda direto no chip da Apple, reescrito apenas onde o macOS exige. |
| **Serviços do Switch** | Respondidos por um pequeno host. |
| **Gráficos** | O NVN do jogo é reconstruído sobre o **Metal 4**. |
| **Conteúdo do jogo** | Nada do jogo está neste repositório. |
| **Afiliação** | O projeto não tem relação com a Nintendo. |

> [!NOTE]
> Por enquanto o projeto suporta **somente Mac**.

---

## 📰 Histórico de atualizações

| Data | Novidade |
| --- | --- |
| **18 de outubro** | ➕ Suporte à versão **1.4.3** e pré-aquecimento de shaders antes da primeira sessão de jogo (cerca de 10 minutos). |
| **17 de outubro** | ➕ Suporte às versões **1.0.0** e **1.1.2**. O pré-aquecimento no primeiro uso ainda não existia, então cenas novas podiam travar. |

> [!IMPORTANT]
> **Travadinhas ao começar a jogar ou ao entrar em uma cena nova são esperadas** enquanto os novos shaders são compilados. Não estranhe. O desempenho costuma melhorar conforme o cache vai esquentando.

---

## 🔊 Correção de áudio

A leitura de músicas e diálogos em streaming dos caminhos `content://Sound/...` foi corrigida. Os diálogos e a música de abertura foram verificados localmente na versão 1.0.0. As outras versões e cenas ainda precisam de validação.

👉 **Para aplicar a correção, refaça o build do app com os arquivos originais do jogo.** Detalhes em [solução de problemas e validação de áudio](https://github.com/lordjunnior/ZELDAPC/blob/main/docs/audio-troubleshooting.md).

---

## 📊 Testes locais

Testado em **MacBook Air M4** e **Mac mini M4**.

| Métrica | Resultado observado |
| --- | --- |
| 🧪 Shaders coletados | **8.582** |
| 🎞️ FPS após o aquecimento | cerca de **90 FPS** |
| 📋 FPS nos menus | até **120 FPS** |

> [!WARNING]
> Essas são observações locais anteriores, **não uma promessa de FPS estável** para todas as versões e cenas. As medições da versão 1.4.3 e as pausas de compilação estão registradas à parte nas [notas de teste](https://github.com/lordjunnior/ZELDAPC/blob/main/docs/v1.4.3-port.md).

### 🧠 Como o cache de shaders funciona

Os shaders são **coletados, compilados e armazenados em cache automaticamente** enquanto você joga. Voltar a cenas já visitadas costuma trazer ganhos claros de FPS e fluidez, porque o cache pode ser reaproveitado.

> **8.582 é o número coletado até agora nos testes locais.** Não é o total de shaders do jogo nem um limite de coleta. O cache continua crescendo conforme você explora.

<details>
<summary><b>🔥 Pré-aquecimento de shaders no primeiro uso (clique para expandir)</b></summary>

<br>

- O build **prepara automaticamente pelo menos 8.000 shaders** a partir dos dados do jogo que você escolheu e os inclui no app.
- Mesmo com o cache do usuário vazio, a primeira abertura descobre e aquece esses shaders **antes do primeiro quadro**. Entradas compatíveis do manifesto de pipelines também são aquecidas.
- **Não é preciso** jogar uma sessão preliminar nem copiar cache manualmente.
- O build atual da 1.4.3 prepara **8.290 shaders**.
- O primeiro aquecimento da GPU pode levar **vários minutos**. Veja os [detalhes de preparação e testes](https://github.com/lordjunnior/ZELDAPC/blob/main/docs/first-launch-shaders.md).
- Esse é o conjunto de shaders extraível do jogo, e **não** inclui todas as variantes que dependem de bindings encontradas durante a jogatina. Cenas novas ainda podem compilar shaders adicionais.
- Ao jogar, os shaders e pipelines novos são adicionados ao seu `pso_collected.tsv` local, reaproveitado nas próximas aberturas.

**Quer compartilhar sua lista coletada?** Envie este arquivo:

```
~/Library/Application Support/MacNativeGames/TOTK/shader-cache/rt-totk-native-v4-unverified/pso_collected.tsv
```

Ele contém apenas identificadores de shaders e pipelines. **Não contém saves nem binários de shaders compilados.** As listas são atreladas à versão correspondente da cadeia de tradução.

</details>

> 📱 **iPhone e iPad em breve.** Para a futura versão de iPhone, recomendamos o **iPhone 16 ou mais novo**.

---

## 📋 O que você precisa

- 🖥️ Um **Mac com Apple Silicon** e **macOS 26** ou superior (os gráficos usam Metal 4). O macOS 27 também é suportado. **16 GB de memória ou mais** é o recomendado.
- 💾 Cerca de **25 GB livres** para a pasta de trabalho, além do arquivo do jogo. Mantenha o app final no **mesmo volume APFS** para compartilhar os arquivos grandes por clonagem. Se a clonagem não estiver disponível ou o app ficar em outro volume, reserve **mais ou menos 17 GB extras** para a cópia do app.
- 🎮 O **jogo base 1.0.0** de Tears of the Kingdom (`0100F2C0115B6000`) em **XCI ou NSP**, e o seu `prod.keys`.
  - Para compilar a **1.1.2 ou a 1.4.3**, informe a atualização cumulativa separada (NSP ou NSZ) com `--update`. Não é preciso nenhuma atualização intermediária.
  - NSZ é descompactado e os hashes das NCAs são verificados antes da extração.
  - **DLC e pacotes combinados não são suportados.**
- 📦 **Para NSP:** o ticket comum precisa estar incluído, a menos que as NCAs não tenham restrição de direitos. NSPs **não contêm as fontes compartilhadas do sistema**. Informe um diretório de NCAs do firmware já desempacotado com `--firmware`, ou fontes extraídas anteriormente com `--shared-fonts`. As seis fontes devem bater com os hashes verificados deste projeto. Fontes válidas que já estejam na pasta de trabalho são reaproveitadas.

---

## ⚡ Início rápido

### 1️⃣ Ferramentas (uma única vez)

Você precisa das ferramentas de linha de comando do Xcode e do [Homebrew](https://brew.sh). É preciso internet para instalar as dependências do build e baixar o código fixado do SPIRV-Cross.

```bash
xcode-select --install
brew install opus libvpx python@3.14
```

> [!TIP]
> **Opcional:** com o toolchain Metal do Xcode (`xcodebuild -downloadComponent MetalToolchain`), o build compila os shaders de inicialização dentro do app. Sem ele, eles são compilados na primeira abertura, e de novo depois que o app for movido ou recompilado.

### 2️⃣ Baixe ou clone este repositório

Abra o Terminal na pasta `MacNativeGames` e rode o script com o arquivo do jogo e as chaves:

```bash
python3.14 build_app.py ~/Games/totk.xci -k ~/keys/prod.keys
```

**Usando NSP?** Informe também as fontes compartilhadas:

```bash
python3.14 build_app.py ~/Games/totk.nsp -k ~/keys/prod.keys --firmware ~/Games/firmware

# Ou reaproveite as seis fontes .bfttf de um build anterior:
python3.14 build_app.py ~/Games/totk.nsp -k ~/keys/prod.keys --shared-fonts /caminho/para/TOTK_build/totk/extracted/firmware/shared-fonts
```

<details>
<summary><b>🔤 Detalhes sobre <code>--firmware</code> e <code>--shared-fonts</code></b></summary>

<br>

- `--firmware` aceita um diretório com arquivos `.nca` (desempacote antes o arquivo de firmware) ou um diretório `registered` de firmware instalado.
- `--shared-fonts` aceita o diretório extraído contendo de `0-standard.bfttf` até `5-nintendo-extension.bfttf`.
- **Nenhuma fonte nem chave é baixada.**
- Em um XCI, as fontes são lidas por padrão da partição de atualização do sistema. Se essa partição estiver ausente, use também uma dessas opções.
- Os dois formatos de entrada geram um `.app` do macOS.

</details>

O app aparece ao lado do arquivo do jogo (`Tears of the Kingdom.app`). Use `-o` para escolher outro local.

<details>
<summary><b>⏱️ Quanto tempo leva o build? E o que ele faz?</b></summary>

<br>

Antes de o preparo completo de shaders ser adicionado, um build limpo local da base 1.0.0 levou **cerca de 6 minutos** em um Mac mini M4. Isso **não é uma estimativa para o build atual**, pois hardware, disco e rede influenciam o tempo.

- ~1 minuto para reescrever o módulo de código principal
- ~1 minuto para descriptografar os 17 GB de dados do jogo
- 2 a 3 minutos para o macOS verificar o app finalizado uma vez

Outras informações úteis:

- O build **não precisa de .NET nem de cmake**, e o jogo pronto **não precisa de Python**: o tradutor de shaders é um programa nativo que acompanha este repositório (`games/totk/prebuilt`).
- Todos os arquivos intermediários ficam em uma pasta de trabalho ao lado do jogo (`TOTK_build`, ou o que você definir em `--work`). **Rodar o script de novo reaproveita o que já foi feito.**
- Os módulos de código extraídos, os módulos reescritos, o RomFS e as fontes compartilhadas são conferidos com `games/totk/assets/expected-v1.0.0.json`, `expected-v1.1.2.json` ou `expected-v1.4.3.json`, conforme a versão escolhida. O tradutor de shaders pré-compilado tem sua própria checagem SHA-256. **Qualquer divergência interrompe o build.** Os valores das chaves nunca são impressos.
- Assinar e verificar um app grande atualizado pode levar alguns minutos a mais. Deixe o build rodando enquanto ele continuar mostrando progresso.

</details>

### 3️⃣ Jogue! 🎉

Abra o app pelo Finder.

- O build inicia o launcher em modo `--check`, que confirma que o executável do jogo existe e deixa o macOS fazer suas verificações da primeira abertura. **Isso não é um teste de gameplay.**
- Você pode manter o app onde foi gerado ou movê-lo para `/Applications`.
- Copiá-lo para outro volume pode usar mais espaço em disco e disparar outra verificação do macOS, e nesse intervalo pode ser que nenhuma janela apareça.
- A primeira execução do jogo verifica os dados (cerca de 20 segundos nos testes locais).
- Shaders novos podem causar travadinhas. O cache reduz esse trabalho nas visitas seguintes.

---

## 🔄 Compilar a versão 1.1.2 ou 1.4.3

```bash
python3.14 build_app.py ~/Games/totk.xci -k ~/keys/prod.keys --update ~/Games/1.1.2.nsp

# Atualizações em NSZ também são aceitas:
python3.14 build_app.py ~/Games/totk.xci -k ~/keys/prod.keys --update ~/Games/1.1.2.nsz

# Ou compile a 1.4.3 direto da mesma base:
python3.14 build_app.py ~/Games/totk.xci -k ~/keys/prod.keys --update ~/Games/1.4.3.nsz

# Com uma base em NSP, mantenha --firmware ou --shared-fonts como descrito acima.
```

Isso gera o mesmo **Tears of the Kingdom.app**, contendo **apenas a versão escolhida**.

- Feche o jogo em execução e refaça o build no mesmo caminho de `-o` para substituir o app antigo. **Não há uma segunda entrada do jogo nem menu de seleção de versão.**
- O título da janela mostra a versão selecionada, como **Ver. 1.4.3**.
- Sem `--update`, o script continua gerando a 1.0.0.

> [!NOTE]
> **Você não precisa baixar tabelas de runtime da 1.4.3 nem cache de shaders separados.** O build gera as tabelas de runtime específicas da versão e a semente de shaders do primeiro uso a partir dos dados do jogo que você forneceu, e inclui a semente no app. O repositório fornece o código do patch da 1.4.3 e os hashes verificados. Só o seu jogo base, a atualização e as chaves são entradas externas.

### 💽 Espaço em disco para builds com atualização

| Cenário | Espaço necessário |
| --- | --- |
| Build novo com atualização | cerca de **40 GB livres** |
| Dados da base já em cache | cerca de **19 GB extras** |

Em ambos os casos, o app deve ficar no **mesmo volume APFS**.

<details>
<summary><b>🔬 O que acontece durante a mesclagem da atualização</b></summary>

<br>

- A mesclagem lê a NCA base por um intervalo limitado do contêiner original.
- Extrai temporariamente a NCA da atualização, bem menor.
- Verifica todos os níveis IVFC.
- Baixa o código fixado do [hactool](https://github.com/SciresM/hactool) e o compila com clang/make.
- Os arquivos de ticket temporários são privados e removidos após a mesclagem. Os valores das chaves não são passados pela linha de comando nem impressos.

</details>

### 💾 Saves e backup

Os saves continuam na mesma pasta de perfil. **Antes da primeira abertura da versão atualizada, o app faz um backup completo ao lado**, como `profile-before-1.1.2` ou `profile-before-1.4.3`. Se o backup falhar, a inicialização é interrompida.

> [!WARNING]
> **Guarde esse backup se precisar voltar a uma versão antiga.** Não presuma que um save atualizado seja compatível com versões anteriores.

### 🖥️ Manter uma única entrada na área de trabalho

Use o instalador opcional para vincular o app empacotado:

```bash
python3.14 games/totk/tools/install_desktop_entry.py --app "/caminho/para/Tears of the Kingdom.app"
```

Ao substituir o launcher de desenvolvimento antigo, o instalador copia e verifica os saves antigos para o perfil padrão do usuário (listado em [Onde ficam as coisas](#-onde-ficam-as-coisas)) e cria um backup da atualização. Ele preserva o perfil original e arquiva o launcher antigo sem adicionar outra entrada de app. **Um perfil padrão existente e diferente nunca é sobrescrito.**

### ✅ O que já foi validado

<details>
<summary><b>Versão 1.1.2</b></summary>

<br>

O runtime de origem passou nas verificações básicas no Mac: carregamento, movimento e pulos, menus e mapa, 2K, salvar e recarregar, e tentar de novo após a morte. Mapa completo, sessões longas e comparações justas de desempenho entre 1.0 e 1.1.2 **ainda estão pendentes**. As observações de FPS anteriores não são uma promessa de desempenho da 1.1.2. A validação do empacotamento está nas [notas de build](https://github.com/lordjunnior/ZELDAPC/blob/main/docs/how-it-works.md#112-validation).

</details>

<details>
<summary><b>Versão 1.4.3</b></summary>

<br>

O port da 1.4.3 passou separadamente em um Mac M4 por:

- carregamento de cenas
- movimento e pulos
- menus e mapa
- salvar e recarregar
- troca de canvas nativo em 2K
- upscaling e interpolação de quadros com MetalFX
- app único empacotado, incluindo morte e retry, e entrada e saída de santuários

Uma build anterior foi testada por três horas e a build final por 30 minutos, registradas separadamente. **Travadas de compilação no primeiro uso, crescimento de memória em sessões longas e underruns de áudio continuam em investigação.** Veja o [registro de testes da 1.4.3](https://github.com/lordjunnior/ZELDAPC/blob/main/docs/v1.4.3-port.md). Isso não é certificação de jogo completo nem de FPS estável.

</details>

---

## 🎮 Controles

Controles reconhecidos pelo macOS (como DualSense e Xbox) são mapeados para um **Switch Pro Controller**. Vibração e sensor de movimento dependem de o controle e o macOS exporem esses recursos, e nem todo controle tem giroscópio.

O teclado usa **posições físicas das teclas** correspondentes ao layout americano:

| Tecla | Switch | | Tecla | Switch |
| --- | --- | --- | --- | --- |
| `W` `A` `S` `D` | Analógico esquerdo | | Setas | Analógico direito |
| `J` | **A** | | `K`, `Espaço` | **B** |
| `I` | **X** (pular) | | `U` | **Y** (atacar) |
| `Q` / `E` | **L** / **R** | | `Option` / `Shift` | **ZL** / **ZR** |
| `F` `T` `H` `G` | D-pad esquerda / cima / direita / baixo | | `Z` / `C` | Clique no analógico esquerdo / direito |
| `Return` | **+** (menu) | | `Tab`, `Esc` | **Menos** (mapa) |

### ⌨️ Teclas extras

| Tecla | Função |
| --- | --- |
| `1` | Mostra ou esconde **todos os overlays** (lembrete de atalhos, FPS, contagem de shaders e uso do sistema) |
| `2` | **Interpolação de quadros**: desligada → 30+30 → metade da taxa de atualização → ilimitada |
| `3` | Alterna o **modo de imagem**: MetalFX 2K → 2K nativo → desligado |

- O **MetalFX vem desligado por padrão** e só inicia quando você seleciona o modo com `3`. Redimensionar a janela nunca o ativa automaticamente.
- A digitação de texto (como dar nome a um cavalo) usa o teclado do Mac.

<details>
<summary><b>🖼️ Sobre o 2K nativo e o MetalFX</b></summary>

<br>

O **2K nativo** muda o canvas de composição e a interface do jogo para 2560×1440. A cena 3D continua seguindo a escala de renderização própria do jogo, ou seja, isso **não força todos os alvos de renderização internos a 1440p**.

O modo manual do **MetalFX** também funciona em janelas menores e escala para o tamanho real da saída, inclusive **4K**.

</details>

### 🌐 Idioma

O jogo e os textos do próprio launcher (tela de início, progresso do aquecimento de shaders, overlays, aviso de teclado) **seguem o idioma do Mac**. O texto do launcher fica em chinês em um Mac chinês e em inglês nos demais casos.

Para forçar um idioma, abra com `TOTK_LANG` (jogo) ou `TOTK_UI_LANG=zh` / `en` (texto do launcher):

```bash
open --env TOTK_LANG=en-US "Tears of the Kingdom.app"
```

---

## 📁 Onde ficam as coisas

| O quê | Caminho |
| --- | --- |
| 💾 **Saves** | `~/Library/Application Support/MacNativeGames/TOTK/profile` |
| 🧪 **Cache de shaders** | `~/Library/Application Support/MacNativeGames/TOTK/shader-cache` (apague para traduzir tudo de novo) |
| 📜 **Log** | `~/Library/Logs/MacNativeGames/totk.log` (execução anterior: `totk.previous.log`) |

---

## 🐞 Problemas conhecidos

- **Versões suportadas:** base 1.0.0, ou base mais a atualização separada 1.1.2 ou 1.4.3 (NSP/NSZ). Outras versões, DLC, pacotes combinados, jogos base compactados (NSZ/XCZ) e tickets personalizados **não são suportados**. Somente Mac.
- 🌐 **Sem recursos online** e **sem amiibo**.
- 🐌 Shaders novos podem causar travadinhas de compilação. Reaproveitar o cache reduz isso, mas **não garante uma jogatina sem travadas**.
- 🧪 **Cobertura de testes:** os 38 testes automatizados cobrem amostras geradas de contêiner e atualização, formatos de realocação, isolamento da pasta de trabalho, empacotamento, migração da área de trabalho, extração de shaders, preservação do cache na primeira importação e 27 casos de caminhos nativos de RomFS. O GitHub Actions está configurado para rodar os testes de código-fonte e os sanitizers do parser de caminhos nativos, sem dados do jogo nem chaves. Um build completo no Mac também passou com uma amostra de NSP derivada de XCI local e um ticket comum gerado. **Uma base NSP de varejo independente, e a jogatina a partir dela, ainda não foram testadas.**

---

## 🔧 Como funciona

Veja [docs/how-it-works.md](https://github.com/lordjunnior/ZELDAPC/blob/main/docs/how-it-works.md).

Experimentos de desempenho registrados, trabalho restante e métodos de comparação: [Próximos passos de desempenho no Mac](https://github.com/lordjunnior/ZELDAPC/blob/main/docs/performance-followups.md).

---

## 🙏 Agradecimentos

| Projeto | Contribuição |
| --- | --- |
| **[d2r-mobile](https://github.com/jhaizhou-ops/d2r-mobile)** (MIT) | Desenvolveu esta forma de rodar jogos de Switch nativamente (para Diablo II: Resurrected). O reescritor de NSO, o host Horizon e a camada NVN sobre Metal daqui nasceram das partes genéricas dele. |
| **[nxnative](https://github.com/seedds/nxnative)** | O empacotamento em um só script e a importação do ticket comum de NSP seguem a abordagem dele. Este projeto compila apenas apps de Mac. |
| **Ryujinx** (MIT) | Seu tradutor de shaders (`vendor/ryujinx`) converte os shaders Maxwell do jogo em SPIR-V, e seu código de serviços do sistema é uma referência. Com os patches e o serviço deste projeto, é compilado antecipadamente (.NET NativeAOT) em `games/totk/prebuilt/macos-arm64/totk_xlate`. `games/totk/tools/totk_xlate/publish_native.sh` o recompila (exige o .NET 10 SDK, apenas para desenvolvedores). |
| **[SPIRV-Cross](https://github.com/KhronosGroup/SPIRV-Cross)** (Apache-2.0) | Converte SPIR-V em Metal Shading Language. É baixado em um commit fixado e compilado com clang durante o build. |
| **deko3d** (zlib) | Tamanhos de layout de textura. |
| **bcdec** | Decodificação de texturas BC. |
| **libopus** e **libvpx** (BSD, via Homebrew) | Áudio e vídeos pré-renderizados. |

---

## 📄 Licença

MIT (veja [LICENSE](https://github.com/lordjunnior/ZELDAPC/blob/main/LICENSE)). Códigos de terceiros mantêm suas próprias licenças: [THIRD_PARTY_NOTICES.md](https://github.com/lordjunnior/ZELDAPC/blob/main/THIRD_PARTY_NOTICES.md).

<div align="center">

<sub>🌐 Traduzido do inglês e do chinês para o português do Brasil.</sub>

</div>
