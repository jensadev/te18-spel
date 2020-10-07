# Texture atlas

När du arbetar med bilder i ett kodprojekt finns det ett par riktlinjer att förhålla sig till. Det rör dels filnamn men även hur bilderna sparas.

## Filnamnsguide

* inga mellanslag, använd _
* inga specialtecken, åäö,:;'*?"\¤"#%@//
* använd helst bara små bokstäver (kolla även filändelsen)

## Free Texture Packer

Spel sparar ofta bilder i form av spritesheets, vilket är flera bilder samlade i en bild. För att skapa en spritesheet så 
kan du använda ett program som [Free Texture Packer](http://free-tex-packer.com/app/).
Programmet låter dig dela upp färdiga spritesheets, men även ladda in flera bilder och skapa spritesheets.
Till den färdiga spritesheeten så skapar även programmet en fil med information kring spritesheeten. Denna fil kallas för en atlas
och för det här projektet så sparas den för Phaser(phaser är spelmotorn du ska använda).

För att skapa en spritesheet med tillhörande atlas.

![FTP image](https://raw.githubusercontent.com/jensnti/te18-spel/main/doc/ftp-1.png)

* Lägg till bilderna i separata frames(går att splitta ett sheet vid behov)
* Kontrollera bilderna(den tar bort duplicerade automatiskt)
* välj ett filnamn
* välj format, phaser3
* disable smoothing(möjligen)
* exportera

Resultatet blir en spritesheet och en json fil.

![Caveperson](https://raw.githubusercontent.com/jensnti/te18-spel/main/assets/caveperson.png)

Här följer ett utdrag ur json filen. Den består av ett json object som innehåller en array med textures.
Den första delen anger vilken spritesheet(bild) som ska användas, dess format och storlek.
Det som följer är varje ruta(frame) som bilden innehåller, dvs. varje enskild bild. Namnen på dessa utgår från 
filnamnen när filken skapades(men de kan med fördel redigeras till att vara något mer beskrivande än 0.png). Varje
ruta har ett antal egenskaper som bredd, höjd och position på spritesheetet.

```
{
	"textures": [
		{
			"image": "caveperson.png",
			"format": "RGBA8888",
			"size": {
				"w": 58,
				"h": 192
			},
			"scale": 1,
			"frames": [
				{
					"filename": "0.png",
					"rotated": false,
					"trimmed": true,
					"sourceSize": {
						"w": 64,
						"h": 64
					},
					"spriteSourceSize": {
						"x": 2,
						"y": 0,
						"w": 58,
						"h": 64
					},
					"frame": {
						"x": 0,
						"y": 0,
						"w": 58,
						"h": 64
					}
				},
```

## Använda en atlas i Phaser

I spelets preload metod behöver atlasen laddas. Funktionen tar ett antal parametrar, ange: assetname, source, folder.

```
function preload() {
  this.load.multiatlas('caveperson', 'assets/caveperson.json', 'assets/');
  this.load.multiatlas('world', 'assets/world.json', 'assets/');
}
```
Den här koden laddar 2 atlas som får asset-namnen caveperson och world.
Efter att preloaden har utförs är de redo att användas. Statiska bilder kan laddas in från den(som texturer) men för animerade
objekt behöver animation skapas. Detta görs i spelets create metod. 

### Animation 
För att skapa en run-cycle för Caveperson. 

```
this.anims.create({
  key: 'run',
  frames: this.anims.generateFrameNames('caveperson', {
    frames: ['0.png', '1.png', '2.png', '1.png']
  }),
  frameRate: 10,
  repeat: -1
});
```
* key är namnet på animationen
* frames är de frames som ska skapas, de kan skapas automatiskt eller anges statiskt. Notera att i exemplet ovan så återanvänds frame 1.
* Sedan följer framerate och repeat.

### Sprite eller Image

Ladda in en bild. Parametrarna är x, y, atlas, frame.

```
this.add.image(400, 300, 'world', '00.png');
```

Ladda in en bild som en sprite. Parametrarna är x, y, atlas, frame.

```
let bg = this.add.sprite(400, 300, 'world', '00.png');
```

För att skapa en bild i en grupp(för att använda fysik bland annat). Först skapas gruppen, sedan skapas objektet i gruppen.
Parametrar är: x, y, atlas följt av .setFrame(frame);

```
platforms = this.physics.add.staticGroup();
platforms.create(i, 568, 'world').setFrame('12.png');
```

## Manipulation

Inladdade bilder och animationer från atlasen kan sedan manipuleras. De kan roteras skalas och flippas.
Detta är väldigt användbart för att justera storleken men även för att vända på animationer(så vi kan återanvända dem).

Koden för detta ser ut som följer, för att uppdatera modifierade objekt så är det viktigt att köra refreshBody() efter
skalan ändrats. Detta gör så att det faktiska fysikobjektet uppdateras (kontrollera detta genom att sätta debug: true i config).

```
.setScale(x,y);
.refreshBody();

.flipX = true/false;
```
