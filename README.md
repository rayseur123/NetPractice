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

Un masque est une autre suite de 4 octets permétant de définir une plage réseau, c'est à dire le nombre de périphérique possible dans un même sous réseau.
Le masque se divise en deux parties. La partie réseau et la partie hôte.

Pour comprendre cette partie il faudra se pencher sur le binaire.
La partie réseau sera constitué de 1 tandis que la partie hôte de 0.

### Exemple
```
255.255.255.0
11111111 11111111 11111111 00000000
```
Ici les trois premiers octet son attribué au reseau.

### Appliquer le masque a une IP

