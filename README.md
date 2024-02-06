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
BSAVE :
```
10 screen 5
20 bload"name.pic",s
30 bsave"name.sc5",0,&h769f,s
```
Header is like this:
```
 db #fe 	;ID byte
 dw {VRAM begin address}
 dW {VRAM end address}
 dw {not used when loading to VRAM}
```
What goes to where depends of how you have set up the VDP... (See BASIC commands VDP, BASE, SCREEN and SET PAGE) When you boot your computer the default addresses in SCREEN 5 are:
```
0000-69FF   Bitmap
 7400-75FF   Sprite colours
 7600-767F   Sprite attribute table
 7680-769F   Palette (MSX-BASIC feature. Stored actually inside VDP)
 7800-7FFF   Sprite character patterns
```
The most interesting part is probably the bitmap... In screen 5 the screen is filled from left to right and up to down. Each nibble represents one color out of the 16-color palette. This means that the whole screen size in bytes is 128 * 212. Please note that in screen 5 you can use second parameter (0-3) of SET PAGE command to add (#8000) offset to BLOAD, VPOKE etc commands... This way you can access whole 128KB (#00000-#1FFFF) although BLOAD header and command parameters use only 16-bit addresses.

Modifier les CHR :
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
