# My XUBUNTU minimal customization x Thinkpad T560 ![alt](https://github.com/miko6/appunti-di-xubuntu/blob/main/immagini/xubuntu.png)  

1. Impostare la risoluzione a 1600x900 
2. Installare [Floorp](https://ppa.floorp.app/)  
3. Settare un *IP statico* e cambiare *DNS*.

* Clic destro sull'icona di rete/Modifica connessioni di rete/Selezioniamo l'interfaccia di rete e clic sull'icona ingranaggio delle impostazioni, nella sezione IPV4 impostiamo i valori di Getaway e Subnet mask.
* Impostiamo il *DNS* del server con PiHole oppure usiamo i classici:  
| DNS | Primario | Secondario |
|----|----|----|
| Cloudflare | 1.1.1.1 | 1.0.0.1 |
| Google | 8.8.8.8 | 8.8.4.4 |

4. In Impostazioni/Tastiera attivare il Block Maiusc all'avvio
5. In Impostazioni/Schermo, tab Salvaschermo, disattivare il Salvaschermo  
6. Fix tap del touchpad  
- Creare il file */etc/X11/xorg.conf.d/40-libinput.conf* e incollare al suo interno le seguenti righe:
```
Section "InputClass"
  Identifier "libinput touchpad catchall"
  MatchIsTouchpad "on"
  MatchDevicePath "/dev/input/event*"
  Driver "libinput"
  Option "Tapping" "on"  
  Option "TappingButtonMap" "lmr"
EndSection
```
* riavviare  

7. *Data e ora* stile windows10.
- Tasto destro sull'orologio, Configura, Utilizza un formato data personalizzato e scrivi nel campo Formato della data %R%n%x
8. *Samba*
* `sudo apt install samba` (se non installato)
* Controllare che l'utente in Utenti e Gruppi sia inserito nel gruppo sambashare
* `sudo smbpasswd -a nomeutente`
* digitare prima la password amministratore e poi due volte la password per la condivisione
* riavviare il server samba con `sudo service smbd restart` o riavviare il sistema  

9. `sudo apt install unrar`
10. `sudo apt install btop`
11. `sudo apt install git`
12. `sudo apt install mousepad`  
13. `sudo apt install fastfetch`  
14. `sudo apt install papirus-icon-theme`  
15. `sudo apt install seahorse` (Portachiavi password con il quale settare una password vuota. Cliccare con il tasto destro su LOGIN, scegliere Cambia password, inserire la vecchia password, poi lasciare i due campi vuoti e dare due volte Continue)  
16. `sudo add-apt-repository ppa:atareao/telegram`
- `sudo apt install telegram`  
17. *Ristretto* (visualizzatore immagini da synaptic)  
18. *xfce4-goodies* (potenziamento di xfce)
19. Se in Gestore File non è presente la sezione Network installare `sudo apt install gvfs-backends`  
20. `sudo apt install mpv`

> :memo: *script* da aggiungere nella cartella */home/.config/mpv/scripts*: **[autoload.lua](https://github.com/mpv-player/mpv/blob/master/TOOLS/lua/autoload.lua)** - Per poter scorrere tra i file di una cartella con i tasti *PG ↑ & PG ↓* creare il file *input.conf* nella cartella */home/.config/mpv* con le seguenti righe:

```
PGUP playlist-prev ; show-text "${playlist-pos-1}/${playlist-count}"
PGDWN playlist-next ; show-text "${playlist-pos-1}/${playlist-count}"
```

21. Per evitare conflitti tra le *WebUi* dei servizi installati nel server andiamo a modificare il file `/etc/hosts` nel seguente modo: 

`sudo nano /etc/hosts`  

aggiungiamo al file le seguenti linee  

```
192.168.1.xxx   pi.hole  
192.168.1.xxx   webmin.local  
192.168.1.xxx   portainer.local  
```  
22. **Fish Shell**

`echo 'deb http://download.opensuse.org/repositories/shells:/fish/Debian_13/ /' | sudo tee /etc/apt/sources.list.d/shells:fish.list`
`curl -fsSL https://download.opensuse.org/repositories/shells:fish/Debian_13/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/shells_fish.gpg > /dev/null`
`sudo apt update`
`sudo apt install fish`

- A terminale aperto, cliccare con il tasto destro in un punto e cliccare su *Preferenze*. Andare nella sezione del profilo, tab *Comando*, spuntare l'opzione *Eseguire un comando personalizzato invece della shell* e nella sezione *Comando personalizzato* inserire */usr/bin/fish*  

- Per togliere le due righe del benvenuto del nuovo terminale lanciamo il seguente comando  
`set -U fish_greeting ""`  

- Installare uno dei *[Nerd Font](https://github.com/ryanoasis/nerd-fonts?tab=readme-ov-file)*  
(scaricare uno dei font **[JetBrains](https://www.jetbrains.com/lp/mono/)**, estrarlo dal file archivio e copiarli nella cartella *~/.local/share/fonts*)   
lanciare il comando `fc-cache -f -v` per aggiornare la cahe dei font  
torniamo nelle Preferenze del terminale e nella sezione del profilo settiamo il nerd font installato come predefinito e *11* come dimensione  

> :memo: Se ci sono problemi nella visualizzazione di alcuni font o icone lanciare i seguenti comandi:  
```
git clone --depth 1 https://github.com/ryanoasis/nerd-fonts.git  
cd nerd-fonts  
./install.sh Meslo  
```
`fc-cache -fv`  

- Installare *[fisher](https://github.com/jorgebucaran/fisher)* + plugin *tide*  
seguiamo i passaggi per configurarlo, le mie scelte:  
**3 (Rainbow) - 1 (True color) - 2 (24-hour format) - 2 (Vertical) - 1 (Sharp) - 1 (Flat) - 4 (Two lines, character and frame) - 3 (Solid) - 2 (Yes) - 4 (Darkest) - 2 (Sparse) - 2 (Many icons) - 2 (Yes) - y (Per sovrascrivere i cambiamenti)**  

- Per terminare possiamo aggiungere qualche configurazione al file `/home/nomeutente/.config/fish/config.fish`  
es. *alias clera clear*  

- Digitare `fish_config` per ulteriori configurazioni  

Riavviare  

23. *Montare disco di rete all'avvio*  

* Per verificare che l'utility sia installata lanciare un:  

`sudo apt update && sudo apt install cifs-utils`  

* Crea un File per le Credenziali  

`sudo mkdir -p /etc/samba`  
`sudo nano /etc/samba/credenziali-server`  

* Inserisci le tue credenziali nel file seguendo questa struttura senza spazi intorno al segno =:  

```
username=nome_utente_samba  
password=tua_password  
```

* Crea il Punto di Montaggio Locale  

`sudo mkdir /media/NASm2`  

* Modifica il File */etc/fstab*  

`sudo nano /etc/fstab` 

* Aggiungi la seguente riga alla fine del file, sostituendo i valori segnaposto con i tuoi:  

`//192.168.1.XXX/NASm2 /media/NASm2 cifs username=username,password=password,rw,uid=1000,gid=1000 0 0

* Riavvia  

> :memo: **Note:** se riceviamo un *permisison denied* relativo a *cifs* dobbiamo aggiungere l'utente *samba* con `sudo smbpasswd -a nomeutente`  
