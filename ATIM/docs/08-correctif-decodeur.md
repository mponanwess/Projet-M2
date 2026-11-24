# Étape 8 — Correctif du décodeur pour la réponse `06 00`

## Problème

Après chaque downlink de configuration (par exemple `25 02`), le module répond par un uplink `06 00` sur le FPort 5. Le décodeur v1 ne le reconnaît pas :

```json
{ "frame_type": "Type de trame inconnu", "errors": ["Unknown frame type for payload 0600"] }
```

Le codec ATIM attend `86 00` : dans le format récent d'ATIM, le bit 7 de l'en-tête vaut 1. Le module envoie cette réponse avec le bit 7 à 0, et le codec la prend pour une trame d'un ancien produit.

## Correctif (v2)

La fonction `decodeUplink` d'origine est renommée `decodeUplinkATIM`. Une nouvelle fonction `decodeUplink` relit l'en-tête `06` comme `86` avant d'appeler le codec ATIM :

```js
function decodeUplink(input) {
    var b = input && input.bytes;
    if (b && b.length > 0 && b[0] === 0x06) {
        var copie = [];
        for (var i = 0; i < b.length; i++) copie.push(b[i]);
        copie[0] = 0x86;
        return decodeUplinkATIM({ bytes: copie, fPort: input.fPort, variables: input.variables });
    }
    return decodeUplinkATIM(input);
}
```

Le fichier est ensuite minifié comme à l'étape 4 : [`acw-pw-codec-ttn-v2.js`](../codec/acw-pw-codec-ttn-v2.js), 30 496 octets. Les 23 exemples d'ATIM donnent toujours le même résultat.

## Résultat

Avec le décodeur v2 installé dans TTN, la réponse se lit :

```json
{ "frame_type": "Réponse à une trame de configuration", "configuration_acceptee": 1, "parametres_refuses": [] }
```

## Lecture de la réponse

Le premier octet indique une réponse à une trame de configuration. Les octets suivants forment un masque des paramètres refusés, numérotés à partir de 1 dans l'ordre de la trame envoyée.

| Réponse | Signification |
| --- | --- |
| `06 00` | Tous les paramètres sont acceptés |
| `06 05` | Paramètres 1 et 3 refusés (bits 0 et 2) |

---

[← Étape 7](07-quatre-ordres.md) · [Retour au dossier ATIM](../README.md) · [Étape 9 : oscilloscope Keysight →](09-oscilloscope-keysight.md)