# msx-graphics
Source to load and save images on MSX

Méthode:  
1) Redimensionner l'image en 256x212.  
2) BMP2MSX pour limiter la palette à 16 couleurs, BMP2MSX -> .BMP file.  
3) ASEPRITE pour utiliser le fichier BMP et des layers (adapter la palette 16 couleurs à son goût), ASEPRITE -> .PNG file.  
4) .PNG to MIFUI pour obtenir le fichier image .SC et le fichier palette .PL5    
5) optionnel : Pour un fichier global SC5 (image + palette) je lance mon programme perso ou un BSAVE de l'image affichée sur MSX (emulator).  

Le format est similaire à tous les fichiers BLOAD. 
 #FE, DW BEGIN, DW END, DW START (Non utilisé) les données sont des données brutes. 

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

