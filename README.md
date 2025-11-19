# NetPractice

## Qu'est-ce qu'une adresse (IPV4) ?

Une adresse IP est donc une suite de 4 octets (32bits) permetant d'identifier un appareil dans un réseau privée ou publique. 
Chaque IP doit être unique dans son réseau pour pouvoir communiquer avec ses paires.

### Exemple
```
128.64.12.236
10000000 01000000 00001011 11101100
```
## L'adresse 127.0.0.1

Certaines adresses ne peuvent pas être appliqué à un periphérique leur usage est déjà réservé.

```
127.0.0.1
Localhost / loopback
```

127.0.0.1 est une adresse permetant à un appareil de communiquer avec lui même. Elle est en général utilisé pour simuler des servers sans accès externe.

Ainsi toutes les IP étant dans la plage 127.0.0.0 - 127.255.255.255 ne sont pas applicable à un périphérique.

## Masque de sous réseaux

Un masque est une autre suite de 4 octets permettant de définir une plage réseau, c'est à dire le nombre de périphérique possible dans un même sous réseau.
Le masque se divise en deux parties. La partie réseau et la partie hôte.

Pour comprendre cette partie il faudra se pencher sur le binaire.
La partie réseau sera constitué de 1 tandis que la partie hôte de 0.

### Exemple
```
255.255.255.0
11111111 11111111 11111111 00000000
```
Ici les trois premiers octets sont attribués au réseau.
Tandis que les bits à 0 représentent la plage c'est a dire le nombre de peripheriques possibe pour ce meme reseau. 

A savoir que la 1er adresse d'un reseau ( la ou tous les bits hotes sont a 0) est reservé pour representé le reseau en lui meme.
Aussi, la derniere adresse ( ou tous les bits sont a 1) est reservé au pour communiquer avec tous les peripheriques du reseau (broadcast).

### Exemple
```
192.12.102.0 # adresse du reseau 
255.255.255.0 # masque 

192.12.102.255 # broadcast
255.255.255.0   # masque
```

### Appliquer le masque à une IP


