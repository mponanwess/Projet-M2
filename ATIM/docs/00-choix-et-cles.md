# Étape 0 — Choix de l'actionneur et obtention des clés

## Ce qu'il faut pour rejoindre le réseau

L'actionneur rejoint le réseau en OTAA (*Over-The-Air Activation*). Il lui faut trois identifiants :

| Identifiant | Rôle | Provenance |
| --- | --- | --- |
| DevEUI | Identifiant unique de l'équipement | Étiquette du module |
| JoinEUI | Identifiant du serveur de jonction | Fabricant, via l'encadrant |
| AppKey | Clé secrète (AES-128) dont dérivent les clés de session | Fabricant, via l'encadrant |

---

[Retour au dossier ATIM](../README.md) · [Étape 1 : préparation du banc et sécurité →](01-banc-et-securite.md)