# NextcloudProjektPss
## Popis projektu
Tento projekt se zaměřuje na vytvoření vlastního domácího cloudového úložiště pomocí **Nextcloud serveru**, který běží na lokálním serveru a je dostupný odkudkoliv přes zabezpečenou VPN síť.
Cílem projektu je:
- mít plnou kontrolu nad svými daty
- vytvořit alternativu ke službám jako Google Drive nebo Dropbox
- zajistit bezpečný vzdálený přístup pomocí VPN

---

## Co je Nextcloud?
**Nextcloud** je open-source platforma pro cloudové úložiště, která umožňuje:
- ukládání a sdílení souborů
- synchronizaci mezi zařízeními
- správu kalendáře, kontaktů a poznámek
- spolupráci (např. dokumenty)

Výhody:
- běží na vlastním serveru
- plná kontrola nad daty
- žádné měsíční poplatky

---

## Co je Tailscale?
**Tailscale** je VPN služba, která:
- propojí tvoje zařízení do jedné privátní sítě
- umožní bezpečný přístup k serveru odkudkoliv
- funguje bez složité konfigurace port forwarding

Výhody:
- jednoduché nastavení
- vysoká bezpečnost (WireGuard)
- funguje i za NATem

---

## Instalace Nextcloud přes Snap

Snap je nejrychlejší způsob jak rozjet Nextcloud — vše (Apache, PHP, databáze) je zabaleno v jednom balíčku.

### 1. Instalace Snap (pokud ještě není)
```bash
sudo apt update
sudo apt install snapd -y
```

### 2. Instalace Nextcloudu
```bash
sudo snap install nextcloud
```

### 3. Vytvoření admin účtu
```bash
sudo nextcloud.manual-install uzivatel silne_heslo
```

### 4. Povolení firewallu (volitelné)
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Nextcloud teď běží na `http://<IP-adresa-serveru>` — to je vše!

---

## Instalace Tailscale na Ubuntu Server

### 1. Přidání repozitáře a instalace
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

### 2. Spuštění a přihlášení
```bash
sudo tailscale up
```
Terminál zobrazí odkaz — otevři ho v prohlížeči a přihlas se svým Tailscale účtem.

### 3. Zjištění Tailscale IP adresy serveru
```bash
tailscale ip -4
```
Tato IP (ve formátu `100.x.x.x`) slouží pro přístup k Nextcloudu odkudkoliv přes VPN.

### 4. Přidání Tailscale IP do trusted domains Nextcloudu
```bash
sudo nextcloud.occ config:system:set trusted_domains 1 --value=100.x.x.x
```
Nahraď `100.x.x.x` svojí skutečnou Tailscale IP adresou.

Nextcloud je nyní dostupný na `http://100.x.x.x` ze všech zařízení připojených do tvé Tailscale sítě.
