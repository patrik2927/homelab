# Saját Homelab

Ebben a repository-ban gyűjtöttem össze a saját, Ubuntu alapú homelab server konfigurációimat (docker-compose.yml fájlokat). Leginkább azért raktam össze, hogy gyakorlatban is kitapasztaljam a hálózatbiztonságot, a container virtualizációt és a szerverüzemeltetést.

## Biztonság és Hozzáférés

Mivel pár szolgáltatás kilát az internetre, eléggé rámentem a biztonságra:

* Érzékeny adatok és jelszavak: Az összes jelszó, API kulcs és érzékeny adat külön environment variable fájlokban (.env) van.
* Reverse proxy és TLS: A bejövő forgalmat a Traefik fogja meg, és ő intézi a Let's Encrypt SSL/TLS certifikátok automatikus frissítését is.
* Védelem (IPS): A CrowdSec figyeli a logokat, és a Traefikkel karöltve vágja el a botnetek, DDoS próbálkozások és egyéb gyanús forgalmak útját.
* Cloudflare: A domain forgalma Cloudflare proxy-n és tunnel-en jön be, így alapból kihasználom az ottani WAF és botvédelmi extrákat.
* Hálózat szétválasztása (VPN): Az admin felületek (Dockge, Traefik dashboard, AdGuard) egyáltalán nem publikusak, ezeket csak Tailscale-en keresztül, privát hálózatból érem el. Kizárólag a napi használatú szolgáltatások (Nextcloud, Vaultwarden, Matrix) vannak nyitva kifelé.

## Futó szolgáltatások

Jelenleg több mint 20 szolgáltatás fut különálló docker container környezetben. A lényegesebbek:

### Hálózat és Biztonság
* Traefik: reverse proxy
* CrowdSec: Viselkedésalapú tűzfal
* Cloudflare DDNS: Dinamikus IP frissítéshez
* AdGuard Home: DNS-szintű reklám- és tracker blokkoló
* Coturn: STUN/TURN szerver a Matrixhoz

### Monitorozás és Admin
* Uptime Kuma: Állapotfigyelés
* Dozzle: Valós idejű logolás a containerekhez
* Dockge: A Docker Compose stackek grafikus kezelője
* Duplicati: Biztonsági mentések készítése

### Napi használatú appok
* Vaultwarden: Saját jelszókezelő 
* Nextcloud: Privát felhőtárhely
* Matrix / Element: Titkosított chat platform
* Média és automatizálás: Jellyfin, Audiobookshelf, illetve a letöltés/menedzsment (Stirling, Homarr, Radarr, Sonarr, Prowlarr, qBittorrent, Jellyseerr)
