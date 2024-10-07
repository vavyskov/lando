# Lando

Vývojové prostředí Lando je uživatelsky přívětivá vrstva nad virtualizačním nástrojem Docker.

## Instalace

### Docker

Možnosti:
- Aplikace Docker Desktop (Linux, Windows, macOS)
    - komerční použití zdarma pouze pro **organizace do 250 zaměstnanců** nebo **obratu do 10 mil dolarů ročně**
- **Docker Engine** - Command Line Interface (Linux, Windows, macOS):
    - zdarma

#### Linux:

Dokumentace:
- [Install using the Apt repository&nbsp;&ndash; Ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
- [Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)
- [Install the plugin manually](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)

#### Windows

Docker CLI (bez Docker Desktop):
1. Požadavky:
   - Windows 10 (build 19041+) nebo Windows 11
     ```
     winver
     ```
   - CPU s podporout SLAT (Second Level Address Translation)
   - v BIOSu pro CPU povolená virtualizace
2. Instalace WSL:
   - Spustit "Příkazový řádek" nebo "PowerShell" jako **Správce**:
     - `wsl --install` (nainstaluje WSL a jako výchozí distribuci Ubuntu)
     - Po restartu počítače počkat na kompletní dokončení instalace (může trvat i několik minut)!!!
3. Instalace Docker CLI: 
      - WSL Ubuntu:
        ```
        sudo apt update &&
        sudo apt install -y ca-certificates curl &&
        sudo install -m 0755 -d /etc/apt/keyrings &&
        sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc &&
        sudo chmod a+r /etc/apt/keyrings/docker.asc &&
        echo \
          "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
          $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
          sudo tee /etc/apt/sources.list.d/docker.list > /dev/null &&
        sudo apt update &&
        # VERSION_STRING=5:25.0.4-1~ubuntu.22.04~jammy &&
        # sudo apt install -y docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin &&
        sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin &&
        sudo apt install -y wget zip unzip mc &&
        sudo service docker start &&
        sudo usermod -aG docker $USER &&
        newgrp docker
        ```
        - Kontrola funkčnosti:
          ```
          service docker status
          (sudo service docker start)
          docker ps
          docker run hello-world
          ```
          - Zobrazení dostupných verzí Docker CE:
            ```
            apt list -a docker-ce
            ```
        - Volitelně Git:
          - Konfigurace uživatele:
            ```
            git config --global user.email "you@example.com"
            git config --global user.name "Your Name"
            git config --list
            ```

Příkazy WSL ("Příkazový řádek" nebo "PowerShell"):
- Instalace WSL a výchozí Distribuce (Ubuntu):
  ```
  wsl --install
  ```
- Instalace distribuce Ubuntu:
  ```
  wsl --install --distribution ubuntu
  (wsl --install -d ubuntu)
  ```
- Odinstalace distribuce Ubuntu:
  ```
  wsl --unregister ubuntu
  ```
- Volitelné nastavení výchozí distribuce:
  ```
  wsl --list --verbose
  (wsl --list -v)
  wsl --set-default ubuntu
  ```
- Vypnutí WSL:
  ```
  wsl --shutdown
  ```

#### macOS

Colima:
1. Instalace [Homebrew](https://brew.sh/) (MacPorts, Nix atd.). 
2. Instalace [Colima](https://github.com/abiosoft/colima):
   - ```
     brew install docker colima
     ```
3. Volitelná instalace Compose pluginu:
   - ```
     mkdir -p ~/.docker/cli-plugins &&
     ln -sfn $(brew --prefix)/opt/docker-compose/bin/docker-compose ~/.docker/cli-plugins/docker-compose
     ```
4. Spuštění:
   - ```
     colima start
     ```
4. Volitelná konfigurace (výchozí hodnoty 2 CPU, 2 GB paměť a 60 GB úložiště):
   - např. Apple silicon (M1/M2):
     - ```
       colima stop
       colima start --cpu 2 --memory 6 --arch aarch64 --vm-type=vz --vz-rosetta --mount-type=virtiofs
       ```
4. Automatické spuštění Colima při zapnutí počítače:
   - ```
     brew services start colima
     ```
5. Ukončete terminál a spusťte jej znovu.

### Lando

Dokumentace:
- [Lando installation &ndash; Linux](https://docs.lando.dev/install/linux.html)

Instalace:
- ```
  /bin/bash -c "$(curl -fsSL https://get.lando.dev/setup-lando.sh)"
  ```

Základní příkazy:
- `lando` - zobrazení nápovědy
- `lando init` - průvodce založením nového projektu (vytvoří soubor .lando.yml)
- **`lando start`** - spuštění projektu (v aktuálním adresáři)
- `lando info` - zobrazení informací o projektu (např. heslo k databázi)
- `lando ssh` - vstup do terminálu
- `lando restart` - restart projektu
- `lando rebuild` - znovunačtení konfigurace (zachová data)
- **`lando stop`** - zastavení projektu (v aktuálním adresáři)
- `lando destroy` - smazání projektu (smaže data)
- `lando poweroff` - zastavení všech kontejnerů

SSL certifikát:
1. Import důvěryhodné kořenové certifikační autority (Lando Local CA) do prohlížeče:
   - Chrome -> Nastavení -> Ochrana soukromí a zabezpečení -> Zabezpečení -> Spravovat certifikáty zařízení -> Důvěryhodné kořenové cetifikační autority -> Importovat
     - `\\wsl$\<distribution>\home\<user>\.lando\certs\lndo.site.crt`
   - Firefox -> Nastavení -> Soukromí a zabezpečení -> Zabezpečení -> Zobrazit certifikáty -> Autority -> Import
     - `\\wsl$\<distribution>\home\<user>\.lando\certs\lndo.site.pem`
   - Edge -> Nastavení -> Ochrana osobních údajů, vyhledávání a služby -> Zabezpečení -> Spravovat certifikáty -> Důvěryhodné kořenové cetifikační autority -> Importovat
     - `\\wsl$\<distribution>\home\<user>\.lando\certs\lndo.site.crt`
2. Restart prohlížeče

Tipy:
- [Accessing Your Services Externally](https://docs.lando.dev/guides/external-access.html#locking-down-ports)

## Vývoj

### Visual Studio Code

Rozšíření:
- [WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
    - ve Visual Studio Code kliknout vlevo dole na ikonu "><" a zvolit:
        - New WSL Window using Distro...
    - nebo ve WSL terminálu přejít do adresáře projektu a spustit příkaz:
      ```
      code .
      ```
- Volitelně [Git Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)
- Volitelně [IntelliJ IDEA Keybindings](https://marketplace.visualstudio.com/items?itemName=k--kato.intellij-idea-keybindings)
