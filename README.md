# Montate en mi motora acertijo python programación visual estudio play juego nota 

## ❄ Explicación del codigo del juego 

## 1.Imports y constantes
```python
import tkinter as tk
import random
import webbrowser

VIDEO_URL = "https://www.youtube.com/watch?v=LXqIq4OxSk4"
```

```tkinter``` es la librería de GUI: crea ventanas, botones, etiquetas, etc.
```random``` sirve para elegir un acertijo al azar.
```webbrowser``` abre una URL en el navegador por defecto.
```VIDEO_URL``` es la dirección del video que se abrirá al pulsar el botón — una constante que separa datos de lógica.


## 2. Datos de la lista de acertijos
```python
ACERTIJOS = [
    {"pregunta": "...", "respuesta": "..."},
    ...
]
```

Es una lista de diccionarios; cada diccionario tiene pregunta y respuesta.

Piensa en ella como la “memoria” del juego: banco de problemas y soluciones.

Las respuestas están en minúsculas en el archivo original? No necesariamente, pero el código compara en minúsculas (ver más abajo), por eso funciona aun si el usuario escribe con mayúsculas.


## 3. Función ``` nuevo_acartijo() ```

```python
def nuevo_acertijo():
    global actual
    actual = random.choice(ACERTIJOS)
    lbl_pregunta.config(text="🧩 Acertijo:\n\n" + actual["pregunta"])
    entrada.delete(0, tk.END)
    resultado.set("")
```

¿Qué hace en términos cognitivos? — elige un problema nuevo y pone al programa en ese “estado” (almacena en actual).

```global actual```: indica que actual es una variable global usada por otras funciones (por ejemplo, verificar()).

```random.choice(...)``` selecciona un elemento aleatorio de la lista.

```lbl_pregunta.config(...)``` actualiza el texto que ve el usuario.

```entrada.delete(...)``` limpia la caja de texto para que el usuario pueda escribir la respuesta nueva.

```resultado.set("")``` borra mensajes previos de resultado.


## 4.Función ```verificar()```

```python
def verificar():
    user = entrada.get().lower().strip()

    if not user:
        resultado.set("❗ Escribe una respuesta.")
        return

    if user == actual["respuesta"]:
        resultado.set("✅ ¡Correcto! Cargando otro acertijo...")
        root.after(1500, nuevo_acertijo)
    else:
        resultado.set("❌ Incorrecto. Intenta de nuevo.")
```

(1) Lee lo que escribió el usuario: ```entrada.get()```.

(2)```lower().strip()``` normaliza la respuesta: quita espacios al principio/final y hace minúsculas → reduce errores por formato.

(3) Si está vacío, muestra un aviso y termina (no verifica).

(4) Compara con la respuesta correcta almacenada en ```actual["respuesta"]```.

(5) Si es correcta: muestra mensaje y programa un nuevo acertijo dentro de 1500 ms con ```root.after(1500, nuevo_acertijo)```. after no bloquea: programa un evento futuro en la cola de Tkinter.

(6) Si es incorrecta: muestra mensaje de intento fallido y deja la entrada para que el usuario lo corrija.


## 5. Función ```abrir_video()```

```python
def abrir_video():
    webbrowser.open(VIDEO_URL)
```

() imple: abre ```VIDEO_URL``` en el navegador del sistema.

() Cognitivamente: es una herramienta auxiliar que el programa ofrece al usuario (ver más abajo en la interfaz).


## 6. Interfaz grafica Tkinter: montajes y los componentes

```python
root = tk.Tk()
root.title("Juego de Acertijos 🧠")
root.geometry("500x350")
root.resizable(False, False)
```

() ```root``` es la ventana principal. Se le pone título, tamaño fijo y se desactiva el redimensionado.

Elementos dentro de ```root``` (con ```pack()``` para colocarlos verticalmente en orden):

```lbl_titulo```: etiqueta grande en la parte superior con el nombre del juego.

```frame_video```: frame (contenedor) para agrupar el texto y el botón del video en la misma línea.

```lbl_video```: texto que invita a ver el video.

```btn_video```: botón que ejecuta ```abrir_video()``` al pulsarlo.

```lbl_pregunta```: etiqueta central donde se muestra la pregunta actual. ```wraplength=460``` indica que el texto se ajusta en esa anchura para no salirse.

```entrada```: campo donde el usuario escribe su respuesta.

```btn```: botón que llama a ```verificar()``` (al presionar "Responder").

