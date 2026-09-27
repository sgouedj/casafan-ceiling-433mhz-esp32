# ESPHome CasaFan Airfusion

Contrôle d'un ventilateur de plafond **CasaFan Airfusion Climate LD-CD52BN** via **Home Assistant** (et Alexa), en remplaçant sa télécommande RF 434 MHz par un ESP32 + module CC1101, sous **ESPHome**.

## Objectif

Le ventilateur est piloté d'origine par une télécommande RF (RH787TG) en 434 MHz OOK. Ce projet reproduit ces trames depuis un ESP32 pour intégrer le ventilateur à Home Assistant sans dépendre du hub CasaFan.

## Matériel

- ESP32 WROOM-32 DevKit (38 broches, CP2102, USB-C)
- Module CC1101 EBYTE E07-M1101D-TH (antenne à ressort)

## Câblage

| CC1101 (E07-M1101D-TH) | ESP32 |
|---|---|
| GND (pin1) | GND |
| VCC (pin2) | 3.3V |
| GDO0 (pin3) | IO4 |
| CSN (pin4) | IO5 |
| SCK (pin5) | IO18 |
| MOSI (pin6) | IO23 |
| MISO (pin7) | IO19 |
| GDO2 (pin8) | IO27 |

## Protocole RF décodé

- Modulation OOK à 434 MHz
- Bit 0 ≈ 310 µs, bit 1 ≈ 690 µs, pulse de synchro ≈ 1050 µs
- Préfixe fixe (ID) sur 12 bits : `001010101011`
- Les 9 commandes de la télécommande (ON/OFF, lumière, été/hiver, vitesses 1 à 6) ont été capturées et confirmées

## État d'avancement

- ✅ Décodage RF complet et confirmé (réception testée, RSSI cohérent)
- 🚧 Transmission en cours de mise au point : plusieurs approches testées (GDO0 manuel, FIFO/OOK, `remote_transmitter` ESPHome) sans succès sur le matériel précédent, suspecté endommagé
- 🔜 Nouveau matériel commandé, re-test de la communication SPI puis de la transmission en cours

## Installation

1. Installer [ESPHome](https://esphome.io/)
2. Copier `secrets.yaml.example` en `secrets.yaml` et renseigner tes identifiants Wi-Fi (ce fichier est ignoré par Git, ne le committe jamais)
3. Flasher la config sur l'ESP32 : `esphome run casafan.yaml`

## Licence

MIT — libre réutilisation, avec mention de la source appréciée.