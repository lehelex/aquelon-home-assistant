# AQUELON Local – Home Assistant

Az AQUELON vezérlő helyi Home Assistant-kiegészítője. A saját HA-gépeden fut,
a saját helyi MQTT-szolgáltatásodat használja. A normál adatúthoz nem szükséges
az AQUELON felhője vagy külön összekötő szerver.

[![Hozzáadás a Home Assistanthoz](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Flehelex%2Faquelon-home-assistant)

Tároló címe: `https://github.com/lehelex/aquelon-home-assistant`

## Telepítés

1. Home Assistant OS alatt legyen működő Mosquitto/MQTT-szolgáltatás és a HA
   beépített MQTT-integrációja. Minimum HA Core: 2026.10.0.
2. A fenti gombbal add hozzá ezt a tárolót, vagy a Beállítások → Kiegészítők /
   Apps → Tárolók menüben add meg a címét.
3. Telepítsd az **AQUELON Local** kiegészítőt.
4. A beállításokban add meg a saját vezérlőd helyi címét, állandó
   eszközazonosítóját és nevét. A példacím/all-zero ID nem automatikus felismerés.
5. Vezérléshez add meg a készülék meglévő tulajdonosi PIN-jét. Ide nem a
   HA-jelszó és nem a PWA-jelszó kerül. Csak megfigyeléshez kapcsold ki a
   Vezérlés engedélyezése opciót.
6. Mentés, majd Indítás. Az eszköz az MQTT-integráció alatt jelenik meg.

Az MQTT-jelszót a kiegészítő automatikusan a HA Supervisor szolgáltatásától kapja.
Nem kell a vezérlő központi MQTT-beállítását átírni. A routerben foglald le a
vezérlő IP-címét. A kiegészítő egy vezérlőt kezel.

## Mit kapsz?

20 entitást: víz- és kültéri hőmérséklet, páratartalom, célértékek, üzemmód,
Komfort-célérték állítása, időzített kényelmi kapcsolók, kézi keringetési kérés,
kimeneti és hibajelzések, diagnosztika. A fűtés/pumpa állapota információ;
nincs közvetlen fűtő- vagy szivattyúrelé-kapcsoló. A Temper célértéke egyelőre
csak olvasható. Foglalás, vegyszer, hibanyugtázás, szerviz-felülbírálat 
nem része ennek az integrációnak.

A vezérlő védelmei, foglalásai és automatikus kikapcsolásai változatlanul
működnek. A HA leállása nem állítja le a készülék önálló vezérlését. Nincs
offline parancssor vagy régi parancsok visszajátszása.

## Már működő helyi/önálló összekötő cseréje

Az új kiegészítőt előbb telepítsd és konfiguráld, indítás nélkül. Állítsd le a
régit, kapcsold ki az automatikus indulását, majd indítsd az újat. A két példány
nem futhat egyszerre ugyanahhoz a készülékhez. Őrizd meg a készülékazonosítót,
a discovery-előtagot és a vezérlés/csak megfigyelés módot: így megmaradnak az
entitásazonosítók, előzmények és automatizálási hivatkozások. A HA MQTT-eszközt
nem kell törölni. A helyi `local_aquelon_local` és a tárolós app külön telepítés.

## Frissítés és adatvédelem

A HA a tároló új verzióját frissítésként jelzi. A kiadások kész amd64/ARM64
képet használnak; a felhasználó gépén nem kell forrást fordítani. A telepítéshez
és frissítéshez internet kell, a normál helyi adatúthoz nem.

A PIN a saját HA védett appbeállításában marad. A maszkolt mező nem
lemeztitkosítás: a HA-t és mentéseit védd. A készülék meglévő helyi HTTP
bejelentkezése megbízható LAN-t feltételez. Ne nyisd ki az eszköz HTTP/MQTT
portját az internetre. PIN-t, jelszót és teljes konfigurációt ne tegyél GitHub-issue-ba.

[Részletes dokumentáció (EN)](aquelon_local/DOCS.md) ·
[Kiadások](aquelon_local/CHANGELOG.md)

## A forrás láthatósága

Ez a nyilvános tároló kizárólag telepítési leírókat és dokumentációt tartalmaz.
Az AQUELON Local saját forrása és buildfolyamata privát. A HA kész, bytecode-ot
tartalmazó konténerképet tölt le; ez nem jelent teljes visszafejtés elleni védelmet.