```resultado```: ```tk.StringVar()``` usado para mostrar mensajes dinámicos en ```lbl_res``` (correcto/incorrecto/avisos).

```lbl_res``` está ligado a resultado y se actualiza automáticamente ```cuando resultado.set(...)``` cambia.


## 7. Inicio y bocle principal
```python
# Cargar primer acertijo
nuevo_acertijo()

root.mainloop()
```

() Antes de mostrar la ventana, el programa llama ```nuevo_acertijo()``` para inicializar actual y mostrar la primera pregunta.

()``` root.mainloop()``` es el bucle de eventos de Tkinter: escucha clicks, entradas, temporizadores (```after```), etc. Hasta que el usuario cierre la ventana, el programa está “vivo”.


 ## ❄ Codigo general del Juego para correrlo en el visual estudio (no olvidar intalar el Tkinter)
 
 ```python
import tkinter as tk
import random
import webbrowser

VIDEO_URL = "https://www.youtube.com/watch?v=LXqIq4OxSk4"

# ---------------------------
# LISTA DE ACERTIJOS
# ---------------------------

ACERTIJOS = [
    {
        "pregunta": "Víctor le dice a Joel. ¿Qué le dice?",
        "respuesta": "montate en mi motora"
    },
    {
        "pregunta": "Pero Joel le dice a Víctor. ¿Qué le dice?",
        "respuesta": "desayuna con huevo"
    },
    {
        "pregunta": "Pero de momento Víctor le dice a Joel. ¿Qué le dice?",
        "respuesta": "toma mango"
    },
    {
        "pregunta": "De repente entonces Joel le dice a Víctor. ¿Qué le dice?",
        "respuesta": "quisiera ser una mosca para pararme en tu piel"
    },
    {
        "pregunta": "De repente Víctor le dice a Joel. ¿Qué soy?",
        "respuesta": "yo soy yo"
    },
    {
        "pregunta": "De momento Joel le dice a Víctor. ¿Qué le dice?",
        "respuesta": "feliz navidad"
    }
]

# ---------------------------
# FUNCIONES DEL JUEGO
# ---------------------------

def nuevo_acertijo():
    """Elige un acertijo aleatorio."""
    global actual
    actual = random.choice(ACERTIJOS)
    lbl_pregunta.config(text="🧩 Acertijo:\n\n" + actual["pregunta"])
    entrada.delete(0, tk.END)
    resultado.set("")

def verificar():
    """Verifica si la respuesta es correcta."""
    user = entrada.get().lower().strip()

    if not user:
        resultado.set("❗ Escribe una respuesta.")
        return

    if user == actual["respuesta"]:
        resultado.set("✅ ¡Correcto! Cargando otro acertijo...")
        root.after(1500, nuevo_acertijo)
    else:
        resultado.set("❌ Incorrecto. Intenta de nuevo.")

def abrir_video():
    """Abre el video en el navegador."""
    webbrowser.open(VIDEO_URL)

# ---------------------------
# INTERFAZ TKINTER
# ---------------------------

root = tk.Tk()
root.title("Juego de Acertijos 🧠")
root.geometry("500x350")  # aumente un poco la altura para el mensaje extra
root.resizable(False, False)

lbl_titulo = tk.Label(root, text="🎉 JUEGO DE ACERTIJOS 🎉", font=("Arial", 16, "bold"))
lbl_titulo.pack(pady=10)

# Mensaje para ver el video antes de responder
frame_video = tk.Frame(root)
frame_video.pack(pady=5)

lbl_video = tk.Label(
    frame_video, 
    text="📺 Mira este video antes de responder las preguntas:", 
    font=("Arial", 11), 
    fg="darkred"
)
lbl_video.pack(side=tk.LEFT)

btn_video = tk.Button(frame_video, text="Ver video", font=("Arial", 11), command=abrir_video)
btn_video.pack(side=tk.LEFT, padx=5)

lbl_pregunta = tk.Label(root, text="", wraplength=460, justify="center", font=("Arial", 13))
lbl_pregunta.pack(pady=15)

entrada = tk.Entry(root, font=("Arial", 14), width=26)
entrada.pack()

btn = tk.Button(root, text="Responder", font=("Arial", 12), command=verificar)
btn.pack(pady=10)

resultado = tk.StringVar()
lbl_res = tk.Label(root, textvariable=resultado, font=("Arial", 12), fg="blue")
lbl_res.pack(pady=10)

# Cargar primer acertijo
nuevo_acertijo()

root.mainloop()
```

