# Practica 4

Aquesta pràctica consisteix en realitzar dos exercissis per familiaritzar-nos amb el funcionament de connexions WiFi i Bluetooth amb la placa ESP32.

## Exercici 1

En aquest exercici veurem el funcionament de les connexions a través de WiFi, generant un servidor al qual ens connectarem amb el mòvil. Aquest servidor ens connectarà a una pàgina web simple que generarem en html al mateix codi.

### Funció `setup()`

En aquesta funció realitzarem la major part del processat necessari. Primer inicialitzarem el WiFi; definint el SSID i la contrasenya amb la funció `WiFi.begin(ssid, password)` Aqui també definirem un petit “subprograma” que ens indiqui si la placa s’està connectant a la WiFi i al servidor:

```cpp
while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }
```

Un cop la placa es connecta a la WiFi, ho indiquem per pantalla; a més d’indicar la direcció IP i dona pas a la funció `loop()`.

### Funció `loop()`

Aquesta funció du a terme el `server.handleClient()` , que gestiona els clients que es connecten al servidor.

### Pàgina web

Aquesta petita part de codi és la que donarà forma a la pàgina web a la qual ens connectarem des del mòvil a través de l’adreça IP:

```cpp
String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1>         Text here          </h1>\
</body>\
</html>";
```

## Exercici 2

En aquest segón exercici veurem el funcionament de les connexions Bluetooth. A més del codi de la placa, utilitzarem una aplicació externa al mòvil per poder-nos connectar a la placa i enviar-li missatges des del mòvil.

### Funció `setup()`

Després d’inicialitzar el `BluetoothSerial` la funció `setup()` ens indicarà que el Bluetooth està encès i que ja podem vincular el mòvil a la placa.

### Funció `loop()`

En aquesta funció mirarem si és possible rebre informació; i en cas afirmatiu, permetre la recepció de dades des del mòvil.

```cpp
void loop() {
  if (Serial.available()) {
  SerialBT.write(Serial.read());
  }
  if (SerialBT.available()) {
  Serial.write(SerialBT.read());
  }
  delay(20);
}
```

## Exercici de millora de nota 1

Aquest exercissi esxtra consistia en aprofundir en el funcionament de l’exercici 1 afegint a la pàgina web dos botons interactius per controlar dos LEDs que connectarem a la placa. En aquest cas, a falta de LEDs, connectarem els pins corresponents a un oscil·loscopi per comprovar el funcionament del programa.

### Funció `setup()`

La funció `setup()` tindrà la mateixa finalitat que en l’exercici 1, iniciar la WiFi, connectar-se i informar-nos de la IP.

### Funció `loop()`

Aquesta funció tindrà més contingut que en el primer exercici de WiFi. Aquest cop cal que tractem les respostes de la pàgina web per que controlin els LEDs. Per això definirem el resultat que volem obtenir depenent de la informació que rebem de la pàgina web.

```cpp
if (header.indexOf("GET /26/on") >= 0) {
  Serial.println("GPIO 26 on");
  output26State = "on";
  digitalWrite(output26, HIGH);
} else if (header.indexOf("GET /26/off") >= 0) {
  Serial.println("GPIO 26 off");
  output26State = "off";
  digitalWrite(output26, LOW);
} else if (header.indexOf("GET /27/on") >= 0) {
  Serial.println("GPIO 27 on");
  output27State = "on";
  digitalWrite(output27, HIGH);
} else if (header.indexOf("GET /27/off") >= 0) {
  Serial.println("GPIO 27 off");
  output27State = "off";
  digitalWrite(output27, LOW);
}
```

### Pàgina web

Aquest cop, la pàgina web també la definirem amb HTML però afegint-hi dos botons cadascun amb dos estats (ON i OFF). Indicarem també a quin LED correspon cada botó.

## Exercici de millora de nota 2

Aquest exercici consisteix també en aprofundir en les connexions Bluetooth; en aquest cas, el BLE (Bluetooth Low Energy).
