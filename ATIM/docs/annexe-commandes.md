# Annexe — Dictionnaire des commandes (hexadécimal → JSON)

Chaque commande part sur le FPort 1. La colonne JSON est l'écriture lisible de la même commande : c'est l'entrée de la fonction `encodeDownlink` du codec ATIM, qui produit exactement les octets de la colonne Hexadécimal. Le dictionnaire brut est dans [`codec/commandes.json`](../codec/commandes.json).

## Mode du fil pilote

| Hexadécimal | JSON | Effet |
| --- | --- | --- |
| `25 00` | `{"setPilotWireMode": 0}` | Suivre le calendrier |
| `25 01` | `{"setPilotWireMode": 1}` | Confort |
| `25 02` | `{"setPilotWireMode": 2}` | Éco |
| `25 03` | `{"setPilotWireMode": 3}` | Hors gel |
| `25 04` | `{"setPilotWireMode": 4}` | Arrêt |
| `25 05` | `{"setPilotWireMode": 5}` | Confort −1 °C |
| `25 06` | `{"setPilotWireMode": 6}` | Confort −2 °C |

## Mode par défaut du calendrier

| Hexadécimal | JSON | Mode appliqué hors des programmes |
| --- | --- | --- |
| `26 01` | `{"setPilotWireCalendarDefaultMode": 1}` | Confort |
| `26 02` | `{"setPilotWireCalendarDefaultMode": 2}` | Éco |
| `26 03` | `{"setPilotWireCalendarDefaultMode": 3}` | Hors gel |
| `26 04` | `{"setPilotWireCalendarDefaultMode": 4}` | Arrêt |
| `26 05` | `{"setPilotWireCalendarDefaultMode": 5}` | Confort −1 °C |
| `26 06` | `{"setPilotWireCalendarDefaultMode": 6}` | Confort −2 °C |

## Programmes du calendrier

Le premier octet désigne le programme : `DE` pour le programme 1, jusqu'à `E4` pour le programme 7.

| Hexadécimal | JSON | Effet |
| --- | --- | --- |
| `DE 07 00 1E 04 38 1F 01 03` | `{"setProgram1": "08:00-18:00;31;1"}` | Programme 1 : Confort du lundi au vendredi, 08:00-18:00 UTC |
| `DF 07 00 1E 04 38 00 03 03` | `{"setProgram2": "08:00-18:00;0;3"}` | Programme 2 désactivé (jours = 0) |

## Trame de vie

| Hexadécimal | JSON | Période |
| --- | --- | --- |
| `03 00` | `{"setKeepAlivePeriod": "Disable"}` | Désactivée |
| `03 04` | `{"setKeepAlivePeriod": "Once every 10 mins"}` | Toutes les 10 min |
| `03 05` | `{"setKeepAlivePeriod": "Once every hour"}` | Toutes les heures |
| `03 0A` | `{"setKeepAlivePeriod": "Once every 2 hours"}` | Toutes les 2 h |
| `03 0B` | `{"setKeepAlivePeriod": "Once every 4 hours"}` | Toutes les 4 h |
| `03 0C` | `{"setKeepAlivePeriod": "Once every 8 hours"}` | Toutes les 8 h |
| `03 06` | `{"setKeepAlivePeriod": "Once every day"}` | Une par jour |
| `03 0D` | `{"setKeepAlivePeriod": "Once every 2 days"}` | Tous les 2 jours |
| `03 0E` | `{"setKeepAlivePeriod": "Once every 3 days"}` | Tous les 3 jours |
| `03 0F` | `{"setKeepAlivePeriod": "Once every 4 days"}` | Tous les 4 jours |
| `03 07` | `{"setKeepAlivePeriod": "Once every week"}` | Une par semaine |
| `03 08` | `{"setKeepAlivePeriod": "Once every month (30 days)"}` | Une par mois (30 jours) |

Les libellés français du codec (« Tous les jours »…) donnent les mêmes octets.

## Enregistrement et horodatage

| Hexadécimal | JSON | Effet |
| --- | --- | --- |
| `D4 03 01 0A 00` | `{"setDataloggingPeriod": 10}` | Enregistrement toutes les 10 min |
| `D4 03 01 3C 00` | `{"setDataloggingPeriod": 60}` | Enregistrement toutes les 60 min |
| `D4 03 01 2C 01` | `{"setDataloggingPeriod": 300}` | Enregistrement toutes les 300 min |
| `0B 00` | `{"setTimestamp": 0}` | Horodatage désactivé |
| `0B 01` | `{"setTimestamp": 1}` | Horodatage activé |

La période d'enregistrement est en minutes, sur deux octets, octet de poids faible en premier : 300 = 0x012C s'écrit `2C 01`.

## Relecture de la configuration

| Hexadécimal | JSON | Effet |
| --- | --- | --- |
| `C1 0A 04 25 26 1E 1F 20 21 22 23 24` | `{"getPilotWireConfiguration": 1}` | Relit les modes et les 7 programmes ; à envoyer seule |

## Commande groupée (étape 11)

Plusieurs réglages tiennent dans un seul downlink : les clés JSON se regroupent dans un même objet, et les octets se suivent.

`0B 01 03 06 D4 03 01 0A 00 25 00 26 02 DE 07 00 24 42 4E 7F 01 03`

```json
{
  "setTimestamp": 1,
  "setKeepAlivePeriod": "Once every day",
  "setDataloggingPeriod": 10,
  "setPilotWireMode": 0,
  "setPilotWireCalendarDefaultMode": 2,
  "setProgram1": "09:40-09:50;127;1"
}
```

## Envoyer une commande en JSON depuis TTN

1. Payload formatters → **Downlink** → Custom Javascript formatter → coller `acw-pw-codec-ttn-v3.js` → **Save changes**.
2. Messaging → Schedule downlink → FPort 1 → payload de type **JSON**, par exemple `{"setPilotWireMode": 2}`.

TTN traduit alors le JSON en octets avec `encodeDownlink`, comme dans ce dictionnaire. Cet envoi en JSON n'a pas encore été testé sur le module.

---

[Retour au dossier ATIM](../README.md)