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

## Les Switchs
Avant de comprendre l’intérêt d’un switch, il faut savoir comment les réseaux fonctionnaient auparavant. Pour connecter plusieurs périphériques entre eux, nous utilisions des hubs. Les hubs se contentent de **partager les paquets reçus d’un hôte à tous les autres hôtes connectés**. Cela pouvait provoquer des **collisions de paquets** (par exemple, si deux hôtes souhaitaient communiquer avec une même destination), ce qui pouvait entraîner des **pertes de données**.

Un switch, quant à lui, est un périphérique permettant également de connecter plusieurs postes ou serveurs dans un même réseau.
Il permet d’éviter les collisions de paquets en **mémorisant quel périphérique est connecté à quel port**. Cela évite d’envoyer des paquets vers des ports inutiles et réduit les risques de collisions.
Si deux hôtes cherchent à communiquer avec une même destination, le switch utilise une **table de correspondance** pour gérer l’envoi des données sans risque.

Un switch permet donc de connecter plusieurs hôtes d'un **même réseau**.

<img width="593" height="285" alt="nsi_prem_introReseau_4-3919421320" src="https://github.com/user-attachments/assets/30d93616-e416-47e5-b43a-a76af57d2346" />

## routeur
