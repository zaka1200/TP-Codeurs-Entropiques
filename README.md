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

```matlab
fc  =
    '1'
    '000'
    '1'
    '000'
    '1'
    '01'
    '1'
    '01'
    '1'
    '1'
    '1'
    '1'
    '01'
    '001'
    '01'
    '001'
>> h2f2 = char(fc)
h2f2 =

1
000
1
000
1
01
1
01
1
1
1
1
01
001
01
001
```

## Application a une image

maintenant on va appliquer les memes fonction et refaire le meme travail que pour la matrice f

on commence par le calcul d entropy en utilisant la commande suivante :
> on lit d'abord l'image **'lena.tif'** **(x=imread('lena.tif'))** 
```matlab
[h p]=entropy(x,255)
```
resultas : 

```matlab
h =

    7.5791


p =

  Columns 1 through 10

    0.0001    0.0000    0.0001    0.0003    0.0005    0.0005    0.0008    0.0009    0.0013    0.0016

  Columns 11 through 20

    0.0020    0.0025    0.0030    0.0035    0.0044    0.0052    0.0053    0.0072    0.0071    0.0079

  Columns 21 through 30

    0.0085    0.0085    0.0090    0.0083    0.0077    0.0084    0.0073    0.0070    0.0068    0.0061

  Columns 31 through 40

    0.0051    0.0056    0.0047    0.0038    0.0043    0.0041    0.0039    0.0041    0.0045    0.0039

  Columns 41 through 50

    0.0040    0.0039    0.0038    0.0045    0.0044    0.0042    0.0037    0.0046    0.0037    0.0041

  Columns 51 through 60

    0.0039    0.0037    0.0038    0.0041    0.0038    0.0043    0.0045    0.0042    0.0052    0.0051

  Columns 61 through 70

    0.0065    0.0069    0.0071    0.0086    0.0082    0.0084    0.0076    0.0073    0.0075    0.0069

  Columns 71 through 80

    0.0068    0.0058    0.0060    0.0060    0.0056    0.0056    0.0063    0.0054    0.0059    0.0052

  Columns 81 through 90

    0.0053    0.0054    0.0058    0.0058    0.0059    0.0059    0.0060    0.0060    0.0069    0.0072

  Columns 91 through 100

    0.0077    0.0082    0.0081    0.0085    0.0080    0.0079    0.0076    0.0079    0.0070    0.0069

  Columns 101 through 110

    0.0064    0.0062    0.0068    0.0065    0.0058    0.0058    0.0054    0.0059    0.0055    0.0060

  Columns 111 through 120

    0.0063    0.0070    0.0062    0.0066    0.0063    0.0069    0.0060    0.0057    0.0062    0.0061

  Columns 121 through 130

    0.0056    0.0066    0.0064    0.0072    0.0073    0.0071    0.0076    0.0072    0.0073    0.0075

  Columns 131 through 140

    0.0074    0.0073    0.0077    0.0081    0.0083    0.0082    0.0073    0.0064    0.0063    0.0058

  Columns 141 through 150

    0.0050    0.0045    0.0046    0.0043    0.0041    0.0040    0.0036    0.0040    0.0038    0.0041

  Columns 151 through 160

    0.0036    0.0031    0.0034    0.0033    0.0033    0.0032    0.0031    0.0030    0.0028    0.0028

  Columns 161 through 170

    0.0023    0.0023    0.0023    0.0018    0.0018    0.0017    0.0020    0.0021    0.0017    0.0018

  Columns 171 through 180

    0.0019    0.0017    0.0018    0.0022    0.0018    0.0020    0.0020    0.0023    0.0025    0.0024

  Columns 181 through 190

    0.0026    0.0029    0.0029    0.0030    0.0029    0.0033    0.0027    0.0025    0.0023    0.0025

  Columns 191 through 200

    0.0025    0.0025    0.0027    0.0029    0.0027    0.0028    0.0024    0.0032    0.0032    0.0030

  Columns 201 through 210

    0.0030    0.0032    0.0034    0.0031    0.0027    0.0026    0.0024    0.0017    0.0016    0.0014

  Columns 211 through 220

    0.0013    0.0011    0.0010    0.0008    0.0006    0.0006    0.0006    0.0004    0.0004    0.0003

  Columns 221 through 230

    0.0003    0.0002    0.0002    0.0002    0.0002    0.0000    0.0001    0.0001    0.0000    0.0000

  Columns 231 through 240

    0.0000    0.0000    0.0000    0.0000         0         0         0         0         0         0

  Columns 241 through 250

         0         0         0    0.0000         0         0         0         0         0         0

  Columns 251 through 255

         0         0         0         0    0.0000

```

