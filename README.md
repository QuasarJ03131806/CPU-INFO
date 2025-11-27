
# CPU Info App

## Objetivo da Aplicação
O **CPU Info App** é uma ferramenta nativa para macOS desenvolvida para fornecer aos usuários uma visão detalhada e em tempo real sobre o hardware e o desempenho do seu sistema. O objetivo principal é monitorar o uso da CPU, exibir informações detalhadas sobre o processador (incluindo caches L1, L2, L3), memória RAM (uso e detalhes dos pentes) e saúde da bateria.

A aplicação foi projetada para ser leve, eficiente e não depender de bibliotecas externas pesadas, utilizando ferramentas nativas do sistema para coleta de dados.

## Funcionalidades Principais
*   **Monitoramento de CPU**: Gráficos em tempo real de uso (Total, Usuário, Sistema, Ocioso).
*   **Detalhes do Hardware**: Exibe Modelo do Mac, Arquitetura, Geração, Codinome, Núcleos Físicos/Lógicos e tamanhos de Cache (L1, L2, L3).
*   **Monitoramento de Memória**: Uso de RAM, pressão de memória e detalhes físicos dos pentes de memória (Slots, Tipo, Velocidade).
*   **Saúde da Bateria**: Capacidade atual, ciclos de carga, status de carregamento e saúde geral.
*   **Sobre**: Informações do desenvolvedor e versão.

## Tecnologias Utilizadas
A aplicação foi construída utilizando tecnologias nativas da Apple para garantir performance e compatibilidade:

*   **Linguagem**: Swift 5
*   **Interface Gráfica**: SwiftUI
*   **Sistema de Build**: Shell Script (`swiftc` manual) para contornar limitações de pacotes e garantir a inclusão correta de recursos.

## Dependências e Ferramentas do Sistema
O aplicativo não possui dependências de terceiros (como CocoaPods ou Swift Package Manager para bibliotecas externas). Ele depende exclusivamente de frameworks nativos e ferramentas de linha de comando do macOS para extrair informações:

### Frameworks Swift
*   `Foundation`: Funcionalidades básicas e manipulação de dados.
*   `SwiftUI`: Construção da interface do usuário.
*   `Darwin`: Acesso a APIs de baixo nível do kernel (C-interop).

### Ferramentas de Sistema (System Calls & CLIs)
A aplicação executa comandos internos para buscar dados que não estão facilmente disponíveis via APIs de alto nível:

1.  **`sysctl`**:
    *   Utilizado para obter detalhes da CPU (marca, frequência, núcleos).
    *   Utilizado para ler tamanhos de Cache (L1, L2, L3) via chaves `hw.l1icachesize`, `hw.l2cachesize`, etc.
    *   Utilizado para obter o tamanho total da memória física.

2.  **`system_profiler`**:
    *   `SPHardwareDataType`: Para obter o "Model Identifier" e mapear para o nome comercial do Mac (ex: "MacBook Pro (15-inch, Late 2011)").
    *   `SPMemoryDataType`: Para detalhar os pentes de memória instalados (Bancos, Tamanho, Tipo, Velocidade).

3.  **`ioreg`**:
    *   Utilizado para acessar o `AppleSmartBattery` e obter dados precisos da bateria (Ciclos, Capacidade de Design, Capacidade Atual).

4.  **`host_statistics64` (Mach Kernel API)**:
    *   Utilizado para calcular o uso detalhado da memória (Active, Wired, Compressed) e a pressão de memória.

## Como Compilar e Gerar o Instalador
O projeto inclui um script de automação para compilar e empacotar o aplicativo em um arquivo `.dmg`.

1.  Abra o terminal na raiz do projeto.
2.  Execute o script de empacotamento:
    ```bash
    ./package_app.sh
    ```
3.  O instalador será gerado como `CPU_Info_Installer.dmg`.

---
**Desenvolvido por**: uppertec
**Contato**: netsouto@gmail.com
