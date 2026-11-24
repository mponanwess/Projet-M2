# Étape 3 — Join et premières trames

## Join (OTAA)

À la mise sous tension, le module envoie une demande de join. Dans Live data, TTN affiche :

| Événement Live data | Signification |
| --- | --- |
| Accept join-request | TTN accepte la demande et attribue une adresse réseau (DevAddr) |
| Forward join-accept message | La réponse part vers le module, par la passerelle |
| Forward uplink data message | Une trame montante est reçue |

## Rythme des premières trames

| Moment | Trame | En-tête |
| --- | --- | --- |
| Pendant environ 5 min après le join | Trame de test, une par minute | `85` |
| Ensuite | Trame de vie (tensions du module) | `81` |
| Ensuite | Trame de mesure (température, humidité) | `A0` |

Chaque trame de test et de vie contient une demande d'heure (DeviceTimeReq) : TTN y répond par un downlink.

## Décodage à la main

Trame de test reçue sur le FPort 5 : `85 08 08 BA 09 13 0E`

| Octets | Signification |
| --- | --- |
| `85` | En-tête : trame de test (bit 7 à 1 : format récent ATIM) |
| `08` | Type de mesure : température |
| `08 BA` | 0x08BA = 2234, soit 22,34 °C |
| `09` | Type de mesure : humidité |
| `13 0E` | 0x130E = 4878, soit 48,78 % |

---

[← Étape 2](02-enregistrement-ttn.md) · [Retour au dossier ATIM](../README.md) · [Étape 4 : décodeur TTN →](04-decodeur-ttn.md)