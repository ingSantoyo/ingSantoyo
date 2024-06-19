import turtle  # Importa la biblioteca turtle para gráficos
import time    # Importa la biblioteca time para manejar el tiempo
import random  # Importa la biblioteca random para generar posiciones aleatorias

delay = 0.1  # Define el retraso entre las actualizaciones de la pantalla

# Inicializa el marcador
score = 0
high_score = 0

# Configuración de la ventana del juego
window = turtle.Screen()  # Crea una ventana de juego
window.title("Snake Game")  # Establece el título de la ventana
window.bgcolor("black")  # Establece el color de fondo de la ventana a negro
window.setup(width=600, height=600)  # Establece el tamaño de la ventana
window.tracer(0)  # Apaga las actualizaciones automáticas de la pantalla

# Creación de la cabeza de la serpiente
cabeza = turtle.Turtle()  # Crea un objeto turtle para la cabeza de la serpiente
cabeza.speed(0)  # Establece la velocidad de animación a la más rápida
cabeza.shape("square")  # Establece la forma de la cabeza a un cuadrado
cabeza.color("green")  # Establece el color de la cabeza a blanco
cabeza.penup()  # Levanta el lápiz para que no dibuje líneas
cabeza.goto(0, 0)  # Coloca la cabeza en el centro de la pantalla
cabeza.direction = "stop"  # Inicializa la dirección de la cabeza como detenida

# Creación de la comida de la serpiente
comida = turtle.Turtle()  # Crea un objeto turtle para la comida
comida.speed(0)  # Establece la velocidad de animación a la más rápida
comida.shape("circle")  # Establece la forma de la comida a un círculo
comida.color("red")  # Establece el color de la comida a rojo
comida.penup()  # Levanta el lápiz para que no dibuje líneas
comida.goto(0, 100)  # Coloca la comida en una posición inicial

# Lista para almacenar los segmentos del cuerpo de la serpiente
segmentos = []

# Creación del texto del marcador
texto = turtle.Turtle()  # Crea un objeto turtle para el texto del marcador
texto.speed(0)  # Establece la velocidad de animación a la más rápida
texto.color("white")  # Establece el color del texto a blanco
texto.penup()  # Levanta el lápiz para que no dibuje líneas
texto.hideturtle()  # Esconde el objeto turtle (solo se mostrará el texto)
texto.goto(0, 260)  # Coloca el texto en la parte superior de la pantalla
texto.write("Score: 0  High Score: 0", align="center", font=("Courier", 24, "normal"))  # Escribe el marcador inicial

# Funciones para cambiar la dirección de la serpiente
def mover_arriba():
    if cabeza.direction != "down":  # Evita que la serpiente se mueva hacia atrás
        cabeza.direction = "up"  # Cambia la dirección a arriba

def mover_abajo():
    if cabeza.direction != "up":  # Evita que la serpiente se mueva hacia atrás
        cabeza.direction = "down"  # Cambia la dirección a abajo

def mover_izquierda():
    if cabeza.direction != "right":  # Evita que la serpiente se mueva hacia atrás
        cabeza.direction = "left"  # Cambia la dirección a izquierda

def mover_derecha():
    if cabeza.direction != "left":  # Evita que la serpiente se mueva hacia atrás
        cabeza.direction = "right"  # Cambia la dirección a derecha

# Función para mover la cabeza de la serpiente
def mover():
    if cabeza.direction == "up":
        y = cabeza.ycor()  # Obtiene la coordenada y actual
        cabeza.sety(y + 20)  # Incrementa la coordenada y para mover hacia arriba
    if cabeza.direction == "down":
        y = cabeza.ycor()  # Obtiene la coordenada y actual
        cabeza.sety(y - 20)  # Decrementa la coordenada y para mover hacia abajo
    if cabeza.direction == "left":
        x = cabeza.xcor()  # Obtiene la coordenada x actual
        cabeza.setx(x - 20)  # Decrementa la coordenada x para mover hacia la izquierda
    if cabeza.direction == "right":
        x = cabeza.xcor()  # Obtiene la coordenada x actual
        cabeza.setx(x + 20)  # Incrementa la coordenada x para mover hacia la derecha

