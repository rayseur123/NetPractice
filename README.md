# NetPractice
## Un réseau ?
Ici, nous parlons de réseaux TCP/IP, c’est-à-dire un ensemble de périphériques et serveurs interconnectés.

## Un sous-réseau ?
Un **sous-réseau** est une partie plus interne d’un réseau plus global.
Il permet notamment de **restreindre la portée de communication de certains périphériques** et de **protéger certains secteurs d’un réseau global.**
Internet, par exemple, est un énorme réseau composé d’un très grand ensemble de sous-réseaux.

Un sous-réseau peut lui aussi contenir plusieurs sous-réseaux.
Ceci est rendu possible grâce au **masque de sous-réseau.**

## Qu'est-ce qu'une adresse IP (IPV4) ?
Une adresse IP est une suite de 4 octets (32 bits) permettant d’identifier un appareil dans un réseau privé ou public.
Chaque IP doit être unique dans son réseau pour pouvoir communiquer avec ses pairs.

### Exemple
```
128.64.12.236
10000000 01000000 00001011 11101100
```
## L'adresse 127.0.0.1
Certaines adresses ne peuvent pas être attribuées à un périphérique, car leur usage est déjà réservé.

```
127.0.0.1
Localhost / loopback
```
127.0.0.1 est une adresse permettant à un appareil de communiquer avec lui-même. Elle est généralement utilisée pour simuler des serveurs sans accès externe.

Ainsi, toutes les IP de la plage 127.0.0.0 à 127.255.255.255 ne sont pas attribuables à un périphérique.

## Masque de sous réseaux
Un masque est une suite de 4 octets permettant de définir une plage réseau, c’est-à-dire le nombre de périphériques possibles dans un même sous-réseau.
Le masque se divise en deux parties : la partie réseau et la partie hôte.

Pour comprendre cela, il faut se pencher sur le binaire :
Pour comprendre cette partie il faudra se pencher sur le binaire.

- **la partie réseau** → bits à 1
- **la partie hôte** → bits à 0

### Exemple
```
255.255.255.0
11111111 11111111 11111111 00000000
```
Ici, les trois premiers octets sont attribués au réseau, tandis que les bits à 0 représentent la plage, c’est-à-dire le nombre de périphériques possibles dans ce même réseau.

À retenir :

- La **première adresse** (tous les bits hôtes à 0) représente le réseau.
- La **dernière adresse** (tous les bits hôtes à 1) est l’adresse de broadcast.

Le **broadcast** permet de communiquer avec tous les périphérique du réseau.

### Exemple
```
192.12.102.0 # adresse du reseau 
255.255.255.0 # masque 

192.12.102.255 # broadcast
255.255.255.0   # masque
```
Si un périphérique doit être inséré dans ce réseau, son IP devra commencer par 192.12.102, et le dernier octet devra être compris entre 1 et 254 inclus.
C’est ça, la plage !

Dans un même réseau, les IP doivent appartenir à une même plage pour communiquer. Il est simple de connaître la taille d’une plage lorsqu’un ou plusieurs octets complets sont attribués à la partie hôte.
Mais les choses se corsent lorsque la partie hôte commence au milieu d’un octet.
```
192.12.102.160 # adresse du réseau
255.255.255.224 # masque
```
Ici, la plage paraît complexe à déterminer. Nous pourrions être tentés de nous pencher directement sur le binaire, mais une solution plus simple existe !

Il suffit de soustraire l’octet du masque (224) à 256 (le nombre de valeurs possibles dans un octet), ce qui nous donne 32.
```
256 - 224 = 32 # taille de la plage
```
Maintenant, il faut trouver à quelle plage appartient 161. Pour ce faire, il suffit de soustraire la taille de la plage à 256 jusqu’à trouver la plage correspondante.
```
256 - 32 = 224
224 - 32 = 192
192 - 32 = 160 # Voici la plage de notre réseau !
```
La plage de notre réseau est donc entre 160 à 191 inclu ! Il ne reste plus qu'a retirer l'adresse du reseau et celle du broadcast.

