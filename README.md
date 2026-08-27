# Projets Cisco Packet Tracer

Ensemble de projets réseaux réalisés sous **Cisco Packet Tracer**. Chaque dossier documente un scénario réseau complet : conception, adressage, configuration CLI, et vérifications.

Les projets sont classés par ordre chronologique et suivent une progression technique, du plus simple (interconnexion basique de deux sites) au plus complexe (segmentation VLAN, routage inter-VLAN, VPN, puis routage statique multi-routeurs).

---

## 🗂 Projets

| N° | Projet | Description |
|---|---|---|
| **01** | [Interconnexion multi-sites – DHCP, DNS, HTTP](./01-interconnexion-multi-sites/README.md) | Interconnexion de deux réseaux d'entreprise avec services DHCP, DNS et hébergement intranet |
| **02** | [Réseau complexe — VLANs et routage](./02-reseau-complexe-vlan-routage/README.md) | Infrastructure multi-services avec VLANs, routage inter-VLAN, SSH, Wi-Fi et VPN IPsec |
| **03** | [Routage statique multi-routeurs](./03-Routage-Statique-Multi-Routeurs/README.md) | Configuration d'une infrastructure réseau avec 4 routeurs et plusieurs LAN |

---

## 🎯 Compétences mobilisées

- Conception d'un plan d'adressage IP cohérent (sous-réseaux, VLSM)
- Configuration de services réseau : DHCP, DNS, serveur HTTP
- Segmentation logique avec les VLANs et routage inter-VLAN (SVI)
- Routage statique et interconnexion multi-routeurs
- Interconnexion de sites distants en VPN IPsec
- Sécurisation des équipements Cisco : accès SSH, mots de passe chiffrés, bannières légales
- Vérification et diagnostic (ping, traceroute, `show` commands)

---

## 🧰 Outil utilisé

[Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer) — simulateur réseau utilisé pour concevoir, configurer et tester chaque topologie avant toute mise en situation réelle.


> [!NOTE]
>Chaque projet suit la même logique de documentation : un **README.md** pour présenter le contexte, les objectifs et les compétences mobilisées, et un **PROCEDURE.md** pour le détail technique pas à pas (schémas, plans d'adressage, commandes CLI, vérifications).
