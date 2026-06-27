# Compte Rendu de TP – Configuration Réseau avec Cisco Packet Tracer
### Interconnexion des réseaux Taco et Biclou

| | |
|---|---|
| **Nom** | Clara |
| **Classe** | SIO 27 |
| **Logiciel** | Cisco Packet Tracer |

**Objectif :** Mettre en place, faire évoluer et sécuriser un réseau inter-entreprises avec partage de ressources, adressage dynamique, résolution de noms et hébergement web.

---

## Sommaire

1. [Introduction et topologie du réseau](#1-introduction-et-topologie-du-réseau)
2. [Première mission – Connexion au serveur de fichiers (Taco)](#2-première-mission--connexion-au-serveur-de-fichiers-taco)
3. [Deuxième mission – Partage des locaux Taco / Biclou + activation ports Gigabit](#3-deuxième-mission--partage-des-locaux-taco--biclou--activation-des-ports-gigabit)
4. [Troisième mission – Commerciaux itinérants + serveur DHCP chez Biclou](#4-troisième-mission--commerciaux-itinérants--serveur-dhcp-chez-biclou)
5. [Quatrième mission – Serveur DNS + site intranet chez Biclou](#5-quatrième-mission--serveur-dns--site-intranet-intranetbicloufr)
6. [Conclusion – Difficultés rencontrées et compétences acquises](#6-conclusion--difficultés-rencontrées-et-compétences-acquises)

---

## 1. Introduction et topologie du réseau

Ce TP simule l'interconnexion de deux entreprises via un cœur de réseau :

| Zone | Couleur | Contenu |
|------|---------|---------|
| **Taco** | 🟠 Orange | Réseau local avec PC filaires, laptops en Wi-Fi, serveur de fichiers |
| **Biclou** | 🔵 Bleu | Location de vélos, plusieurs postes de travail, laptops, serveur |
| **Cœur réseau** | 🔴 Rouge | Routeur Cisco 2911 reliant les deux sites |

La topologie évolue au fil des missions avec l'ajout de matériel, la configuration de services (DHCP, DNS, HTTP) et la vérification de la connectivité inter-entreprises.

![Schéma Final](images/0.png)

---

## 2. Première mission – Connexion au serveur de fichiers (Taco)

### Objectif

Configurer un réseau pour l'auto-école Taco :
- **1.a)** Relier un ordinateur au serveur de fichiers
- **1.b)** Ajouter un deuxième PC au même serveur

### Réalisations

Au départ, très peu d'équipements étaient connectés sur Switch3 (situation quasi *from scratch* sur cette partie).

**Étape 1 – Connexion du premier PC**

Connexion de PC0 au serveur avec un câble droit.

![Cablage PC0](images/3.png)

**Étape 2 – Configuration IP statique sur PC0**

| Paramètre | Valeur |
|-----------|--------|
| Adresse IP | `192.168.0.1` |
| Masque de sous-réseau | `255.255.255.0` |
| Passerelle par défaut | `192.168.0.254` |

**Étape 3 – Test de connectivité**

```
ping 192.168.0.1  →  réussi ✅
```

**Étape 4 – Tentative d'ajout d'un deuxième PC → échec ❌**

Message Packet Tracer : `No available port` (plus de ports libres sur Switch3).

![Erreur No Available Ports](images/2.png)

### Solution appliquée

Ajout d'un switch supplémentaire dans la zone Taco :

1. Ajout d'un switch
2. Branchement de PC0, Laptop1 et Server0 sur ce switch
3. Configuration IP statique sur le deuxième PC : `192.168.0.2 /24`
4. Tests finaux : ping PC ↔ serveur et PC ↔ PC → **réussis ✅**

![Ping PC](images/5.png)

---

## 3. Deuxième mission – Partage des locaux Taco / Biclou + activation des ports Gigabit

### Objectif

Permettre le partage de données entre Taco et l'entreprise Biclou via le cœur réseau.

### Réalisations

**Étape 1 – Création du mini réseau local Biclou**

- Ajout de PC1 et PC2
- Ajout d'un switch pour connecter les machines entre elles

**Tableau récapitulatif des adresses IP**

| **Taco** | | **Biclou** | |
|----------|---|------------|---|
| **Terminal** | **IP** | **Terminal** | **IP** |
| Server0 | `192.168.0.1` | PC1 | `192.168.100.1` |
| PC0 | `192.168.0.2` | PC2 | `192.168.100.2` |
| Laptop1 | `192.168.0.3` | — | — |
| **Passerelle** | `192.168.0.254` | **Passerelle** | `192.168.100.254` |

**Étape 2 – Configuration IP des terminaux Biclou**

Attribution des adresses statiques sur PC1 et PC2 avec passerelle `192.168.100.254` (interface du routeur côté réseau `192.168.100.0`).

**Étape 3 – Vérification de la connectivité inter-sites**

Ping d'un PC Biclou vers un PC Taco :

![Configuration PC2](images/16.png) 

**Étape 4 – Configuration des ports Gigabit**

Activation des ports FastEthernet sur les switches concernés et vérification de la stabilité après modification.

![Show interface](images/7.png)

---

## 4. Troisième mission – Commerciaux itinérants + serveur DHCP chez Biclou

### Objectif

Installer un serveur DHCP pour attribuer automatiquement des adresses aux laptops des commerciaux itinérants :
- Plage d'adresses : `192.168.100.10` à `192.168.100.50`
- Configurer la passerelle par défaut
- Vérifier la connectivité jusqu'au serveur Taco

### Réalisations

**Étape 1 – Ajout du matériel**

Ajout de deux laptops et d'un serveur (Server1) sur le réseau Biclou, puis activation du service DHCP.

**Étape 2 – Configuration du serveur DHCP**

Dans `Services → DHCP` sur Server1 :

| Paramètre | Valeur |
|-----------|--------|
| Nom du pool | `serverPool` |
| Réseau | `192.168.100.0 /24` |
| IP du serveur | `192.168.100.250` |
| Adresse de début | `192.168.100.10` |
| Adresse de fin | `192.168.100.50` |
| Nombre max d'utilisateurs | `40` |
| Passerelle par défaut | `192.168.100.254` |
| Serveur DNS | Ajouté en mission 5 |

![interface DHCP configuré](images/C9.png)

**Étape 3 – Activation DHCP sur les laptops**

Les PC fixes conservent leur IP statique. Les deux nouveaux laptops sont configurés en DHCP → ils obtiennent automatiquement une adresse dans la plage `192.168.100.10 – 192.168.100.50`.

![IP Configuration laptop avec DHCP ](images/10.png)

**Étape 4 – Test de connectivité**

Ping du Laptop1 vers le serveur DHCP (`192.168.100.250`) → **réussi ✅**

![ping laptop → 192.168.100.250](images/11.png)

---

## 5. Quatrième mission – Serveur DNS + site intranet (intranet.biclou.fr)

### Objectif

Mettre en place un serveur DNS sur Server1 et héberger un site intranet accessible par nom de domaine.

### Annuaire DNS à configurer

| Nom | Type | Adresse IP |
|-----|------|------------|
| `intranet.biclou.fr` | A Record | `192.168.100.250` |
| `pc1` | A Record | `192.168.100.1` |
| `pc2` | A Record | `192.168.100.2` |
| `serveur1` | A Record | `192.168.100.250` |

### Réalisations

**Étape 1 – Configuration DNS sur Server1**

- Activation du service DNS sur Server1 (`192.168.100.250`)
- Ajout des enregistrements de type A listés ci-dessus

![Interface DNS ](images/12.png)

- Activation du service HTTP (et HTTPS) pour héberger la page intranet

![Interface HTTP](images/13.png)

**Étape 2 – Configuration DNS statique sur PC1 et PC2**

Renseignement du serveur DNS `192.168.100.250` dans la configuration IP des postes fixes.

**Étape 3 – Ajout du serveur DNS dans le pool DHCP**

Mise à jour du pool `serverPool` sur Server1 : ajout de `192.168.100.250` dans le champ DNS Server → les laptops obtiennent désormais le DNS automatiquement.

**Étape 4 – Tests de validation**

Ping par nom de domaine :

```
ping intranet.biclou.fr  →  résolution correcte ✅  (192.168.100.250)
```
![Ping intranet.biclou.fr](images/14.png)

Analyse en mode simulation : observation des échanges DNS (requête + réponse) puis ICMP.

![Mode simulation (paquets DNS puis ICMP)](images/15.png)

Accès au site intranet depuis un navigateur Packet Tracer :

```
http://intranet.biclou.fr  →  page web affichée ✅
```

![Portail BICLOU – Espace Salariés](images/17.png)

> Le portail intranet a été développé dans l'onglet `index.html` du service HTTP de Server1.

---

## 6. Conclusion – Difficultés rencontrées et compétences acquises

 
| Difficulté | Solution appliquée |
|---|---|
| Ports switch saturés (`No available port`) | Ajout d'un switch supplémentaire |
| DNS absent du pool DHCP → laptops ne résolvent pas les noms | Ajout du serveur DNS dans la configuration du pool DHCP |
| Vérification du routage inter-sites | Méthodologie ping étape par étape : local → passerelle → inter-site |

### Compétences acquises

- Extension physique d'un réseau (ajout de switch)
- Configuration des services DHCP, DNS et HTTP sous Packet Tracer
- Utilisation du mode simulation pour analyser les échanges réseau (DNS, ICMP)
- Dépannage de base : ports indisponibles, résolution de noms, connectivité inter-sites
- Développement d'une page HTML pour un portail intranet

---