Les adresses utilisables sont donc :
- 192.12.102.160 à 192.12.102.191 inclus

## Les Switchs (commutateur)
Avant de comprendre l’intérêt d’un switch, il faut savoir comment les réseaux fonctionnaient auparavant. Pour connecter plusieurs périphériques entre eux, nous utilisions des hubs. Les hubs se contentent de **partager les paquets reçus d’un hôte à tous les autres hôtes connectés**. Cela pouvait provoquer des **collisions de paquets** (par exemple, si deux hôtes souhaitaient communiquer avec une même destination), ce qui pouvait entraîner des **pertes de données**.

Un switch, quant à lui, est un périphérique permettant également de connecter plusieurs postes ou serveurs dans un même réseau.
Il permet d’éviter les collisions de paquets en **mémorisant quel périphérique est connecté à quel port**. Cela évite d’envoyer des paquets vers des ports inutiles et réduit les risques de collisions.
Si deux hôtes cherchent à communiquer avec une même destination, le switch utilise une **table de correspondance** pour gérer l’envoi des données sans risque. 

Un switch retiens les adresses MAC connecté à ses ports et les utilisent pour savoir précisement quel est la source et quel est la destination. 

Un switch permet donc de connecter plusieurs hôtes d'un **même** réseau. 

<img width="593" height="285" alt="nsi_prem_introReseau_4-3919421320" src="https://github.com/user-attachments/assets/30d93616-e416-47e5-b43a-a76af57d2346" />

## Routeur
Un routeur est un périphérique permettant la connexion entre plusieurs réseaux. Il se charge de transferer les packets en calculant le trajet optimal.
Comme pour le switch, le router envoi les packets directement àla bonne cible.

Le routeur est identifiable pour chacun de ses reseaux par une IP propre à chaque reseaux. Il se charge ensuite de les faire communiquer entre eux. Attention au chevauchement d'IP, chaque réseaux d'un routeur doit etre different et les plages des un de doivent pas obsorber celle des autres.

### Exemple avec chevauchement
```
# Réseau A
192.168.1.0
255.255.0.0       (/16)

# Réseau B
192.168.2.0
255.255.255.0     (/24)
```
- Le réseau /16 couvre **192.168.0.0** → **192.168.255.255**
- Le réseau /24 couvre **192.168.2.0** → **192.168.2.255**

Le petit réseau (/24) est complètement inclus dans le grand réseau (/16). On dit qu’il y a **chevauchement**, car les deux plages d’adresses se recouvrent. Le routeur ne peut donc pas différencier ces deux réseaux correctement.

## Les tables de routages
Une table de routage est une structure de données permettant d’indiquer comment les paquets doivent être acheminés en fonction de leur destination. Pour cela, la table de routage contient les éléments suivants:

- **Destination** : L’adresse IP représentant le réseau de destination.
- **Masque** : Le masque de sous-réseau permettant de préciser la portée du paquet dans ce réseau.
- **Passerelle** : Le routeur passerelle nécessaire pour atteindre ce réseau.
- **Interface** : L’interface réseau par laquelle le paquet sera envoyé.

<img width="900" height="581" alt="Screenshot from 2025-11-25 12-26-41" src="https://github.com/user-attachments/assets/f2ec525d-686e-46f8-adcd-32f238452638" />

// expliquer comment choisir quel chemin emprimter

### Types de tables de routages
#### Table de routage statique
Une table de routage statique est configurée manuellement et n’adapte pas ses routes en fonction des changements du réseau. Elle est pratique dans les petits réseaux et permet d’acheminer simplement et efficacement les paquets vers leur destination.

#### Table de routage dynamique
Une table de routage dynamique s’adapte automatiquement grâce à un paramètre supplémentaire :

- **Metric** : Une valeur représentant le coût d’acheminement jusqu’à la destination, permettant de choisir la route la plus adaptée selon l’état du trafic.

