# Compteur d'eau aquastream → Home Assistant (ESPHome)

Lecture d'un compteur d'eau **aquastream** (Integra Metering) par radio
**wM-Bus 868 MHz** avec une carte **LilyGO T3 LoRa32 V1.6.1**, et affichage
des valeurs dans **Home Assistant** et sur l'écran de la carte.

## Ce que l'on obtient

**Dans Home Assistant**
- Index du compteur (m³), utilisable dans le tableau de bord *Énergie → Eau*
- Index à la fin du mois précédent, volume en retour, durée de vie de la pile
- État du compteur : `OK`, ou `LEAK` (fuite), `BURST` (rupture), `REVERSE`, `LOW_BATTERY`…
- Diagnostic de réception : connecté ou déconnecté, âge de la dernière trame, signal radio

**Sur l'écran de la carte** (deux pages qui alternent ; le bouton IO0 passe à la suivante)
- *Consommation* : index, litres du jour et du mois, heure de la dernière lecture,
  alarme du compteur le cas échéant
- *État* : Wi-Fi, connexion à Home Assistant, dernière trame reçue, signal radio

## Matériel

- LilyGO T3 LoRa32 **V1.6.1**, version **868 MHz**, avec son antenne
- Compteur aquastream avec module radio wM-Bus (mode T1, OMS)
- **La clé AES du compteur**, à demander au distributeur d'eau (la commune)

## Installation

1. Dans ESPHome Device Builder, créez un nouvel appareil (carte *ESP32 Dev Module*).
2. Remplacez son contenu en partant de [`exemple.yaml`](exemple.yaml) :
   - gardez la **clé d'API** et le **mot de passe du point d'accès** générés par
     l'assistant ;
   - renseignez `meter_id`, le numéro du compteur sur l'étiquette du haut
     (par exemple `1234567/2022` donne `01234567` : sans l'année, complété à 8 chiffres).
3. Ajoutez dans le `secrets.yaml` d'ESPHome :
   ```yaml
   compteur_eau_cle: "<32 caractères hexadécimaux>"
   ```
4. Première installation **par USB** (*Installer → Plug into this computer*),
   les suivantes sans fil.

## Bon à savoir

- **Ne jamais publier** sa clé de compteur, sa clé d'API ni ses identifiants Wi-Fi :
  ils restent dans votre fichier local et votre `secrets.yaml`.
- Pointez vers une **version précise** du dépôt (`@v1.0.0`), pas vers la branche
  principale. Sinon, une modification du dépôt change votre firmware à la prochaine
  compilation.
- Sur cette carte, **GPIO16 est réservé** à la mémoire interne de l'ESP32-PICO-D4 :
  ne pas l'utiliser (par exemple comme reset d'écran), sous peine de plantage au
  démarrage.
- Testé avec ESPHome **2026.11.0** et le composant wM-Bus d'IoTLabs (révision figée
  dans le fichier commun).

## Licence

Les fichiers de ce dépôt sont publiés sous la licence
[PolyForm Noncommercial 1.0.0](LICENSE.md) : usage libre à des fins **non
commerciales** (usage personnel, associations, recherche, etc.).

Le firmware compilé intègre en outre des composants tiers qui ont leur propre
licence, en particulier le composant wM-Bus d'[IoTLabs](https://github.com/IoTLabs-pl/ESPHome-Components)
et les décodeurs [wmbusmeters](https://github.com/wmbusmeters/wmbusmeters),
tous deux sous **GPL-3.0**. La licence ci-dessus ne s'applique qu'aux fichiers
de ce dépôt.
