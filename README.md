# TP-Codeurs-Entropiques
# Run Length Encoding

```matlab

```
on commence par lire notre image en utilisonsla fonction **imread** 
```matlab

```
afin de creer une image binaire a partir de notre image **I** on utilise la fonction imbinarize
> tout d'abord on doit installer le **image processing toolbox**
```matlab

```
on détermine la taille de l'image à l'aide de la fonction **size()**, et on utilise le produit **prod()** pour trouver la longueur des données d'image
```matlab

```
pour transformer les donnees en un seul vecteur on utilise la fonction **reshape()**
```matlab

```
le while va s executer jusqu'à ce que la longueur des données d'image soit nulle
- La fonction min trouve l'indice de la premiere occurrence de 0 ou de 1 dans le vecteur im . Si aucune valeur n'est trouvée, la boucle est interrompue
- L'indice de la premiere occurrence est stocke dans la variable arr
- la variable x change de valeur 0 ou 1
- apres on mise a jour le vecteur **im** est sa longueur
```matlab

```
on affiche les informations de l image avant et apres compression
```matlab

```
## resultats 
les resultats de la compression de l'image 'lena.tif' en utilisant le codage RLE indiquent la taille de l image avant et apres compression , l image avant compression est une image de 256x256 pixels, qui occupe 65536 octets en mémoire. apres avoir utiliser le codage rle la variable **arr** a une taille de 1x4401, ce qui signifie qu'elle occupe 8802 octets en mémoire
- **Le taux de compression = 65536/8802 = 7.4456** soit **744.56 %**.
