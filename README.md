# TP-Codeurs-Entropiques
# Run Length Encoding

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
on commence par lire notre image en utilisonsla fonction **imread** 
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
- **Le taux de compression = 65536/8802 = 7.4456** soit **744.56 %**
