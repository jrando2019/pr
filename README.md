# Sensor de Humedad 🌱 (Práctica 7)

Este script en Python está diseñado para ejecutarse en una Raspberry Pi y se conecta con un sensor de humedad del suelo para monitorear el nivel de humedad del sustrato de una planta. Cuando la humedad del suelo cae por debajo de un umbral determinado, se activa una indicación de que la planta necesita ser regada.

## Circuito 🔋

## Video 🎥

Link -> [Video p7 🌱](https://urjc-my.sharepoint.com/:v:/g/personal/j_rando_2019_alumnos_urjc_es/EZVOeQH7JU9NtNICULWBm4ABM6OLeIcLRRN6l4jsbZKfSA?e=XasetT)

## Explicación del código

### Importación de módulos:

- Se importa el módulo `RPi.GPIO` para interactuar con los pines GPIO de la Raspberry Pi.
- También se importan los módulos `signal`, `sys` y `time` para gestionar señales, realizar operaciones del sistema y pausar la ejecución, respectivamente.

### Definición de constantes:

- `ENTRADA_SENSOR`, `LED_LISTO`, y `LED_INDICADOR` representan los números de pin GPIO utilizados para el sensor de humedad y los LEDs de indicación.

### Variables globales:

- `water` se utiliza para almacenar el estado actual de la humedad del suelo.

### Función de devolución de llamada (`callbackHumedad`):

- Esta función se ejecuta cuando hay un cambio en el estado del sensor de humedad.
- Invierte el valor de la variable global `water`.

### Configuración inicial (`setup`):

- Configura los pines GPIO, establece el modo de numeración BCM, y configura los pines como entrada o salida según sea necesario.
- Enciende el LED_LISTO para indicar que el sistema está listo.
- Configura la detección de eventos en el pin del sensor de humedad (`ENTRADA_SENSOR`) llamando a `callbackHumedad` con un tiempo de rebote de 300 ms.

### Función principal (`main`):

- Llama a la función `setup` para la configuración inicial.
- Inicializa la variable `an_water` para realizar un seguimiento del estado anterior del agua.
- En un bucle infinito, compara el estado actual del agua con el estado anterior y realiza acciones en consecuencia.
- Muestra mensajes en la consola y enciende o apaga el LED_INDICADOR según la necesidad de riego.
- Espera 0.1 segundos antes de la siguiente iteración.

### Punto de entrada principal (`if __name__ == '__main__':`):

- Llama a la función `main` cuando se ejecuta el script.
