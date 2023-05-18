# Proyecto Montacargas ARDUINO

![portada](https://github.com/IbarrolaCamila/Proyecto-Estacion-de-Subte-Arduino/blob/main/img/TINKERCAD.png "portada")
## Integrantes:
* Ibarrola Camila.

![montacargas](https://github.com/IbarrolaCamila/Proyecto-Montacargas-Arduino/blob/master/img/montacargas.png"montacargas")

## Descripción:

Un sistema que permite al usuario saber a qué piso del montacargas se encuentra, además le permite, mantener el montacargas en el mismo piso, subir o bajar. 

## Función principal:

Su función es recibir el estado de los pulsadores (subir/bajar),  luego verifica en un periodo de 3 segundos si llega una señal de pausa, si es así, mantiene el montacargas en el mismo piso, sino ejecuta la siguiente acción elegida (subir/bajar). 

Muestra como se conforma el codigo:
```c++
//FUNCIÓN PRINCIPAL:
void subir_bajar(int pulsador, int pulsador2){
  if (pulsador == LOW) {
    if (verificar_pausa() == false){
      digitalWrite(LED_ROJO, LOW);
      piso_actual = piso_montacarga(piso_actual, "subir");
      Serial.println("subiendo");
      digitalWrite(LED_VERDE, HIGH);
      delay(demora_led);
      segmento(piso_actual);
     }
  }
  if (pulsador2 == LOW) {
    if (verificar_pausa() == false){
      digitalWrite(LED_ROJO, LOW);
      piso_actual = piso_montacarga(piso_actual, "bajar");
      Serial.println("bajando");
      digitalWrite(LED_VERDE, HIGH);
      delay(demora_led);
      segmento(piso_actual);
    }
  }
} // sube, baja o mantiene el piso actual del montacarga
```
### Funciones secundarias:
```c++
void segmento(int numero) {
  PORTD = matriz_segmento[numero] << 2;
  PORTB = matriz_segmento[numero] >> 6;
} // configura los puertos de salida para el 7 segmentos


int piso_montacarga(int piso, const char* accion){
  if (accion == "bajar"){
    if (piso <= 0){
      piso = 0; 
    }else if (piso > 0 && piso < 10){
      piso--;
    }
  }else if (accion == "subir"){
    if (piso >= 9){
      piso = 9;
    }else if (piso >= 0 && piso < 9){
      piso++;
    }
  }
  return piso;
} // verifica, valida y luego retorna el piso que corresponda segun la accion


bool verificar_pausa(){
  int contador = 0;
  bool check = false;
  
  Serial.println("ingresando al check");
  
  while (contador <= intervalos_pausa){
   delay(ping_pausar);
   if (digitalRead(PULSADORTRES) == LOW){
     Serial.println("Pausando");
     digitalWrite(LED_ROJO, HIGH);
     check = true;
     break;
   }
    contador++;
  }
  return check;
} // Verifica el pulsador pausar y retorna un bool.

```
### Matriz 7 segmentos:

```c++
int matriz_segmento[] = {
  // bafgcde
  0b01110111, //   "0"       
  0b01000100, //   "1"       
  0b01101011, //   "2"       
  0b01101110, //   "3"       
  0b01011100, //   "4"       
  0b00111110, //   "5"       
  0b00111111, //   "6"       
  0b01100100, //   "7"
  0b01111111, //   "8"
  0b01111110, //   "9"
}; // valor binario de un dígito del 0 al 9 para el display de 7 segmentos

```

## Link del proyecto :arrow_double_up:

* [proyecto](https://www.tinkercad.com/things/0Srxj39qDag)


### Fuentes:

* [tutorial ARDUINO](https://www.youtube.com/watch?v=Y4qtI3yk_s0&t=200s)
* [emojis](https://gist.github.com/rxaviers/7360908)