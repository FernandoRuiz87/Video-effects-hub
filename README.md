# 🎬 Video Effects Hub

Sistema distribuido de procesamiento de video que aplica efectos visuales de forma paralela mediante una arquitectura **Cliente – Broker – Nodo**.

---

## 📐 Arquitectura

```
┌─────────────┐        ┌─────────────┐        ┌─────────────┐
│   Cliente   │ ──────▶│    Broker   │ ──────▶│    Nodo     │
│  (GUI/TK)   │◀────── │  (Servidor) │◀────── │ (Procesado) │
└─────────────┘        └─────────────┘        └─────────────┘
```

| Componente | Archivo      | Descripción                                                      |
|------------|--------------|------------------------------------------------------------------|
| **Cliente**| `Cliente.py` | GUI con drag & drop para enviar videos al Broker                 |
| **Broker** | `Broker.py`  | Servidor central que divide el video y lo distribuye a los Nodos |
| **Nodo**   | `Nodo.py`    | Recibe un segmento, aplica el efecto y lo devuelve al Broker     |

---

## 🔄 Flujo de trabajo

1. El **Cliente** selecciona un video (.mp4, .avi, .mov, .mkv) y lo envía al **Broker**.
2. El **Broker** divide el video en tantos segmentos como **Nodos** estén conectados.
3. Cada **Nodo** recibe su segmento, aplica el efecto de video configurado y lo devuelve.
4. El **Broker** une todos los segmentos procesados en un video final.
5. El **Cliente** descarga el video resultante y lo reproduce automáticamente.

---

## 🎨 Efectos disponibles

| # | Efecto              | Descripción                                 |
|---|---------------------|---------------------------------------------|
| 1 | Escala de grises    | Convierte los fotogramas a blanco y negro   |
| 2 | Invertir colores    | Aplica negativo a cada fotograma            |
| 3 | Efecto sepia        | Tonalidad cálida estilo fotografía antigua  |
| 4 | Efecto espejo       | Refleja horizontalmente cada fotograma      |
| 5 | Grises y bordes     | Combina escala de grises con Canny edges    |

---

## 📋 Requisitos previos

- Python **3.10+** (se requiere `match/case`)
- Windows (la GUI utiliza `windll` y `windows-curses`)

---

## 📦 Instalación de dependencias

```bash
pip install opencv-python PyQt5 tkinterdnd2 windows-curses numpy
```

> Las librerías estándar (`socket`, `threading`, `uuid`, `os`, `time`, `datetime`) vienen incluidas con Python.

---

## ⚙️ Configuración

Edita el archivo `helpers/env.py` para definir el host y el puerto del Broker:

```python
# helpers/env.py
BROKER_HOST = "127.0.0.1"
BROKER_PORT = 5000
```

---

## 🚀 Ejecución

> **Orden recomendado:** Broker → Nodo(s) → Cliente

### 1. Iniciar el Broker

```bash
python Broker.py
```

### 2. Iniciar uno o más Nodos

Abre una terminal por cada Nodo que desees conectar. Al iniciar, el Nodo pedirá que selecciones el efecto a aplicar.

```bash
python Nodo.py
```

### 3. Iniciar el Cliente

```bash
python Cliente.py
```

Se abrirá la interfaz gráfica. Arrastra tu video o usa el selector de archivos y haz clic en **Enviar**.

---

## 🗂️ Estructura del proyecto

```
Video-effects-hub/
├── Cliente.py          # Interfaz gráfica y lógica del cliente
├── Broker.py           # Servidor central de distribución
├── Nodo.py             # Nodo de procesamiento de video
├── helpers/
│   ├── env.py          # Configuración de host y puerto
│   └── Colores.py      # Constantes ANSI para la consola
├── images/             # Iconos e imágenes de la GUI
├── fonts/              # Fuentes tipográficas
└── .gitignore
```

---

## 📁 Archivos generados en tiempo de ejecución

| Ruta                                              | Contenido                              |
|---------------------------------------------------|----------------------------------------|
| `Broker_files/<video_id>/SinProcesar/`            | Video original y segmentos sin procesar|
| `Broker_files/<video_id>/Procesado/`              | Segmentos procesados y video final     |
| `Nodos/NODO-<uuid>/<video_id>/`                   | Copia local del segmento en cada nodo  |
| `video_procesado.mp4`                             | Video final en el directorio del cliente|

---

## 🛠️ Tecnologías utilizadas

- **Python 3.10+** — Lenguaje principal
- **OpenCV (`cv2`)** — División, procesamiento y unión de video
- **Tkinter + TkinterDnD2** — Interfaz gráfica con drag & drop
- **Sockets TCP** — Comunicación entre componentes
- **Threading** — Concurrencia en el Broker para múltiples nodos

---

## 👥 Autores

Proyecto desarrollado como parte de la materia **Sistemas Distribuidos**.
