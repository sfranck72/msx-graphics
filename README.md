# msx-graphics
Source to load and save images on MSX

Méthode:  
1) I grab a reference that I resize to 256x212.  
2) BMP2MSX to limit the palette, BMP2MSX -> .BMP file.  
3) ASEPRITE to draw the BMP file and add layers (tune the 16 colors palette for my taste), ASEPRITE -> .PNG file.  
4) .PNG to MIFUI to obtain the SC5 and PL5 files.  
5) optional : For the single SC5 file (screen + palette) I use a personal program or BSAVE the image on the MSX (emulator).  

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

