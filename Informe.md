# PRÀCTICA 7
En aquesta pràctica  reproduirem música tant des de la memòria interna com des d'una targeta SD externa.

## EXERCICI 1: Reproducció desde la memòria interna
Per aquesta exercici només necessitem un altaveu i cables. Pel muntatge he seguit la següent assignació de pins: 
- LRC --> G25
- BCLK --> G26
- DIN --> G22
- GND --> GND
- Vin --> 3.3V


El que obtenim quan fem *Upload and Monitor* és el següent:[Vídeo funcionament](https://drive.google.com/file/d/1YKYbzegySmjhGpX2p5JX_yrmkcF-wALd/view?usp=sharing).
Com podem veure en el vídeo es repordueix un àudio per l'altaveu i per pantalla es mostra *Sound Generator*.  

El funcionament del programa consisteix en: les dades de so s'emmagatzemen com una matriu en la RAM interna del ESP32. Fem servir la placa de conexió audio MAX98357 I2S per decodificar el senyal digital en un senyal analògic. Trobem les dades de l'àudio en el fitxer *sampleaac.h*. Accedim a aquest fitxer en el setup: 
```cpp
in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
```
Aquest és el senyal d'entrada. A continuació s'han de convertir les dades en un àudio: 
```cpp
aac = new AudioGeneratorAAC();
```
I finalment que surtin pel port de sortida, en aquest cas un altaveu. 
```cpp
out = new AudioOutputI2S();
out -> SetGain(0.125);
out -> SetPinout(26,25,22);
aac->begin(in, out);
```
Quan s'acaba l'àudio, per pantalla es mostra en bucle el missatge *Sound Generator*. Això és el que tenim programat en el loop: 
```cpp
void loop(){
if (aac->isRunning()) {
aac->loop();
} else {
aac -> stop();
Serial.printf("Sound Generator\n");
delay(1000);
}
}
```
## EXERCICI 2: Reproducció d'un arxiu WAV des d'una targeta SD externa
El codi d'aquest exercici i el seu corresponent informe el podem trobar en el repositori *Pràctica7_ex2*.
