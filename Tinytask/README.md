# 🖱️ Macro Recorder GUI

Una herramienta de automatización ligera desarrollada en Python diseñada para capturar, gestionar y reproducir acciones del sistema en tiempo real. Permite registrar flujos de trabajo complejos combinando la interacción con el mouse y el teclado de forma precisa, ofreciendo la posibilidad de almacenar las secuencias en archivos estructurados y ejecutarlas en bucles infinitos con control inmediato mediante atajos globales.

---

## 📋 Características Principales

* **Interfaz Gráfica Intuitiva:** Desarrollada con Tkinter, fácil de usar y sin dependencias pesadas.
* **Reproducción Indefinida (Bucle):** Ejecución continua del flujo de trabajo hasta que el usuario decida frenarlo.
* **Atajos Globales (Hotkeys):** Control por teclado (`F8` y `F9`) funcional incluso si la ventana está minimizada o en segundo plano.
* **Persistencia de Datos:** Exportación e importación de rutinas grabadas mediante formato `.json`.
* **Interrupción Inmediata:** Optimización de pausas internas para detener la reproducción al instante sin congelar la interfaz.

---

## 🛠️ Requisitos Previos e Instalación

### Requisitos del Sistema
* **Lenguaje:** Python 3.8 o superior instalado en el sistema.

---

### Instalación en Windows

1. **Paso uno**
   Crear una carpeta con el nombre "Tinytask.py", dentro de la carpeta creamos un archivo de texto y pegamos el siguiente codigo


---

import time
import threading
import json
import os
import tkinter as tk
from tkinter import messagebox
from pynput import mouse, keyboard

FILE = "macro.json"