<div align="center"><strong>l'entropie est 7,5791</strong></div>
## huffmanim
on applique la fonction **huffmanim()** , on obtient les resultats suivants :

```matlab
l =

    7.6056
```
la longueur moyenne nous montre qu on a besoin de 7,6055 bits pour coder un elemnent de cette image en utilisant le codage dea **Huffman**

## Application a un texte

on commence par creer un fichie texte **seq1.txt**
```matlab
AAABBSSSASRRRZZZZBBA
```
le code suivant lira le fichier , la variable arr contiert notre texte

```matlab
file1=fopen('seq.txt','r');
arr=fread(file1,'*char');
fclose(file1);
arr=reshape(arr,1,length(arr));
```
maintenant an va creer la fonction qui calcule les frequences des lettres de la sequence .
```matlab
function calculate_frequency(file_in, file_out)
    text = fileread(file_in);
    frequency = zeros(1,256);
    for i = 1:length(text)
        frequency(uint8(text(i))+1) = frequency(uint8(text(i))+1) + 1;
    end
    frequency = frequency / sum(frequency);
    fileID = fopen(file_out,'w');
    for i = 1:256
        if(frequency(i)~=0)
            fprintf(fileID,'%s %f\n',char(i-1),frequency(i));
        end
    end
    fclose(fileID);
end
```
- lire le fichier text
```matlab
text = fileread(file_in);
```
- Initialiser le tableau de fréquences
```matlab
frequency = zeros(1,256);
```
- Incrémenter l'élément correspondant dans le tableau de fréquence
```matlab
 frequency(uint8(text(i))+1) = frequency(uint8(text(i))+1) + 1;
```
- Normaliser le tableau de fréquences
```matlab
frequency = frequency / sum(frequency);
```
- Parcourez chaque élément du tableau de fréquences et écrivez le caractère et sa fréquence dans le fichier de sortie
```matlab
for i = 1:256
        if(frequency(i)~=0)
            fprintf(fileID,'%s %f\n',char(i-1),frequency(i));
        end
    end
```
resultats :
```txt
A 0.250000
B 0.200000
R 0.150000
S 0.200000
Z 0.200000
```
on applique la focntion **imhist** a la sequence numerique
```matlab
imhist(uint8('seq.txt'))
```
resultat :
![untitled](https://user-images.githubusercontent.com/121964432/212541677-26510ef3-1d7b-484c-a3c3-0e3b4b40ce75.jpg)
on applique la fonction huff sur le fichier text seq 
> d'abord on doit ajouter cette ligne de code don le fichier **huff.m** 
```matlab
[long,lar]=size(seq)
```
```matlab
decseq=huffdecode(huf,encseq)
   [long,lar]=size(seq)
  gf=[];
  for ii=1:long
   gf=[gf decseq(ii:ii+lar-1)];
  end
   disp(' Séquence Décodée :');
     disp(str2num(gf))
     disp(decseq);
```
Cette fonction prend deux arguments d'entrée : un nom de fichier pour le fichier texte d'entrée (par exemple, "exemple.txt") et un nom de fichier pour le fichier de fréquence de sortie (par exemple, "fréquences.txt").
Il lit le fichier texte d'entrée en utilisant la fonction fileread(), puis boucle sur chaque caractère du texte et compte la fréquence de chaque caractère en utilisant la même méthode que précédemment.
Ensuite, elle ouvre le fichier de sortie à l'aide de la fonction fopen(), en spécifiant qu'il doit être ouvert en écriture.
Ensuite, il boucle sur chaque élément du tableau de fréquence et écrit le caractère et sa fréquence dans le fichier de sortie en utilisant la fonction fprintf().
Enfin, il ferme le fichier de sortie à l'aide de la fonction fclose().

Ainsi, la fréquence de chaque caractère du fichier d'entrée sera enregistrée dans le fichier de sortie sous forme de tableau, chaque ligne contenant un caractère et sa fréquence correspondante.

resultats de la fonction huff sur notre fichier text
```matlab
huff = 

  1×5 struct array with fields:

    sym
    prob
    code

    {'A :0.25 :10'}

    {'B :0.2 :00'}

    {'S :0.2 :01'}

    {'R :0.15 :111'}

    {'Z :0.2 :110'}
```

<div align="center"><strong>le codage huffman des 5 caracteres</strong></div>

```matlab
Entropie =2.3037
```

<div align="center"><strong>l entropie est egale a 2.3037</strong></div>


```matlab
Longmoyen =2.35
```

<div align="center"><strong>la longueur moyenne est egale a 2,35</strong></div>

```matlab
Séquence  de départ:
AAABBSSSASRRRZZZZBBA
Séquence codée :
10101000000101011001111111111110110110110000010
```

<div align="center"><strong>la sequence avant et apres codage</strong></div>