# Asignación de las funciones de movimiento a las teclas de flecha
window.listen()  # Escucha por eventos del teclado
window.onkey(mover_arriba, "Up")  # Asigna la tecla de flecha arriba a la función mover_arriba
window.onkey(mover_abajo, "Down")  # Asigna la tecla de flecha abajo a la función mover_abajo
window.onkey(mover_izquierda, "Left")  # Asigna la tecla de flecha izquierda a la función mover_izquierda
window.onkey(mover_derecha, "Right")  # Asigna la tecla de flecha derecha a la función mover_derecha

# Bucle principal del juego
while True:
    window.update()  # Actualiza la pantalla

    # Comprobar colisión con los bordes de la ventana
    if cabeza.xcor() > 290 or cabeza.xcor() < -290 or cabeza.ycor() > 290 or cabeza.ycor() < -290:
        time.sleep(1)  # Pausa el juego por 1 segundo
        cabeza.goto(0, 0)  # Restaura la cabeza al centro de la pantalla
        cabeza.direction = "stop"  # Detiene el movimiento de la cabeza

        # Esconde los segmentos del cuerpo de la serpiente
        for segmento in segmentos:
            segmento.goto(1000, 1000)  # Mueve el segmento fuera de la pantalla

        # Limpia la lista de segmentos
        segmentos.clear()

        # Reinicia el marcador
        score = 0
        texto.clear()  # Limpia el texto del marcador
        texto.write("Score: {}  High Score: {}".format(score, high_score), align="center", font=("Courier", 24, "normal"))  # Actualiza el marcador

    # Comprobar colisión con la comida
    if cabeza.distance(comida) < 20:
        # Mueve la comida a una posición aleatoria
        x = random.randint(-290, 290)
        y = random.randint(-290, 290)
        comida.goto(x, y)

        # Añade un nuevo segmento al cuerpo de la serpiente
        nuevo_segmento = turtle.Turtle()
        nuevo_segmento.speed(0)
        nuevo_segmento.shape("square")
        nuevo_segmento.color("grey")
        nuevo_segmento.penup()
        segmentos.append(nuevo_segmento)

        # Aumenta el marcador
        score += 10
        if score > high_score:
            high_score = score  # Actualiza el high score si es necesario
        texto.clear()
        texto.write("Score: {}  High Score: {}".format(score, high_score), align="center", font=("Courier", 24, "normal"))

    # Mueve los segmentos del cuerpo de la serpiente
    for i in range(len(segmentos) - 1, 0, -1):
        x = segmentos[i - 1].xcor()
        y = segmentos[i - 1].ycor()
        segmentos[i].goto(x, y)

    # Mueve el primer segmento al lugar de la cabeza
    if len(segmentos) > 0:
        x = cabeza.xcor()
        y = cabeza.ycor()
        segmentos[0].goto(x, y)

    mover()  # Llama a la función mover para actualizar la posición de la cabeza

    # Comprobar colisión con el cuerpo de la serpiente
    for segmento in segmentos:
        if segmento.distance(cabeza) < 20:
            time.sleep(1)
            cabeza.goto(0, 0)
            cabeza.direction = "stop"

            # Esconde los segmentos del cuerpo de la serpiente
            for segmento in segmentos:
                segmento.goto(1000, 1000)

            # Limpia la lista de segmentos
            segmentos.clear()

            # Reinicia el marcador
            score = 0
            texto.clear()
            texto.write("Score: {}  High Score: {}".format(score, high_score), align="center", font=("Courier", 24, "normal"))

    time.sleep(delay)  # Pausa el juego para controlar la velocidad

window.mainloop()  # Mantiene la ventana abierta
