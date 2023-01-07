# TP-Codeurs-Entropiques
L’objectif de ce TP est de parcourir les différentes étapes du codage et du décodageRLE, Arithmetique, HUFFMANN, et LZW
# Run Length Encoding
Pour ce faite nous n’utiliserons pas les règles classiques données auparavant pour codage RLE, nous allons tous simplement compter le nombre de ‘0’ , puis des ‘1’, puis des ‘0’, puis des ‘1’ et ainsi de suite jusqu’à la fins de l’image.
L’image doit être tout d’abords réécrite en un vecteur ligne (ou colonne) de longueur le nombre total d’éléments de l’image soit NXM (N et M étant les dimensions de l’image). 
On commence par chercher le nombre des ‘0 au début du vecteur.
le vecteur ne contient que des ‘0’, dans ce cas, pas de ‘1’ trouvés et nous aurons atteint la fin du vecteur. Le résultats du codage sera la longueur du vecteur ; soit L On trouve un ‘1’, on détermine sa position P, le nombre de ‘0’ avant cette position est donné par P-1. Ensuite on va chercher le nombre de ‘1’ qui suivent par le même procédé puis le nombre de ‘0’ et ainsi de suite jusqu’à la fin du vecteur.


```matlab
function arr=rle(I)
image=imread('lena.tif');
image=imbinarize(image);
L=prod(size(image));
im=reshape(image',1,L);
x=1;
arr=[];
while L ~= 0,
temp=min(find(im==x));
if isempty(temp),
arr=[arr L];
break
end;
arr=[arr temp-1];
x=abs(1-x);
im=im(temp:L);
L=L-temp+1;
end;

arr=uint16(arr)
image=uint16(image)
whos image arr
```
on commence par lire notre image en utilisant la fonction **imread()** 
```matlab
image=imread('lena.tif');
```
afin de creer une image binaire a partir de notre image **I** on utilise la fonction imbinarize
> tout d'abord on doit installer le **image processing toolbox**
```matlab
image=imbinarize(image);
```
on détermine la taille de l'image à l'aide de la fonction **size()**, et on utilise le produit **prod()** pour trouver la longueur des données d'image
```matlab
L=prod(size(image));
```
pour transformer les donnees en un seul vecteur on utilise la fonction **reshape()**
```matlab
im=reshape(image',1,L);
```
le while va s executer jusqu'à ce que la longueur des données d'image soit nulle
- La fonction min trouve l'indice de la premiere occurrence de **0** ou de **1** dans le vecteur im . Si aucune valeur n'est trouvée, la boucle est interrompue
- L'indice de la premiere occurrence est stocke dans la variable arr
- la variable **x** change de valeur **0** ou **1**
- apres on mise a jour le vecteur **im** est sa longueur
```matlab
while L ~= 0,
temp=min(find(im==x));
if isempty(temp),
arr=[arr L];
break
end;
arr=[arr temp-1];
x=abs(1-x);
im=im(temp:L);
L=L-temp+1;
end;
```
on affiche les informations de l image avant et apres compression
```matlab
arr=uint16(arr)
image=logical(image)
whos image arr
```
## resultats 
on utilise la commande suivante 
```matlab
rle('lena.tif')
```
```matlab
  Name         Size              Bytes  Class      Attributes

  arr          1x4401             8802  uint16               
  image      256x256             65536  logical   
```
les resultats de la compression de l'image **'lena.tif'** en utilisant le codage **RLE** indiquent la taille de l image avant et apres compression , l image avant compression est une image de **256x256** pixels, qui occupe **65536** octets en mémoire. apres avoir utiliser le codage rle la variable **arr** a une taille de **1x4401**, ce qui signifie qu'elle occupe 8802 octets en mémoire
# Codage ARITHMETIQUE et d'HUFFMAN

<div align="center"><strong>f=[ 1 1 2 1 ; 3 2 4 2 ]</strong></div>
<div align="center"><strong>f=[ f ; 1 1 2 1 ; 3 2 4 2 ]</strong></div>

## entropy
on commence par calculer manuellement l entropie de cette matrice
on trouve que :
- probabilite que **x = 1** est **p1 = 0,375**
- probabilite que **x = 2** est **p2 = 0,375**
- probabilite que **x = 3** est **p3 = 0,125**
- probabilite que **x = 4** est **p4 = 0,125** 
- l entropie est h = 1,8112
On applique lafonction **entropy** a cette matrice
```matlab
>>f=[ 1 1 2 1 ; 3 2 4 2 ]
>>f=[ f ; 1 1 2 1 ; 3 2 4 2 ]
>>[h p]=entropy(f,4)
```
resultas :
```matlab
>> f=[ 1 1 2 1 ; 3 2 4 2 ]

f =

     1     1     2     1
     3     2     4     2

>> f=[ f ; 1 1 2 1 ; 3 2 4 2 ]

f =

     1     1     2     1
     3     2     4     2
     1     1     2     1
     3     2     4     2

>> [h p]=entropy(f,4)

p =

    0.3750    0.3750    0.1250    0.1250


h =

    1.8113


p =

    0.3750    0.3750    0.1250    0.1250
```

<div align="center"><strong>on remarque que ce sont les memes resultats qu'on a obtenu avec un calcul manuel</strong></div>

## Huffman
on applique la fonction de **huffmanim()** a cette matrice pour avoir les codes et la longueur moyenne
```matlab
>> [c l] = huffmanim(hist(f(:),4))
```
resultats :
```matlab
c =

  4×1 cell array

    {'01' }
    {'1'  }
    {'000'}
    {'001'}


l =

    1.8750
```
<div align="center"><strong>c represente les codes et l represente la longueur moyenne</strong></div>

<div align="center"><strong>en utilisant un codage simple on aurait besoin de 2 bits pour coder chaque elements de notre matrice mais avec le codage huffman nous avant pu deminuer cette longueur a 1,875 </strong></div>
