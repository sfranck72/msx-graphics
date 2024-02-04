# msx-graphics
Source to load and save images on MSX

Si on utilise **BMP2MSX** pour le Screen 5, suivre ce code:

10 SCREEN 5  
20 BLOAD"SCREEN.SC5",S  
30 COLOR=RESTORE  
40 A$=INPUT$(1)  

Le format est similaire à tous les fichiers BLOAD. 
 #FE, DW BEGIN, DW END, DW START (Non utilisé) les données sont des données brutes. 

Méthode:  
1) I grab a reference that I resize to 256x212.  
2) BMP2MSX to limit the palette, BMP2MSX -> .BMP file.  
3) ASEPRITE to draw the BMP file and add layers (tune the 16 colors palette for my taste), ASEPRITE -> .PNG file.  
4) .PNG to MIFUI to obtain the SC5 and PL5 files.  
5) optional : For the single SC5 file (screen + palette) I use a personal program or BSAVE the image on the MSX (emulator).  

For MSX newcomers, the BASIC for loading image on MSX or emulator when files are on a disk, ex :  

10 SCREEN 5:COLOR 15,0,0  
20 BLOAD"A:AWAKE.SC5",S  
30 COLOR=RESTORE  
40 A$=INPUT$(1)  
