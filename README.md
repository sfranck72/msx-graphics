# msx-graphics
***Source de code en BASIC MSX pour gérer les images***

**Méthode:**  
1) Redimensionner l'image en 256x212.  
2) BMP2MSX pour limiter la palette à 16 couleurs, BMP2MSX -> .BMP file.  
3) ASEPRITE pour utiliser le fichier BMP et des layers (adapter la palette 16 couleurs à son goût), ASEPRITE -> .PNG file.  
4) PNG to MIFUI pour obtenir le fichier image .SC et le fichier palette .PL5    
5) Optionnel : Pour un fichier global SC5 (image + palette) je lance mon programme perso ou un BSAVE de l'image affichée sur MSX (emulator).  

Le format est similaire à tous les fichiers BLOAD :  
 #FE, DW BEGIN, DW END, DW START (Non utilisé) les données sont des données brutes. 
 
# BLOAD
Charger une image **SCREEN 5** qui est sur disk :  
```
10 SCREEN 5:COLOR 15,0,0  
20 BLOAD"A:SCREEN.SC5",S  
30 COLOR=RESTORE  
40 A$=INPUT$(1)  
```
Avec 2 fichiers (SC5+PL5) :  
```  
10 SCREEN 5  
20 COLOR ,,0  
30 VDP(9)=VDP(9)OR&H20  
40 BLOAD"screen.pl5",S  
50 COLOR=RESTORE  
60 BLOAD"screen.sc5",S  
70 A$=INPUT$(1)  

```
Note La couleur 0 est transparente, la couleur de bordure est vue comme transparente - pour la passer en couleur normale, on rajoute :  
```
VDP(9)=VDP(9) OR &H20
```
Revenir à l'état initial transparent :
```
VDP(9)=VDP(9) AND &HDF
```
# BSAVE 
```
10 screen 5
20 bload"name.pic",s
30 bsave"name.sc5",0,&h769f,s
```
L'entête du fichier :
```
 db #fe 	;ID byte
 dw {VRAM begin address}
 dW {VRAM end address}
 dw {not used when loading to VRAM}
```
Ceci dépend de la configuration de votre VDP (consultez les commandes BASIC VDP, BASE, SCREEN et SET PAGE). Lorsque vous démarrez votre ordinateur, les adresses par défaut dans SCREEN 5 sont :
```
0000-69FF  Bitmap
7400-75FF  Couleurs des sprites
7600-767F  Table d'attributs des sprites
7680-769F  Palette (fonctionnalité MSX-BASIC. Stockée en réalité dans le VDP)
7800-7FFF  Motifs des caractères des sprites
```
La partie la plus intéressante est probablement le bitmap... En écran 5, l'écran est rempli de gauche à droite et de haut en bas. Chaque quartet représente une couleur de la palette de 16 couleurs. Cela signifie que la taille totale de l'écran en octets est de 128 * 212. Notez qu'en écran 5, vous pouvez utiliser le deuxième paramètre (0-3) de la commande SET PAGE pour ajouter un décalage (#8000) aux commandes BLOAD, VPOKE, etc. De cette façon, vous pouvez accéder à l'ensemble des 128 Ko (#00000-#1FFFF) bien que l'en-tête BLOAD et les paramètres de commande n'utilisent que des adresses 16 bits.

# Modifier les CHR :
```
100 a=peek(5)*256+peek(4)  
101 for e=0 to 2047  
102 u=peek(a+e)  
103 poke &hd000+e,u or u/2  
104 next  
200 poke &hf91f,(inp(&ha8)and48)/16  
201 poke &hf920,0  
202 poke &hf921,&hd0  
203 screen 1 
210 screen 2  
220 OPEN "GRP:" FOR OUTPUT AS #1  
230 print #1,"ceci est un TEST"  
240 goto 240  
```


> [!TIP]  
> Pour tester plus facilement :  
> [MSX Pen](https://msxpen.com/)
