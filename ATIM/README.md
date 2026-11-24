# Actionneur fil pilote ATIM ACW/LW8-PW sur The Things Network

Ce dossier décrit, étape par étape et dans l'ordre chronologique, la mise en œuvre d'un actionneur fil pilote LoRaWAN : enregistrement sur TTN, réception de ses mesures (uplinks), envoi d'ordres de chauffage (downlinks) et vérification de chaque ordre à l'oscilloscope.

## Matériel

| Élément | Modèle |
| --- | --- |
| Actionneur fil pilote LoRaWAN | ATIM ACW/LW8-PW |
| Réseau | The Things Network (The Things Stack Sandbox, cluster eu1) |
| Passerelle | Passerelle LoRaWAN du laboratoire |
| Sonde différentielle | Elditest GE 8100 (×20 / ×200) |
| Oscilloscopes | Siglent SDS1202X-E (23/11), Keysight EDUX1052A (24/11) |

## Organisation du dossier

- `docs/` : une page par étape ;
- `images/` : photos des écrans d'oscilloscope ;
- `codec/` : : les trois versions du décodeur TTN (codec ATIM allégé, puis corrigé).

## Sécurité

Le fil pilote est au potentiel du secteur (230 V). Toutes les mesures se font avec une sonde différentielle, et ses pinces se branchent boîtier hors tension.

## Crédits

Le décodeur du dossier `codec/` est dérivé du codec officiel d'ATIM ([atim-codecs](https://github.com/ATIM-radiocommunication/atim-codecs)), publié sous licence MIT. La mention de licence d'origine est conservée en tête du fichier.