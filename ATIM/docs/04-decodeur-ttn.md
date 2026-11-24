# Étape 4 — Décodeur TTN allégé

## Problème

Le codec officiel d'ATIM ([atim-codecs](https://github.com/ATIM-radiocommunication/atim-codecs), dossier `fr/environnement/acw-pw`, licence MIT) fait 65 272 octets. TTN refuse un décodeur de plus de 40 960 octets.

## Solution : minification

Le code est minifié avec [terser](https://github.com/terser/terser), en JavaScript ES5 accepté par TTN. La licence MIT d'ATIM est recopiée en tête du fichier.

```js
const fs = require('fs');
const { minify } = require('terser');

const src = fs.readFileSync('acw-pw-codec.js', 'utf8');
const licence = fs.readFileSync('LICENSE', 'utf8');   // licence MIT d'ATIM

minify(src, {
  ecma: 5,
  compress: { passes: 2 },
  mangle: true,
  format: { comments: false, preamble: '/*\n' + licence + '*/' }
}).then(r => fs.writeFileSync('acw-pw-codec-ttn.js', r.code));
```

| Fichier | Taille |
| --- | --- |
| Codec ATIM d'origine | 65 272 octets |
| Codec minifié (v1) | 30 126 octets |

Vérification : les 23 exemples fournis par ATIM (`examples.json`) donnent le même résultat avec le codec d'origine et le codec minifié.

## Installation dans TTN

1. End device → **Payload formatters** → **Uplink**.
2. Formatter type : **Custom Javascript formatter**.
3. Coller le contenu du fichier, puis **Save changes**.

## Test

Dans la zone de test du formatter, saisir une vraie trame : Byte payload `85 08 08 BA 09 13 0E`, FPort 5.

```json
{ "frame_type": "Trame de test", "temperature0": 22.34, "humidity0": 48.78 }
```

Un Byte payload vide donne l'erreur « Unknown frame type for payload 00 » : ce n'est pas un défaut du décodeur.

Les trois versions du décodeur sont dans [`codec/`](../codec/) : v1 (cette étape), v2 (étape 8) et v3 (étape 11).

---

[← Étape 3](03-join-premieres-trames.md) · [Retour au dossier ATIM](../README.md) · [Étape 5 : oscilloscope Siglent →](05-oscilloscope-siglent.md)