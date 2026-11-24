# Étape 2 — Enregistrement du module sur TTN

## Paramètres saisis

Console TTN (The Things Stack Sandbox, cluster eu1), dans une application, par saisie manuelle :

| Paramètre | Valeur |
| --- | --- |
| Plan de fréquences | Europe 863-870 MHz (EU868) |
| Version LoRaWAN | 1.0.4 |
| Paramètres régionaux | RP002 1.0.3 |
| Activation | OTAA |
| DevEUI | Étiquette du module |
| JoinEUI et AppKey | Fournis par l'encadrant |

## Signification des versions

| Version | Ce qu'elle fixe |
| --- | --- |
| LoRaWAN 1.0.4 | Le protocole du module. Le DevNonce de chaque demande de join est un compteur : TTN refuse une valeur déjà utilisée, ce qui empêche de rejouer une ancienne demande. |
| RP002 1.0.3 | Les paramètres régionaux : fréquences, débits et tailles de trame autorisés en Europe (EU868). |

Ces deux versions doivent correspondre à ce que le module implémente (documentation ATIM).

## Remarque

Un couple DevEUI / JoinEUI ne peut être enregistré qu'une fois par cluster TTN. Pour repartir de zéro, le end device doit être supprimé.

---

[← Étape 1](01-banc-et-securite.md) · [Retour au dossier ATIM](../README.md) · [Étape 3 : join et premières trames →](03-join-premieres-trames.md)