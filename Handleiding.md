Kans van slagen met de webapp op Android: hoog, mits je de juiste adapter koopt.
Wat te zoeken op AliExpress
Zoek specifiek op "OBDLink", "Vgate iCar Pro BLE" of "ELM327 BLE 4.0" — vermijd listings die alleen "Bluetooth" of "Bluetooth 3.0" zeggen zonder "BLE" of "4.0/5.0" erbij.
Check de reviews/Q&A op de listing zelf op het woord "BLE" — kopers vragen dit vaak letterlijk.
Veiligste optie: de Vgate iCar Pro BLE is een bekend, betrouwbaar model dat ook op AliExpress te vinden is en met naam al is ingebouwd in mijn code (het "Vgate/OBDLink-stijl" profiel).
Waarom dit nog steeds geen 100% garantie is
Zelfs een correct gelabelde BLE-adapter kan een andere chipset/UUID gebruiken dan de twee profielen die ik heb ingebouwd. Realistisch scenario: je bestelt, sluit aan, en de app zegt "protocol niet herkend" bij de eerste poging.
Wat ik dan voor je kan doen
Als dat gebeurt, stuur me:
De exacte productlink/naam van wat je hebt besteld
De naam die verschijnt in de Bluetooth-kieslijst als je op "Zoek adapter" klikt
Dan zoek ik de juiste service-UUID's voor die specifieke chipset op en voeg ik ze toe aan de KNOWN_UART_SERVICES-lijst — dat is een kleine aanpassing, geen herbouw.
Kort samengevat: ja, met de juiste BLE-adapter en je Android-telefoon (Chrome) kan je straks live data uitlezen én foutcodes wissen via de webapp. Het enige echte risico zit in het correct identificeren van een BLE-adapter tussen de wirwar aan bijna-identieke AliExpress-listings.
