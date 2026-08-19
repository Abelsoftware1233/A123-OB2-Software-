# Diagnose.Link — OBD2 Web Bluetooth app

Dit bestand (`obd2-dashboard-real.jsx`) praat écht met een fysieke OBD2-adapter
via de Web Bluetooth API. Geen simulatie — dit stuurt daadwerkelijke
ELM327-commando's naar je auto en leest de responses uit.

## Wat de app echt doet

- **Verbinden**: klik "Zoek adapter" → de browser toont een systeem-dialoog
  met gevonden BLE-apparaten → na selectie stuurt de app de
  ELM327-initialisatiesequentie (`ATZ`, `ATE0`, `ATL0`, `ATSP0`).
- **Live data**: elke seconde worden RPM, snelheid, koelvloeistoftemperatuur,
  motorbelasting, brandstofniveau en accuspanning uitgelezen via echte
  OBD-PID-commando's, waarna de hex-response wordt geparsed naar leesbare
  waarden.
- **Foutcodes lezen**: stuurt mode `03`, decodeert de hex-bytes naar echte
  DTC-codes (bv. P0301) volgens de SAE J1979-standaard.
- **Foutcodes wissen**: stuurt mode `04` — met een bevestigingsscherm dat
  uitlegt dat dit het onderliggende probleem niet oplost, alleen het
  geheugen van de ECU wist.

## Belangrijke beperking: alleen BLE, geen Bluetooth Classic

Web Bluetooth API (wat een browser/webapp kan gebruiken) ondersteunt alleen
**BLE (Bluetooth Low Energy)**. Bluetooth Classic — wat de meeste goedkope
ELM327-adapters gebruiken (SPP-profiel, serial-over-Bluetooth) — is via de
browser **niet toegankelijk**, op geen enkel platform. Dat zit in hoe
browsers werken en is een bewuste veiligheidskeuze van browserbouwers, geen
beperking die met code is op te lossen.

| Adaptertype | Werkt met deze webapp? |
|---|---|
| BLE OBD2-adapter | Ja |
| Bluetooth Classic / SPP OBD2-adapter | Nee — vereist een native app (Android) |

Herken een BLE-adapter aan de productomschrijving: zoek naar "OBD2 BLE" of
"OBD2 Bluetooth LE/4.0" in plaats van gewoon "OBD2 Bluetooth".

### Als je toch een Classic-adapter hebt

- Werkt native op **Android** (via `BluetoothSocket`), zoals apps als Torque
  al jaren doen — vereist een aparte native app, geen webapp.
- Werkt in de praktijk **niet op iOS**: Apple staat SPP-Bluetooth Classic
  niet toe voor losse consumenten-apparaten zonder MFi-certificering
  (Apple's eigen accessoire-programma).

## Platformondersteuning

Alleen **Chrome of Edge**, op **Windows, macOS of Android**.

- **Geen Safari, geen iOS** — Apple ondersteunt de Web Bluetooth API niet in
  Safari, op geen enkel Apple-platform.
- Voor volledige iOS-ondersteuning is een native app nodig, geen webapp.

## Grootste risico bij eerste gebruik: UUID-onzekerheid

BLE ELM327-adapters communiceren via een UART-achtige service, maar de
exacte service-UUIDs verschillen per fabrikant/chipset. De app heeft twee
veelvoorkomende profielen ingebouwd:

- **Nordic UART (NUS)** — meest voorkomend bij goedkope BLE OBD2-adapters
- **Vgate/OBDLink-stijl**

Er zijn echter tientallen chipsets in omloop. Als jouw adapter niet
verbindt of de app "protocol niet herkend" meldt, gebruikt jouw adapter
waarschijnlijk een ander BLE-profiel. Geef in dat geval het merk/model van
de adapter door, of de exacte naam/het MAC-adres dat in de
Bluetooth-kieslijst verschijnt, zodat de UUID's daarop afgestemd kunnen
worden.

## Voorwaarden om te testen

1. Een **BLE (Bluetooth Low Energy)** ELM327-adapter — geen Classic/SPP.
2. **Chrome of Edge**, op Windows, macOS of Android.
3. **Motor aan** (of contact op stand II) — zonder spanning op de
   OBD2-poort reageert de adapter niet.

## Belangrijk over foutcodes wissen

Het wissen van foutcodes (mode `04`) wist het geheugen van de ECU, niet het
onderliggende mankement. Als het probleem er nog is, komt de code na een
paar ritten vaak terug. Dit is vooral bedoeld om te bevestigen dat een
reparatie het probleem écht heeft opgelost.
