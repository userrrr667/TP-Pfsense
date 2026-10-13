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

<img width="777" height="110" alt="image" src="https://github.com/user-attachments/assets/ab492ef3-83a6-4cb2-876f-f5c780866ec6" />

<aside>

**Configuration du poste Debian DMZ** 

</aside>

**On n’attribue l’adresse IP 192.168.30.2 à la VM debian,afin quelle rentrent dans le lan :**

<img width="488" height="181" alt="image" src="https://github.com/user-attachments/assets/7281140e-f3ee-4208-b4f0-151be8d6337b" />


**Vérifier le statut du serveur Web Apache :**

<img width="739" height="92" alt="image" src="https://github.com/user-attachments/assets/75f517da-5d9b-4c73-b771-5557a578fbb6" />

**Ouvrir un navigateur et saisir http://192.168.30.2**

<img width="430" height="146" alt="image" src="https://github.com/user-attachments/assets/53d40a15-6f31-4781-a4fe-51e2b3d96f63" />


<aside>

**Autoriser le LAN à accéder au serveur Web (DMZ)**

</aside>

**On ouvre Edge et  on se connecte à pfSense : https://192.168.10.5 :**

<img width="438" height="44" alt="image" src="https://github.com/user-attachments/assets/404d566d-fe72-4def-9529-8719bc504454" />


**Dans le menu haut, va dans Firewall > Rules > onglet LAN et On clique sur le bouton Add** 

**Configurer la règle :**

<img width="810" height="279" alt="image" src="https://github.com/user-attachments/assets/23f8ff92-63ce-415a-86f5-ed4a51db9592" />


<aside>

**Publication du serveur Web sur l'interface WAN**

</aside>

**Validation finale de l'accès extérieur**

**Sur un poste physique de la salle  Ouvre un navigateur et tape l'IP WAN de pfSense (192.168.20.174) ;**

<img width="560" height="110" alt="image" src="https://github.com/user-attachments/assets/1584d969-07a5-449d-bc49-3941fff5c6e2" />


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

<img width="550" height="38" alt="image" src="https://github.com/user-attachments/assets/dfbe9554-5520-4676-9687-07bdcfedc292" />


**Recherche du paquet ntopng,on clic sur Install puis confirmation :**

<img width="427" height="62" alt="image" src="https://github.com/user-attachments/assets/aa4b5fa5-cc26-4d4e-9fdb-0ba9b4513dae" />

**Activation et paramétrage du service dans Diagnostics > ntopng Settings :**

<img width="512" height="46" alt="image" src="https://github.com/user-attachments/assets/9802986b-a55a-4736-9ff5-1806568c1afc" />


**Cocher Enable ntopng**

<img width="389" height="49" alt="image" src="https://github.com/user-attachments/assets/5bbae798-ad3d-4ee8-a500-4e4ac4c0e7f9" />

 **Sélectionner l'interface LAN**
 
<img width="292" height="49" alt="image" src="https://github.com/user-attachments/assets/38cda6a1-7aef-4ba1-8402-d2c6de1e46f1" />

**Définir un mot de passe administrateur**

<img width="282" height="58" alt="image" src="https://github.com/user-attachments/assets/f2474f24-a6ea-4781-be76-e159ea63da11" />

**Dans pfsense on va dans “Access ntopng”**

<img width="453" height="59" alt="image" src="https://github.com/user-attachments/assets/f5fe85eb-2bf0-492d-ba5c-506663622aa2" />


**Page d'authentification on s'y authentifie avec l'utilisateur par défaut admin et le mot de passe configuré dans pfSense :**

<img width="577" height="181" alt="image" src="https://github.com/user-attachments/assets/7199ef72-ccd7-499a-9113-91852f6bff95" />


**Tableau de bord  (Dashboard / vtnet1) : Confirme que le service fonctionne correctement et écoute bien sur l'interface LAN (désignée par l'interface système vtnet1 :**

!<img width="567" height="128" alt="image" src="https://github.com/user-attachments/assets/24a0289d-d55d-4588-bd5a-e4e1f98cb668" />


<aside>

**Mise en place d'un bureau distant (RDP) sécurisé en DMZ**

</aside>

**Pour éviter les attaques par force brute sur le port RDP standard (3389), on configure un NAT/PAT sur pfSense : le port externe personnalisé 33890 (WAN) est redirigé vers le port 3389 de l'IP du serveur Windows en DMZ.**

 **dans Centre Réseau et Partage > Modifier les paramètres de la carte > Propriétés d'Ethernet > Protocole Internet version 4 (TCP/IPv4) :**

<img width="497" height="170" alt="image" src="https://github.com/user-attachments/assets/f613ffea-d745-4250-8f6c-f64600e3ba58" />


**Activer le Bureau à distance (RDP) dans Ce PC (clic droit) > Propriétés (ou Paramètres > Système > Bureau à distance)**

<img width="466" height="188" alt="image" src="https://github.com/user-attachments/assets/6a005f54-6c06-4889-93a6-d875bd63e0ba" />


**Autoriser le trafic RDP dans le Pare-feu Windows**

**Va dans Panneau de configuration > Pare-feu Windows Defender > Autoriser une application ou une fonctionnalité > Coche la case Bureau à distance pour les réseaux Privés et Publics.**

<img width="966" height="34" alt="image" src="https://github.com/user-attachments/assets/eaf687cb-bebd-4b0e-91e9-05f9fcd281e1" />

**NAT/PAT sur pfSense**

<img width="1135" height="137" alt="image" src="https://github.com/user-attachments/assets/2c66148f-be16-46a9-ac7f-3acf135b55ea" />

**validation du RDP**
**Depuis le poste client externe (WAN), exécution du client RDP :**

<img width="617" height="20" alt="image" src="https://github.com/user-attachments/assets/a447b2a7-621d-40cc-8cca-0c21d6a8dba9" />

**Résultat : La session distante du serveur Windows Server en DMZ s'ouvre correctement à travers la redirection de port du pare-feu pfSense.**

<img width="1878" height="797" alt="image" src="https://github.com/user-attachments/assets/5cabec83-f4fb-4422-a485-2193b19e1dff" />





