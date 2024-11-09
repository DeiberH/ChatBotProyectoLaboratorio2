MANUAL DE USUARIO
Bienvenido al manual de usuario de este proyecto.

Inicio(JFrame)
Este es el JFrame que veremos cuando iniciemos el proyecto. 
	
Empezar(JButton)
Botón para inicializar el programa de chatbot, contiene una línea de código que permite cerrar el JFrame Inicio cuando se presione este botón.

UIOllama(JFrame)
en el apartado estético existen:
JList historial: para ver los distintos chats en el historial 
Jlist Chat: para ver la interacción entre el usuario y el chatbot
TextField prompt: para ingresar lo que el usuario desea escribirle al chatbot
JButton enviar: para que el texto en prompt se envíe a la JList de chat y se pueda interactuar con el chatbot
JButton nuevo:  para guardar el chat y visualizarlo en el historial
JButton jButton1: para salir de UIOllama


IMPORTS
import java.io.IOException; 
Es una excepción comprobada en Java que indica un problema mientras se realizan operaciones de Entrada/Salida. Suele ocurrir cuando se produce un fallo al realizar operaciones de lectura, escritura o búsqueda en archivos o directorios

import java.io.OutputStream; 
Clase abstracta que define métodos para enviar datos de salida en forma de bytes. Es la clase base para varias clases en Java que manejan operaciones de salida, como escribir datos en archivos, en la consola, o en redes. 

import java.net.MalformedURLException; 
Esta excepción se lanza cuando una URL (Uniform Resource Locator) tiene un formato incorrecto o no es válida según el protocolo especificado. Es parte de las excepciones de Java relacionadas con operaciones de red y URL.

import java.net.HttpURLConnection; 
Permite realizar solicitudes HTTP (como GET, POST, PUT, DELETE) a un servidor web y manipular las respuestas de ese servidor

import java.net.URL; 
Representa una URL (Uniform Resource Locator) que proporciona una dirección única para un recurso disponible en la web, como una página web, una imagen, un archivo, o un servicio. Esta clase permite trabajar con direcciones URL y es fundamental para aplicaciones de red que necesitan acceder a recursos remotos.

import java.nio.charset.StandardCharsets; 
Esta clase contiene constantes estáticas que representan conjuntos de caracteres estándar y ampliamente usados (o "charsets"), como UTF-8, UTF-16, y US-ASCII. Se puede especificar el charset para operaciones de codificación y decodificación de cadenas de texto, especialmente en entrada/salida de archivos o comunicación en red.

import java.io.BufferedReader; 
Permite leer texto de una secuencia de entrada (como un archivo o una entrada estándar) de manera eficiente, almacenando en memoria un buffer de datos para reducir el número de accesos directos al flujo. Esto mejora el rendimiento en comparación con leer cada carácter o línea individualmente sin un buffer.

import java.io.InputStreamReader; 
Convierte un flujo de bytes (InputStream) en un flujo de caracteres (Reader) utilizando una codificación de caracteres específica (como UTF-8). Es comúnmente utilizada para leer datos de entrada, como texto proveniente de un archivo o de la entrada estándar (teclado).

import java.net.ProtocolException; 
Esta excepción se lanza cuando ocurre un error relacionado con el protocolo de red utilizado en una conexión. ProtocolException es una subclase de IOException y generalmente indica que hay un problema con el uso o implementación del protocolo de comunicación (como HTTP).

import java.util.logging.Level; 
Define los niveles de severidad para los mensajes de registro (logging), permitiendo que el sistema de registro clasifique y filtre los mensajes en función de su importancia o severidad. Esto es útil para gestionar el registro de eventos en aplicaciones de manera estructurada, controlando qué mensajes se muestran o almacenan.

import java.util.logging.Logger; 
Es parte de la biblioteca estándar de Java para el registro de mensajes, y permite a los desarrolladores capturar y registrar eventos en una aplicación, facilitando el monitoreo, la depuración y el análisis del comportamiento de la aplicación.

import org.json.JSONObject; 
Esta clase es parte de la biblioteca de JSON de Java y se utiliza para crear, manipular y analizar objetos JSON. JSON (JavaScript Object Notation) es un formato de intercambio de datos ampliamente utilizado debido a su simplicidad y ligereza.


