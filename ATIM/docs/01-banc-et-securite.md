# Étape 1 — Préparation du banc et sécurité

## Banc de test

| Élément | Rôle |
| --- | --- |
| Boîtier de test | Alimente l'actionneur en 230 V et donne accès au fil pilote et au neutre |
| Sonde différentielle Elditest GE 8100 | Réduit la tension mesurée (×20 ou ×200) et isole l'oscilloscope du secteur ; ±700 V crête, CAT III 600 V |
| Oscilloscope | Affiche la tension entre le fil pilote et le neutre |

## Branchement de la sonde

| Pince | Borne du boîtier | Entrée de la sonde |
| --- | --- | --- |
| Noire | Fil pilote | + |
| Jaune | Neutre | − |

- Calibre de la sonde : ×200. Une sinusoïde de 230 V (±325 V crête) donne alors ±1,6 V en sortie.
- Sur l'oscilloscope, le rapport de sonde doit être réglé sur 200:1, sinon les tensions affichées sont fausses.

## Règles de sécurité

- Le fil pilote est au potentiel du secteur : ne jamais utiliser une sonde d'oscilloscope classique, dont la masse est reliée à la terre.
- Brancher et débrancher les pinces boîtier hors tension.
- La sonde s'éteint seule au bout d'un moment : vérifier son voyant avant chaque mesure.

---

[← Étape 0](00-choix-et-cles.md) · [Retour au dossier ATIM](../README.md) · [Étape 2 : enregistrement sur TTN →](02-enregistrement-ttn.md)