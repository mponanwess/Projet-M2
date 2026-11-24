# Étape 12 — Relecture de la configuration

## Commande envoyée à 10:43:27 (FPort 1)

`C1 0A 04 25 26 1E 1F 20 21 22 23 24`

| Octets | Signification |
| --- | --- |
| `C1` | Commande (sa longueur suit dans l'octet suivant) |
| `0A` | Longueur : 10 octets |
| `04` | Commande « lire des paramètres » |
| `25 26` | Mode du fil pilote, mode par défaut du calendrier |
| `1E` à `24` | Programmes 1 à 7 |

Cette commande s'envoie seule, sans réglage dans la même trame.

## Réponse : deux trames d'en-tête `07`

| Heure | Trame | Contenu |
| --- | --- | --- |
| 10:43:29 | `07 04 25 00 26 02 DE 07 00 24 42 4E 7F 01 03 DF …` | Modes et programmes 1 à 5 |
| 10:43:34 | `07 04 E3 07 00 1E 04 38 00 03 03 E4 07 00 1E 04 38 00 03 03` | Programmes 6 et 7 |

L'en-tête `07`, au lieu de `87`, confirme le décalage prévu à l'étape 11 : le décodeur v3 lit ces trames sans erreur.

## Configuration relue

| Paramètre | Valeur relue |
| --- | --- |
| Mode du fil pilote | 0 : Calendrier |
| Mode par défaut du calendrier | 2 : Éco |
| Programme 1 | 09:40-09:50 UTC, tous les jours, Confort |
| Programmes 2 à 7 | Désactivés (jours = 0, valeurs par défaut `08:00-18:00;0;3`) |

La configuration relue correspond exactement à celle envoyée à l'étape 11.

## Remarques

- Les réponses `06` et `07` ne sont pas horodatées, contrairement aux trames de mesure et de vie.
- TTN a envoyé un downlink automatique juste après la première trame de réponse.

---

[← Étape 11](11-calendrier-et-reglages.md) · [Retour au dossier ATIM](../README.md) · [Étape 13 : bilan et perspectives →](13-bilan-perspectives.md)