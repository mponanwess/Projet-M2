# Étape 6 — Premiers downlinks : blocage, classe C et ADR statique

## Envoi d'un downlink

Console TTN → end device → **Messaging** → **Schedule downlink** :

| Champ | Valeur |
| --- | --- |
| FPort | 1 (les uplinks arrivent sur le FPort 5) |
| Payload type | Bytes |
| Bytes | `25 02` (mode Éco) |

## Problème : downlinks bloqués

| Heure | Downlink | Ce qu'affiche Live data |
| --- | --- | --- |
| 11:20:57 | `25 02` (Éco) | « Receive downlink data message », mais aucun « Schedule data downlink for transmission » |
| 11:38:21 | `25 01` (Confort) | Même chose : l'ordre reste dans la file d'attente |

Redémarrer le boîtier débloquait la file d'attente, mais ce n'était qu'un contournement.

## Causes

1. **Classe C non déclarée** : la case « Supports class C » n'était pas cochée. TTN traitait le module comme un équipement de classe A et attendait un uplink pour lui transmettre le downlink.
2. **ADR dynamique** : TTN envoyait des commandes LinkADRReq. Le module n'y répond qu'à son uplink suivant, et TTN retient les downlinks tant qu'une commande MAC attend sa réponse.

## Solution

Settings → **Network layer**, après avoir rechargé la page (une page restée ouverte peut afficher d'anciens réglages) :

| Réglage | Valeur |
| --- | --- |
| Supports class C | Coché |
| Class C timeout | 300 s |
| ADR | Static mode |
| Data rate index | 5 (SF7BW125) |
| TX power index | 0 |
| Nombre de transmissions | 1 |

Enregistrer, puis redémarrer le module une fois.

## Résultat

Le downlink part dans la seconde, et le module répond `06 00` environ une seconde plus tard. Le décodage de cette réponse fait l'objet de l'étape 8.

---

[← Étape 5](05-oscilloscope-siglent.md) · [Retour au dossier ATIM](../README.md) · [Étape 7 : mesure des quatre ordres →](07-quatre-ordres.md)