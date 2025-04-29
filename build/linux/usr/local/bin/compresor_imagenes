#!/usr/bin/env python3
import os
from tkinter import Tk, Label, Button, Scale, HORIZONTAL, filedialog, messagebox, Entry, StringVar, simpledialog
from PIL import Image

def seleccionar_origen():
    carpeta = filedialog.askdirectory(title="Selecciona carpeta de origen")
    if carpeta:
        entrada_origen.set(carpeta)

def seleccionar_destino():
    carpeta = filedialog.askdirectory(title="Selecciona carpeta de destino")
    if carpeta:
        entrada_destino.set(carpeta)

def crear_nueva_carpeta():
    carpeta_base = filedialog.askdirectory(title="Selecciona dónde crear nueva carpeta")
    if carpeta_base:
        nombre_nueva_carpeta = simpledialog.askstring("Nueva carpeta", "Ingresa el nombre de la nueva carpeta:")
        if nombre_nueva_carpeta:
            nueva_carpeta = os.path.join(carpeta_base, nombre_nueva_carpeta)
            try:
                os.makedirs(nueva_carpeta, exist_ok=True)
                messagebox.showinfo("Carpeta creada", f"Carpeta creada: {nueva_carpeta}")
                if messagebox.askyesno("Usar como destino", "¿Deseas usar esta carpeta como destino?"):
                    entrada_destino.set(nueva_carpeta)
            except Exception as e:
                messagebox.showerror("Error", f"No se pudo crear la carpeta.\n{e}")

def comprimir_imagenes():
    origen = entrada_origen.get()
    destino = entrada_destino.get()
    calidad = slider_calidad.get()

    if not origen or not destino:
        messagebox.showwarning("Faltan datos", "Selecciona carpetas de origen y destino.")
        return

    os.makedirs(destino, exist_ok=True)
    procesadas = 0

    for carpeta_actual, _, archivos in os.walk(origen):
        for archivo in archivos:
            if archivo.lower().endswith(('.jpg', '.jpeg', '.png')):
                ruta_img = os.path.join(carpeta_actual, archivo)
                rel_path = os.path.relpath(carpeta_actual, origen)
                carpeta_destino_actual = os.path.join(destino, rel_path)
                os.makedirs(carpeta_destino_actual, exist_ok=True)
                ruta_out = os.path.join(carpeta_destino_actual, archivo)

                try:
                    img = Image.open(ruta_img)
                    if img.mode in ("RGBA", "P"):
                        img = img.convert("RGB")
                    if archivo.lower().endswith(('.jpg', '.jpeg')):
                        img.save(ruta_out, "JPEG", quality=calidad, optimize=True)
                    else:
                        img.save(ruta_out, "PNG", optimize=True)
                    procesadas += 1
                except Exception as e:
                    print(f"Error con {archivo}: {e}")

    messagebox.showinfo("Proceso terminado", f"Se comprimieron {procesadas} imágenes.")
    entrada_origen.set("")
    entrada_destino.set("")
    slider_calidad.set(75)

def comprimir_imagen_individual():
    archivo = filedialog.askopenfilename(title="Selecciona una imagen", filetypes=[("Imágenes", "*.jpg *.jpeg *.png")])
    if archivo:
        carpeta_destino = filedialog.askdirectory(title="Selecciona carpeta de destino")
        if carpeta_destino:
            calidad = slider_calidad.get()
            try:
                img = Image.open(archivo)
                if img.mode in ("RGBA", "P"):
                    img = img.convert("RGB")
                nombre_archivo = os.path.basename(archivo)
                ruta_out = os.path.join(carpeta_destino, nombre_archivo)
                if archivo.lower().endswith(('.jpg', '.jpeg')):
                    img.save(ruta_out, "JPEG", quality=calidad, optimize=True)
                else:
                    img.save(ruta_out, "PNG", optimize=True)
                messagebox.showinfo("Éxito", f"Imagen comprimida en:\n{ruta_out}")
            except Exception as e:
                messagebox.showerror("Error", str(e))

# --- GUI ---
root = Tk()
root.title("Compresor de Imágenes")
root.geometry("370x430")

entrada_origen = StringVar()
entrada_destino = StringVar()

Label(root, text="Carpeta de origen").grid(row=0, column=0, padx=10, pady=5, sticky="w")
Label(root, text="Carpeta de destino").grid(row=0, column=1, padx=10, pady=5, sticky="w")

Entry(root, textvariable=entrada_origen).grid(row=1, column=0, padx=10, sticky="ew")
Entry(root, textvariable=entrada_destino).grid(row=1, column=1, padx=10, sticky="ew")

Button(root, text="Seleccionar origen", command=seleccionar_origen).grid(row=2, column=0, padx=10, pady=5, sticky="ew")
Button(root, text="Crear nueva carpeta", command=crear_nueva_carpeta).grid(row=2, column=1, padx=10, pady=5, sticky="ew")
Button(root, text="Seleccionar destino", command=seleccionar_destino).grid(row=3, column=1, padx=10, pady=5, sticky="ew")

Label(root, text="Compresión en (%)").grid(row=4, column=0, columnspan=2, pady=15)
slider_calidad = Scale(root, from_=10, to=100, orient=HORIZONTAL)
slider_calidad.set(75)
slider_calidad.grid(row=5, column=0, columnspan=2, pady=5)

Button(root, text="Comprimir imágenes de carpeta", command=comprimir_imagenes, bg="#4CAF50", fg="white", width=25).grid(row=6, column=0, columnspan=2, pady=10)
Button(root, text="Comprimir imagen individual", command=comprimir_imagen_individual, bg="#2196F3", fg="white", width=25).grid(row=7, column=0, columnspan=2, pady=10)

root.mainloop()