class MacroAppGUI:
    def __init__(self, root):
        self.root = root
        self.root.title("Macro Recorder - Bucle Infinito")
        self.root.geometry("380x320")
        self.root.resizable(False, False)

        self.events = []
        self.recording = False
        self.playing = False
        self.start_time = 0.0
        self.last_time = 0.0

        # Controladores de pynput
        self.mouse_ctrl = mouse.Controller()
        self.key_ctrl = keyboard.Controller()

        # Componentes de la Interfaz
        self.lbl_status = tk.Label(
            root, text="Estado: Inactivo", font=("Arial", 12, "bold"), fg="black"
        )
        self.lbl_status.pack(pady=12)

        self.lbl_info = tk.Label(
            root, text="Atajos globales:\nF8: Grabar/Detener | F9: Reproducir/Detener",
            font=("Arial", 9), fg="gray"
        )
        self.lbl_info.pack(pady=2)

        # Botones
        self.btn_record = tk.Button(
            root, text="🔴 Grabar (F8)", width=25, font=("Arial", 10), command=self.toggle_record
        )
        self.btn_record.pack(pady=5)

        self.btn_play = tk.Button(
            root, text="▶ Reproducir Bucle (F9)", width=25, font=("Arial", 10), command=self.toggle_play
        )
        self.btn_play.pack(pady=5)

        self.btn_clear = tk.Button(
            root, text="🗑 Borrar Macro", width=25, font=("Arial", 10), command=self.clear_macro
        )
        self.btn_clear.pack(pady=5)

        frame_files = tk.Frame(root)
        frame_files.pack(pady=10)

        self.btn_save = tk.Button(
            frame_files, text="💾 Guardar", width=11, font=("Arial", 10), command=self.save_macro
        )
        self.btn_save.pack(side=tk.LEFT, padx=5)

        self.btn_load = tk.Button(
            frame_files, text="📂 Cargar", width=11, font=("Arial", 10), command=self.load_macro
        )
        self.btn_load.pack(side=tk.LEFT, padx=5)

        # Listeners de fondo para capturar eventos y atajos
        self.mouse_listener = mouse.Listener(
            on_move=self.on_mouse_move,
            on_click=self.on_mouse_click,
            on_scroll=self.on_mouse_scroll
        )
        self.key_listener = keyboard.Listener(
            on_press=self.on_key_down,
            on_release=self.on_key_up
        )
        self.hotkey_listener = keyboard.Listener(
            on_press=self.on_hotkey_press
        )

        self.mouse_listener.start()
        self.key_listener.start()
        self.hotkey_listener.start()

        self.root.protocol("WM_DELETE_WINDOW", self.on_close)

    def add_event(self, kind, data):
        if not self.recording:
            return
        now = time.perf_counter()
        delay = now - (self.last_time if self.last_time else self.start_time)
        self.events.append({"type": kind, "delay": round(delay, 6), "data": data})
        self.last_time = now

    # Interceptores de Mouse
    def on_mouse_move(self, x, y):
        self.add_event("move", {"x": x, "y": y})

    def on_mouse_click(self, x, y, button, pressed):
        if pressed:
            self.add_event("click", {"x": x, "y": y, "button": button.name})

    def on_mouse_scroll(self, x, y, dx, dy):
        self.add_event("scroll", {"dx": dx, "dy": dy})

    # Interceptores de Teclado
    def _key_data(self, k):
        if isinstance(k, keyboard.KeyCode):
            return {"kind": "char", "value": k.char}
        return {"kind": "special", "value": k.name}

    def on_key_down(self, k):
        if k not in (keyboard.Key.f8, keyboard.Key.f9, keyboard.Key.esc):
            self.add_event("key_down", self._key_data(k))

    def on_key_up(self, k):
        if k not in (keyboard.Key.f8, keyboard.Key.f9, keyboard.Key.esc):
            self.add_event("key_up", self._key_data(k))

    def _key_from_data(self, d):
        if d["kind"] == "char":
            return keyboard.KeyCode.from_char(d["value"])
        return getattr(keyboard.Key, d["value"], None)

    # Atajos de teclado (F8 y F9)
    def on_hotkey_press(self, k):
        if k == keyboard.Key.f8:
            self.root.after(0, self.toggle_record)
        elif k == keyboard.Key.f9:
            self.root.after(0, self.toggle_play)

    # Lógica de Grabación
    def toggle_record(self):
        if self.playing:
            self.stop_play()

        if self.recording:
            self.recording = False
            self.last_time = 0.0
            self.lbl_status.config(
                text=f"Estado: {len(self.events)} eventos grabados", fg="black"
            )
            self.btn_record.config(text="🔴 Grabar (F8)")
            self.btn_play.config(state=tk.NORMAL)
            self.btn_clear.config(state=tk.NORMAL)
        else:
            self.events.clear()
            self.recording = True
            self.start_time = time.perf_counter()
            self.last_time = 0.0
            self.lbl_status.config(text="Estado: Grabando...", fg="red")
            self.btn_record.config(text="⏹ Detener Grabación (F8)")
            self.btn_play.config(state=tk.DISABLED)
            self.btn_clear.config(state=tk.DISABLED)

    # Lógica de Reproducción
    def toggle_play(self):
        if self.recording:
            return
        if self.playing:
            self.stop_play()
        else:
            if not self.events:
                messagebox.showwarning("Advertencia", "No hay eventos para reproducir.")
                return
            self.playing = True
            self.lbl_status.config(text="Estado: Reproduciendo en bucle...", fg="green")
            self.btn_record.config(state=tk.DISABLED)
            self.btn_play.config(text="⏹ Detener Bucle (F9)")
            self.btn_clear.config(state=tk.DISABLED)
            threading.Thread(target=self._play_loop, daemon=True).start()

    def stop_play(self):
        if self.playing:
            self.playing = False
            self.lbl_status.config(text="Estado: Deteniendo...", fg="orange")

    def _play_loop(self):
        try:
            while self.playing:
                for e in self.events:
                    if not self.playing:
                        break

                    delay = max(0, e["delay"])
                    step = 0.05
                    while delay > 0 and self.playing:
                        time.sleep(min(delay, step))
                        delay -= step

                    if not self.playing:
                        break

                    d = e["data"]
                    if e["type"] == "move":
                        self.mouse_ctrl.position = (d["x"], d["y"])
                    elif e["type"] == "click":
                        self.mouse_ctrl.position = (d["x"], d["y"])
                        self.mouse_ctrl.click(getattr(mouse.Button, d["button"]))
                    elif e["type"] == "scroll":
                        self.mouse_ctrl.scroll(d["dx"], d["dy"])
                    elif e["type"] == "key_down":
                        key = self._key_from_data(d)
                        if key: self.key_ctrl.press(key)
                    elif e["type"] == "key_up":
                        key = self._key_from_data(d)
                        if key: self.key_ctrl.release(key)
        finally:
            self.playing = False
            self.root.after(0, self._reset_play_ui)

    def _reset_play_ui(self):
        self.lbl_status.config(text=f"Estado: {len(self.events)} eventos listos", fg="black")
        self.btn_record.config(state=tk.NORMAL)
        self.btn_play.config(text="▶ Reproducir Bucle (F9)", state=tk.NORMAL)
        self.btn_clear.config(state=tk.NORMAL)

    # Funciones secundarias
    def clear_macro(self):
        if not self.recording and not self.playing:
            self.events.clear()
            self.lbl_status.config(text="Estado: Macro borrada", fg="black")

    def save_macro(self):
        if self.events:
            with open(FILE, "w", encoding="utf-8") as f:
                json.dump(self.events, f, indent=2)
            messagebox.showinfo("Guardado", f"Macro guardada en {os.path.abspath(FILE)}")
        else:
            messagebox.showwarning("Advertencia", "No hay macro para guardar.")

    def load_macro(self):
        try:
            with open(FILE, encoding="utf-8") as f:
                self.events = json.load(f)
            self.lbl_status.config(text=f"Estado: {len(self.events)} eventos cargados", fg="black")
            messagebox.showinfo("Cargado", f"Se cargaron {len(self.events)} eventos.")
        except FileNotFoundError:
            messagebox.showerror("Error", "No existe el archivo macro.json")

    def on_close(self):
        self.playing = False
        self.recording = False
        self.mouse_listener.stop()
        self.key_listener.stop()
        self.hotkey_listener.stop()
        self.root.destroy()

if __name__ == "__main__":
    root = tk.Tk()
    app = MacroAppGUI(root)
    root.mainloop()
    
---

Le damos a guardar como y en el tipo de archivo seleccionamos "todos los archivos"

En esa misma carpeta hacemos click derecho y abrimos una terminal, y ahi procedemos a instalar las dependencias con el siguiente comando: pip install pynput

y por ultimo lo ejecutamos con el comando : python .\macro_recorder_gui.py

📄 Licencia
Este proyecto se distribuye bajo la licencia MIT. Puedes usarlo, modificarlo y distribuirlo libremente.
