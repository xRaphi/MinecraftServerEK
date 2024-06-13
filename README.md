# Inhaltsverzeichnis
[Erste Schritte](#erste-schritte)
- [Server vorbereiten](#server-vorbereiten)
- [tmux installieren](#tmux-installieren)
- [tmux-Sessions verwalten](#tmux-sessions-verwalten)
- [Minecraft-Server einrichten](#minecraft-server-einrichten)

# Erste Schritte

## Server vorbereiten
Um einen Minecraft-Server auf einem gemieteten Linux-Server einzurichten, benötigen wir einige grundlegende Schritte. Wir werden `tmux` installieren und dann den Minecraft-Server einrichten.

## tmux installieren
`tmux` ist ein Terminal-Multiplexer, der es ermöglicht, mehrere Terminal-Sitzungen zu erstellen und zwischen ihnen zu wechseln. So installieren Sie `tmux`:

1. Melden Sie sich auf Ihrem Linux-Server an.
2. Aktualisieren Sie die Paketliste:
    ```sh
    sudo apt update
    ```
3. Installieren Sie `tmux`:
    ```sh
    sudo apt install tmux
    ```
4. Überprüfen Sie die Installation:
    ```sh
    tmux -V
    ```
   Dies sollte die installierte Version von `tmux` anzeigen.

## tmux-Sessions verwalten
Mit `tmux` können Sie verschiedene Sessions erstellen und verwalten. Hier sind einige grundlegende Befehle:

- Starten einer neuen `tmux`-Session:
    ```sh
    tmux
    tmux new
    tmux new-session
    tmux new -s mysession
    ```
    Mit dem Befehl `tmux new -s mysession` können Sie eine Session mit dem Namen `mysession` erstellen.

- Starten oder Anfügen an eine existierende Session:
    ```sh
    tmux new-session -A -s mysession
    ```
    Dieser Befehl startet eine neue Session oder fügt sich an eine existierende Session mit dem Namen `mysession` an.

- Auflisten aller Sessions:
    ```sh
    tmux ls
    tmux list-sessions
    ```

- An eine existierende Session anhängen:
    ```sh
    tmux attach
    tmux attach-session
    tmux a
    tmux at
    tmux attach -t mysession
    tmux attach-session -t mysession
    ```
    Diese Befehle hängen sich an die letzte oder eine spezifizierte Session mit dem Namen `mysession` an.

- Session umbenennen:
    ```sh
    Ctrl+b, dann Shift+s
    ```

- Session verlassen, ohne sie zu beenden:
    ```sh
    Ctrl+b, dann d
    ```

- Andere Clients von der Session trennen:
    ```sh
    tmux attach-session -d
    ```

- Session beenden:
    ```sh
    tmux kill-session -t mysession
    tmux kill-ses -t mysession
    tmux kill-session -a
    tmux kill-session -a -t mysession
    ```
    Diese Befehle beenden eine oder alle Sessions außer der aktuellen.

## Minecraft-Server einrichten
Nun richten wir den Minecraft-Server ein:

1. Erstellen Sie ein Verzeichnis für den Minecraft-Server:
    ```sh
    mkdir ~/minecraft-server
    cd ~/minecraft-server
    ```
2. Laden Sie die neueste Minecraft-Server-JAR-Datei herunter:
    ```sh
    wget https://launcher.mojang.com/v1/objects/<server-version>.jar -O minecraft_server.jar
    ```
    Ersetzen Sie `<server-version>` durch die aktuelle Version. Die neueste Version finden Sie auf der [Minecraft-Website](https://www.minecraft.net/en-us/download/server).

3. Akzeptieren Sie die EULA:
    ```sh
    echo "eula=true" > eula.txt
    ```
4. Starten Sie `tmux`:
    ```sh
    tmux
    ```
5. Starten Sie den Minecraft-Server in einer `tmux`-Sitzung:
    ```sh
    java -Xmx1024M -Xms1024M -jar minecraft_server.jar nogui
    ```
    Dies startet den Minecraft-Server ohne grafische Benutzeroberfläche.

6. Um die `tmux`-Sitzung zu verlassen, ohne den Server zu stoppen, drücken Sie:
    ```sh
    Ctrl+b, dann d
    ```
7. Um später zur `tmux`-Sitzung zurückzukehren, verwenden Sie:
    ```sh
    tmux attach-session
    ```

Damit ist der Minecraft-Server auf Ihrem Linux-Server eingerichtet und läuft in einer `tmux`-Sitzung. Sie können den Server verwalten, indem Sie die `tmux`-Sitzung wieder betreten.
