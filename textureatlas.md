# Texture atlas

När du arbetar med bilder i ett kodprojekt finns det ett par riktlinjer att förhålla sig till. Det rör dels filnamn men även hur bilderna sparas.

## Filnamnsguide

* inga mellanslag, använd _
* inga specialtecken, åäö,:;'*?"\¤"#%@//
* använd helst bara små bokstäver (kolla även filändelsen)

## Free Texture Packer

Spel sparar ofta bilder i form av spritesheets, vilket är flera bilder samlade i en bild. För att skapa en spritesheet så 
kan du använda ett program som Free Texture Packer http://free-tex-packer.com/app/ .
Programmet låter dig dela upp färdiga spritesheets, men även ladda in flera bilder och skapa spritesheets.
Till den färdiga spritesheeten så skapar även programmet en fil med information kring spritesheeten. Denna fil kallas för en atlas
och för det här projektet så sparas den för Phaser(phaser är spelmotorn du ska använda).

För att skapa en spritesheet med tillhörande atlas.

IMG

* Lägg till bilderna i separata frames(går att splitta ett sheet vid behov)
* Kontrollera bilderna(den tar bort duplicerade automatiskt)
* välj ett filnamn
* välj format, phaser3
* disable smoothing(möjligen)
* exportera

