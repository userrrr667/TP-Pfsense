# TP-Pfsense
# **TP PfSense / DMZ**

<aside>

**Projet réseau — Serveur Web en DMZ**

Étude de la mise en place d’un serveur Web sécurisé avec PfSense.


## Mise en situation

La société **Vikor** souhaite installer un serveur Web au sein de son architecture réseau.

L’objectif est de rendre ce serveur accessible :

- depuis le **LAN de l’entreprise** ;
- depuis **Internet**, via l’extérieur.

---

### Objectif

Mettre en place une architecture réseau permettant l’accès au serveur Web tout en limitant l’exposition du réseau interne.

### Accès

- Réseau local
- Internet
- Flux contrôlés

### Solution

**PfSense** est retenu pour sa flexibilité et ses nombreuses fonctionnalités de sécurité réseau.

## Enjeu de sécurité

Afin de sécuriser au maximum le système d’information, le serveur Web doit être isolé du réseau interne tout en restant accessible aux utilisateurs autorisés.

> La DMZ constitue une zone intermédiaire dédiée aux services exposés, comme un serveur Web, afin de réduire les risques pour le LAN de l’entreprise.
> 

<aside>

**Principe retenu :** contrôler les flux entre Internet, la DMZ et le LAN à l’aide de règles de pare-feu adaptées.

</aside>

<img width="369" height="409" alt="image" src="https://github.com/user-attachments/assets/1033a66e-8b7d-47bc-8914-c930c6c64110" />


<aside>

**Installer PfSense sur une VM dotée de 3 interfaces : LAN,DMZ,Internet**

**Créer une VM pfSense avec 3 cartes réseau (WAN en Accès ponted/Accès physique, LAN et DMZ en Réseau interne :**

</aside>

<img width="819" height="60" alt="image" src="https://github.com/user-attachments/assets/2cdb6eff-9e55-435b-ac84-44859f6d2b26" />

<aside>

**Attribuer les 3 interfaces réseau à la VM pfSense :**

</aside>

<img width="551" height="78" alt="image" src="https://github.com/user-attachments/assets/5740c9a6-2cec-4799-a08c-5d1c4f825234" />

<aside>

**On demarre notre pfsens et on n’arrive sur l’interface:**

</aside>
<img width="650" height="280" alt="image" src="https://github.com/user-attachments/assets/8a079f97-4501-4d25-ac29-34f4cc9ae04d" />


<aside>

**Étape 1 : Assigner les interfaces :**

</aside>

<img width="524" height="117" alt="image" src="https://github.com/user-attachments/assets/af2a93e0-bd8b-4914-ae84-e3d989821e4c" />


<aside>

**Définir l'adresse IP du LAN :**

</aside>

<img width="747" height="71" alt="image" src="https://github.com/user-attachments/assets/97c62b48-70f7-46fc-a92f-36fe7a3ca1cb" />

<aside>

**Configurer l'adresse IP de la DMZ :**

</aside>

<img width="749" height="76" alt="image" src="https://github.com/user-attachments/assets/0756cf7a-9329-4908-9d63-5307737c62d7" />


On vérifie que tout est bon :

<img width="697" height="84" alt="image" src="https://github.com/user-attachments/assets/40ddfea4-605e-4f28-84ab-d3fc1f137c4e" />

<aside>

**Configuration du poste Client Windows 11** 

</aside>

**On n’attribue l’adresse IP 192.168.10.10 à la VM Windows 11,afin quelle rentrent dans le lan  :**


<img width="437" height="220" alt="image" src="https://github.com/user-attachments/assets/6e3218df-02b5-48f1-8a65-5eff3921cc54" />



**Se connecter à l'interface Web pfSense :**

- **Ouvrir un navigateur et saisir https://192.168.10.5**



<aside>

**Configuration du poste Debian DMZ** 

</aside>

**On n’attribue l’adresse IP 192.168.30.2 à la VM debian,afin quelle rentrent dans le lan :**

!image.png

**Vérifier le statut du serveur Web Apache :**

!image.png

**Ouvrir un navigateur et saisir http://192.168.30.2**

!image.png

<aside>

**Autoriser le LAN à accéder au serveur Web (DMZ)**

</aside>

**On ouvre Edge et  on se connecte à pfSense : https://192.168.10.5 :**

!image.png

**Dans le menu haut, va dans Firewall > Rules > onglet LAN et On clique sur le bouton Add** 

**Configurer la règle :**

!image.png

<aside>

**Publication du serveur Web sur l'interface WAN**

</aside>

**Validation finale de l'accès extérieur**

**Sur un poste physique de la salle  Ouvre un navigateur et tape l'IP WAN de pfSense (192.168.20.174) ;**

!image.png

**Accès interne LAN -> DMZ fonctionnel via http://192.168.30.2]**

**Accès externe WAN -> DMZ fonctionnel via http://192.168.20.174 grâce à la règle de NAT Port Forwarding**

# **TP PfSense / Addon**

<aside>

**Situation 1 : Monitoring du réseau LAN sous pfSense**

**Étape 1 : Choix et argumentation de la solution**

S**olution retenue : Installation du paquet ntopng via le gestionnaire de paquets de pfSense.**

- **Pourquoi ce choix : ntopng s'intègre directement à l'interface Web de pfSense et permet d'analyser le trafic réseau en temps réel avec des graphiques clairs (bande passante, protocoles, hôtes actifs).**
- **Alternatives : Bandwidthd (moins visuel) ou un serveur externe de supervision SNMP/Zabbix (plus lourd à déployer).**
</aside>

<aside>

**Déploiement et validation :**

</aside>

**Connexion à l'interface de gestion de pfSense depuis la VM Windows 11 du LAN https://192.168.10.5**

**System > Package Manager > Available Packages :**

!image.png

**Recherche du paquet ntopng,on clic sur Install puis confirmation :**

!image.png

**Activation et paramétrage du service dans Diagnostics > ntopng Settings :**

!image.png

**Cocher Enable ntopng**

!image.png

 **Sélectionner l'interface LAN**

!image.png

**Définir un mot de passe administrateur**

!image.png

**Dans pfsense on va dans “Access ntopng”**

!image.png

**Page d'authentification on s'y authentifie avec l'utilisateur par défaut admin et le mot de passe configuré dans pfSense :**

!image.png

**Tableau de bord  (Dashboard / vtnet1) : Confirme que le service fonctionne correctement et écoute bien sur l'interface LAN (désignée par l'interface système vtnet1 :**

!image.png

<aside>

**Mise en place d'un bureau distant (RDP) sécurisé en DMZ**

</aside>

**Pour éviter les attaques par force brute sur le port RDP standard (3389), on configure un NAT/PAT sur pfSense : le port externe personnalisé 33890 (WAN) est redirigé vers le port 3389 de l'IP du serveur Windows en DMZ.**

 **dans Centre Réseau et Partage > Modifier les paramètres de la carte > Propriétés d'Ethernet > Protocole Internet version 4 (TCP/IPv4) :**

!image.png

**Activer le Bureau à distance (RDP) dans Ce PC (clic droit) > Propriétés (ou Paramètres > Système > Bureau à distance)**

!image.png

**Autoriser le trafic RDP dans le Pare-feu Windows**

**Va dans Panneau de configuration > Pare-feu Windows Defender > Autoriser une application ou une fonctionnalité > Coche la case Bureau à distance pour les réseaux Privés et Publics.**
