![image](https://github.com/fedecorbalan/montacargas-parcial1/assets/123754871/85a89b76-4714-4d75-8fbc-b7d93cd2f023)
# Primer parcial practico SPD
#### Alumno: Federico Corbalán 
##### En este proyecto, se pidio armar un modelo de montacarga funcional como maqueta para un hospital. El objetivo fue la implementacion de un sistema que recibira ordenes de subir, bajar o pausar desde diferentes pisos y muestre el estado del montacargas en un display 7 segmentos.
# Modelo Realizado
![image](https://github.com/fedecorbalan/montacargas-parcial1/assets/123754871/ab0caa4f-fffb-46a1-8031-7b442588b73b)
#### Para este modelo, se han utilizado 3 (tres) pulsadores cada uno con resistencias de 10 kΩ, una placa de pruebas (o protoboard) la cual tiene conectada en el polo negativo una resistencia de 220Ω la cual sirve para la correcta función de los 2 (dos) leds utilizados y el display de 7 segmentos, no se ingresa ninguna corriente en el polo positivo.

# Funcionalidad del modelo (Código)
Para comenzar con el codigo, se definen las las entradas a las cuales estan conectados los elementos, se firman las funciones y se declaran las variables.
~~~ 
//C++ code
//Se definen los pines
//PULSADORES
#define pulsador1 A0
#define pulsador2 A2
#define pulsador3 A1
//DISPLAY
#define A 12
#define B 13
#define C 9
#define D 8
#define E 7
#define F 11
#define G 10
//LEDS
#define LED_ROJO 4
#define LED_VERDE 5
//SE FIRMAN LAS FUNCIONES
void EncenderDisplay(int numero);
int contador = 0;
void BotonEmergencia();
bool Flag = true;
~~~
Luego se asignan las entradas y salidas analogicas y se inicializa el puerto serial.
~~~
void setup()
{
  pinMode(pulsador1, INPUT);
  pinMode(pulsador2, INPUT);
  pinMode(pulsador3, INPUT);
  pinMode(A, OUTPUT);
  pinMode(B, OUTPUT);
  pinMode(C, OUTPUT);
  pinMode(D, OUTPUT);
  pinMode(E, OUTPUT);
  pinMode(F, OUTPUT);
  pinMode(G, OUTPUT);
  pinMode(LED_ROJO, OUTPUT);
  pinMode(LED_VERDE, OUTPUT);	
  Serial.begin(9600);
}
~~~
## Funciones utilizadas
#### Se crea la funcion EncenderDisplay, a la cual se le ingresa por parametro un numero del 0 al 9 y en el caso que corresponda, lo muestra en el dei
~~~
void EncenderDisplay(int numero){
 switch(numero)
  {
  	case 1:
  		digitalWrite(B, HIGH);
    	digitalWrite(C, HIGH);
  		delay(3000);
    	digitalWrite(B, LOW);
    	digitalWrite(C , LOW);
    	break;
    case 2:
      digitalWrite(A, HIGH);
    	digitalWrite(B, HIGH);
      digitalWrite(D, HIGH);
    	digitalWrite(E, HIGH);
      digitalWrite(G, HIGH);
  		delay(3000);
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(D, LOW);
      digitalWrite(E, LOW);
    	digitalWrite(G, LOW);
    	break;
        
    case 3:
    	digitalWrite(A, HIGH);
    	digitalWrite(B, HIGH);  		
      digitalWrite(C, HIGH);
    	digitalWrite(D, HIGH);
      digitalWrite(G, HIGH);
  		delay(3000);
    	digitalWrite(A, LOW);  		
    	digitalWrite(B, LOW);  		
    	digitalWrite(C, LOW);  		
      digitalWrite(D, LOW);  		
    	digitalWrite(G, LOW);
    	break;
    case 4:
    	digitalWrite(B, HIGH);
    	digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
  		delay(3000);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
      digitalWrite(F, LOW);
    	digitalWrite(G, LOW);
    	break;
    case 5:
    	digitalWrite(A, HIGH);
    	digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
    	digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
  		delay(3000);
    	digitalWrite(A, LOW);
    	digitalWrite(C, LOW);
    	digitalWrite(D, LOW);
      digitalWrite(F, LOW);
    	digitalWrite(G, LOW);
    	break;
    case 6:
    	digitalWrite(A, HIGH);
      digitalWrite(C, HIGH);
    	digitalWrite(D, HIGH);
    	digitalWrite(E, HIGH);
    	digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
  		delay(3000);
    	digitalWrite(A, LOW);
    	digitalWrite(C, LOW);
    	digitalWrite(D, LOW);
      digitalWrite(E, LOW);
    	digitalWrite(F, LOW);
    	digitalWrite(G, LOW);
    	break;
    case 7:
    	digitalWrite(A, HIGH);
    	digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
  		delay(3000);
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
    	break;
    case 8:
    	digitalWrite(A, HIGH);
    	digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
    	digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
    	delay(3000);
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
      digitalWrite(C, LOW);
    	digitalWrite(D, LOW);
      digitalWrite(E, LOW);
    	digitalWrite(F, LOW);
    	break;
    case 9:
    	digitalWrite(A, HIGH);
    	digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
    	digitalWrite(D, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
  		delay(3000);
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
      digitalWrite(D, LOW);
    	digitalWrite(F, LOW);
      digitalWrite(G, LOW);
    	break;
    case 0:
    	digitalWrite(A, HIGH);
    	digitalWrite(B, HIGH);
       digitalWrite(C, HIGH);
    	digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
    	digitalWrite(F, HIGH);
  		delay(3000);
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
      digitalWrite(D, LOW);
    	digitalWrite(E, LOW);
    	digitalWrite(F, LOW);
    	break;
  }
}
~~~
### Tambien se crea la funcion BotonEmergencia, a la cual no se le ingresa por parametro. Lo que hace consiste en que cuando se pulse el boton de emergencia (el del medio), la flag declarada anteriormente como true, cambie a false, impidiendo asi el funcionamiento del codigo que se ejecuta en el loop.
~~~
void BotonEmergencia()
{
  while(digitalRead(pulsador3) == LOW)
  {               
    EncenderDisplay(contador);
    digitalWrite(LED_ROJO, HIGH);
    digitalWrite(LED_VERDE, LOW);
    Serial.print("Boton de emergencia, se encuentra parado en el piso ");
    Serial.println(contador);
    delay(10);
    Flag = false;
    if(digitalRead(pulsador3) == LOW)
    {
      Flag = true;
      Serial.print("PROCEDA");
      delay(1000);
      digitalWrite(LED_ROJO, LOW);
      break;
    }
  }
}
~~~
### Por último, en el loop se llama a la funcion BotonEmergencia y luego se aplican condicionales en el caso de que se oprima alguno de los dos botones.
- Se utilizan flags para poder entrar a los botones de subida y bajada, dado a que en el caso de que se utilize la funcion BotonEmergencia, la flag va a ser false, lo cual impide el funcionamiento de los otros botones hasta que se presione devuelta.
-Se utiliza un contador inicializado en 0 y a la funcion EncenderDisplay se le va a ingresar por parametro el este mismo, el cual va a representar el numero que se va a mostrar
- En el caso de que el contador sea igual a 0 y se quiera bajar de piso, el display no va a dejar de mostrar el numero 0 hasta que se suba de piso, lo mismo ocurre en el caso de que se llegue al piso 9, pero en ese caso, no deja de ser 9 hasta que baje de piso.
- Tambien se configuro el prendido y apagado de las luces en base al funcionamiento del montacargas, cada ves que se mueve, se prende el led verde, sino, se prende el led rojo.
~~~
void loop()
{
  BotonEmergencia();
  if (digitalRead(pulsador1) == LOW)
  {
    if(Flag == true)
    {
      if(contador == 9)
      {
        contador = 9;
        digitalWrite(LED_ROJO, HIGH);
        EncenderDisplay(contador);
        Serial.print("Usted se encuentra en el piso ");
        Serial.println(contador);
      }
      else
      {

        contador++;
        digitalWrite(LED_ROJO, LOW);
        digitalWrite(LED_VERDE, HIGH);
        EncenderDisplay(contador);
        digitalWrite(LED_VERDE, LOW);
        digitalWrite(LED_ROJO, HIGH);
        Serial.print("Usted se encuentra en el piso ");
        Serial.println(contador);
      }
    }
  }
  else if(digitalRead(pulsador2) == LOW)
  {
    if(Flag == true)
    {
      if(contador == 0)
      {
        contador = 0;
        digitalWrite(LED_ROJO, HIGH);
        EncenderDisplay(contador);
        Serial.print("Usted se encuentra en el piso ");
        Serial.println(contador);
      }
      else
      {
        contador--;
        digitalWrite(LED_ROJO, LOW);
        digitalWrite(LED_VERDE, HIGH);
        EncenderDisplay(contador);
        digitalWrite(LED_VERDE, LOW);
        digitalWrite(LED_ROJO, HIGH);
        Serial.print("Usted se encuentra en el piso ");
        Serial.println(contador);
      }
    }
  }
  
}
~~~
## :robot: Link al proyecto
- [Montacargas Arduino](https://www.tinkercad.com/things/dgiVyPlA8JO)