Ce type de table offre des avantages en termes de performance, mais n’est réellement utile que dans des infrastructures réseau de grande taille, en plus d’être plus complexe à mettre en place et à maintenir.

*Pour savoir comment les routeurs trouvent la route optimal : [RIP](https://fr.wikipedia.org/wiki/Routing_Information_Protocol), [OSPF](https://en.wikipedia.org/wiki/Open_Shortest_Path_First), [BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)*

# Informations bonus 
## Adresse MAC ? Pourquoi faire ?
L'adresse MAC (adresse physique) est l'adresse réel de votre périphérique. La ou une IP est purement logique, une adresse MAC est associé au composant même. Elle est nécessaire pour la comunication physique de vos composants. Un cable ethernet par exemple, ne connais que l'adresse MAC. L'ip n'est donc qu'une surcouche permettant d'identifier un périphérique dans un réseau.

## Les ports réseaux
Lorsqu’un ordinateur reçoit des données sur le réseau, il doit savoir quel processus ou application doit traiter ces données.
- L’**adresse IP** identifie un ordinateur ou périphérique sur le réseau
- Le **port** identifie un processus ou une application sur cet ordinateur.

Ainsi, la combinaison **IP + port** permet de diriger correctement chaque paquet vers le bon destinataire sur le réseau.

### Ports éphémères
Lorsqu’un processus a besoin d’envoyer des données, il utilise souvent un **port temporaire** (appelé **port éphémère**) qui lui est attribué automatiquement par le système. 
Ce port, combiné à l’adresse IP de l’ordinateur, permet de gérer correctement la communication entre les différents processus et périphériques sur le réseau.

### Exemple
```
192.126.32.72::4365 # 4365 étant le port
```
### Ports réservés
Certains ports sont **réservés ou standardisés** (ex. 80 pour HTTP, 443 pour HTTPS, 25 pour SMTP).

- faciliter la communication entre les applications.
- faciliter la gestion de la sécurité, car les ports réservés étant très courants, les standardiser permet de mieux contrôler et configurer leur utilisation.

## LAN (Local Area Network)
Un réseau LAN désigne simplement un réseau local. C’est-à-dire un ensemble de périphériques qui communiquent entre eux sans utiliser Internet comme intermédiaire, par exemple le réseau d’un campus ou d’un foyer.

## ARP (Address Resolution Protocol)
L’ARP est un protocole permettant la communication entre plusieurs périphériques sur un même réseau local. Il ne s’applique donc qu’à l’échelle du réseau local.

Pour communiquer dans un réseau, un périphérique doit connaître l’adresse MAC (adresse physique) du périphérique cible. Pour obtenir cette adresse, l’ordinateur source envoie une requête ARP à tous les périphériques (en utilisant le broadcast !) du réseau, demandant : « À qui appartient l’IP de destination ? »

Le périphérique correspondant répond alors en renvoyant sa MAC, ce qui permet à l’ordinateur source d’envoyer correctement les paquets.

*Pour en savoir plus : [ARP](https://www.geeksforgeeks.org/ethical-hacking/how-address-resolution-protocol-arp-works/).*

## NAT (Network address translation)
Avec toutes ces explications, un problème se pose : comment les périphériques d’un réseau peuvent-ils communiquer avec Internet alors qu’ils partagent tous le même routeur et donc la même **IP publique** ? C’est ici que le protocole **NAT** intervient.

Lorsque votre ordinateur envoie des données sur Internet, il utilise **son IP** privée et un **port source**. Ces données passent par le routeur, qui :

- Remplace votre **IP privée** par son **IP publique**.
- Attribue un **port temporaire** pour remplacer celui de votre machine, afin de pouvoir différencier plusieurs connexions simultanées.

Le routeur garde une **table de correspondance NAT** associant chaque port interne à son port externe. Cela lui permet de **rediriger correctement les réponses entrantes vers le bon périphérique du réseau local**.

# Source