EXPLICACIÓN DE LA FUNCIÓN CHATBOT
- public static String ChatBot(String prompt) throws IOExecption{} 
Creación de una función que devuelve un String y toma como parámetro a String prompt (el cual es el TextField en el que se ingresará lo que el usuario desee)

- String modelo="llama3.2"; 
Es el nombre del modelo de la IA

- Posterior a esto se conecta con el enlace con el siguiente código: 
URL url = null;
    try {
        url = new URL("http://localhost:11434/api/generate");
    } catch (MalformedURLException ex) {
        Logger.getLogger(UIOllama.class.getName()).log(Level.SEVERE, null, ex);
    }
Donde se crea el objeto url que contendrá la dirección a la que se enviará la solicitud HTTP para generar la respuesta

- Se establece la conexión HTTP a la URLy se configura la solicitud como "POST" con:
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        try {
            conn.setRequestMethod("POST");
        } catch (ProtocolException ex) {
            Logger.getLogger(UIOllama.class.getName()).log(Level.SEVERE, null, ex);
        }

- Se crea un objeto JSON como un String, que contiene el nombre del modelo y el prompt (el mensaje del usuario). Aquí, también se establece que stream es false:
String input = String.format("{\"model\": \"%s\",\"prompt\": \"%s\", \"stream\": false}", modelo, prompt);

- Se convierte el JSON en bytes y se envía al servidor a través de un flujo de salida: 
 try (OutputStream os = conn.getOutputStream()) {
        byte[] inputjson = input.getBytes(StandardCharsets.UTF_8);
        os.write(inputjson, 0, inputjson.length);
    }

- Se convierte la respuesta del servidor en un objeto JSONObject y se obtiene el valor correspondiente a la clave "response", que es la respuesta generada por el modelo de IA: 
 JSONObject jsonresponse = new JSONObject(response.toString());
 String responsetext = jsonresponse.getString("response");

- Y finalmente se retorna la respuesta y se cierra la conexión.



VARIABLES CREADAS
int indice;
int c;
String cht[];
String ind[];
String hstrl[][];
c = 0;
hstrl = new String[10000][9999];
cht = new String[9999];
ind = new String[9999];



EXPLICACIÓN DE LOS EVENTOS DEL CODIGO


private void nuevoMouseClicked(java.awt.event.MouseEvent evt){

Se almacena en la matriz hstrl[][] para asignar el nombre del chat actual en el historial. declarándose como "Chat"+(c+1). 

Se utiliza un ciclo para recorrer el hstrl[][], copiando los nombres de los chats de la primera columna al vector ind[]. Utilizándose este para mostrar el nombre de los chats en la JList historial de la interfaz gráfica. 

Se utiliza otro ciclo que recorre el vector cht[], el cual almacena el chat actual y limpia sus elementos. 

Se incrementa la variable c, para iterar.

Finalmente se actualizan los datos de las JList historial y Chat. 
historial.setListData(ind); actualizando la lista en los nombres de los chats de la interfaz gráfica
Chat.setListData(cht); limpiando el chat actual en la interfaz gráfica

}


private void enviarouseClicked(java.awt.event.MouseEvent evt){

Se le asigna a la variable String prpt lo que se obtenga del TextField prompt.

Se declara la variable String respuesta como null parcialmente.

Se llama a la función ChatBot(prpt) para obtener una respuesta del modelo IA. Haciendo que la variable respuesta sea el retorno de la función ChatBot
Si ocurre un error se registra en los logs

Se declara la variable boolean s= false;
Se usa un ciclo para recorrer el vector cht[], donde 
cht[i]= "Usuario: "+prpt;
cht[i+1]="Ollama: " + respuesta;
Se limpia prompt para la próxima asignación del usuario
s= true; para indicar que se ha agregado un nuevo mensaje y su respectiva respuesta

Se usa un ciclo que recorre cht[] para verificar que no sea nulo. Para así copiar todos los mensajes del chat actual del vector cht[] al historial hstrl[][] en la posición que le corresponde a la variable c.

Condiacional para verificar que s es true para actualizar la JList Chat.

}


private void historialMouseClicked(java.awt.event.MouseEvent evt){

Se le asigna a la variable int índice lo seleccionado en la JList historial 
La variable c pasa a ser índice

Se usa un ciclo para cargar los mensajes del chat seleccionado y se actualiza la interfaz gráfica.

}


private void jButton1MouseClicked(java.awt.event.MouseEvent evt) {

Se usa dispose(); para salir de la ventana actual.

}

