# msx-graphics
source to load and save images on MSX

Si on utilise **BMP2MSX** pour le Screen 5, suivre ce code:

10 SCREEN 5
20 BLOAD"SCREEN.SC5",S
30 COLOR=RESTORE
40 A$=INPUT$(1)

Le format est similaire à tous les fichiers BLOAD. 
 #FE, DW BEGIN, DW END, DW START (Non utilisé) les données sont des données brutes. 
