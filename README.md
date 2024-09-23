# semana14
Aplicacion de GUI

import tkinter as tk
from tkinter import messagebox, ttk
import datetime

class AgendaPersonal:
    def __init__(self, root):
        self.root = root
        self.root.title("Agenda Personal")
        self.root.geometry("400x400")
        self.root.config(bg="#f0f0f0")

        # Frame para la lista de eventos
        self.frame_lista = tk.Frame(self.root, bg="#d9d9d9")
        self.frame_lista.pack(pady=10)

        # TreeView para mostrar eventos
        self.tree = ttk.Treeview(self.frame_lista, columns=("fecha", "hora", "descripcion"), show='headings')
        self.tree.heading("fecha", text="Fecha")
        self.tree.heading("hora", text="Hora")
        self.tree.heading("descripcion", text="Descripción")
        self.tree.pack()

        # Frame para entrada de datos
        self.frame_entrada = tk.Frame(self.root, bg="#d9d9d9")
        self.frame_entrada.pack(pady=10)

        # Etiquetas y campos de entrada
        tk.Label(self.frame_entrada, text="Fecha (YYYY-MM-DD)", bg="#d9d9d9").grid(row=0, column=0)
        self.entry_fecha = tk.Entry(self.frame_entrada)
        self.entry_fecha.grid(row=0, column=1)

        tk.Label(self.frame_entrada, text="Hora (HH:MM)", bg="#d9d9d9").grid(row=1, column=0)
        self.entry_hora = tk.Entry(self.frame_entrada)
        self.entry_hora.grid(row=1, column=1)

        tk.Label(self.frame_entrada, text="Descripción", bg="#d9d9d9").grid(row=2, column=0)
        self.entry_descripcion = tk.Entry(self.frame_entrada)
        self.entry_descripcion.grid(row=2, column=1)

        # Botones
        self.boton_agregar = tk.Button(self.root, text="Agregar Evento", command=self.agregar_evento, bg="#4A90E2", fg="white")
        self.boton_agregar.pack(pady=5)

        self.boton_eliminar = tk.Button(self.root, text="Eliminar Evento Seleccionado", command=self.eliminar_evento, bg="#E94E77", fg="white")
        self.boton_eliminar.pack(pady=5)

        self.boton_salir = tk.Button(self.root, text="Salir", command=self.root.quit, bg="#FF5722", fg="white")
        self.boton_salir.pack(pady=5)

    def agregar_evento(self):
        """Agrega un evento a la lista."""
        fecha = self.entry_fecha.get()
        hora = self.entry_hora.get()
        descripcion = self.entry_descripcion.get()

        if fecha and hora and descripcion:
            try:
                # Validar la fecha y la hora
                datetime.datetime.strptime(fecha, "%Y-%m-%d")
                datetime.datetime.strptime(hora, "%H:%M")
                # Agregar evento al TreeView
                self.tree.insert("", "end", values=(fecha, hora, descripcion))
                self.entry_fecha.delete(0, tk.END)
                self.entry_hora.delete(0, tk.END)
                self.entry_descripcion.delete(0, tk.END)
            except ValueError:
                messagebox.showerror("Error", "Formato de fecha o hora incorrecto.")
        else:
            messagebox.showwarning("Advertencia", "Por favor, complete todos los campos.")

    def eliminar_evento(self):
        """Elimina el evento seleccionado del TreeView."""
        seleccionado = self.tree.selection()
        if seleccionado:
            if messagebox.askyesno("Confirmar", "¿Estás seguro de que quieres eliminar este evento?"):
                self.tree.delete(seleccionado)
        else:
            messagebox.showwarning("Advertencia", "Por favor, selecciona un evento para eliminar.")

if __name__ == "__main__":
    root = tk.Tk()
    app = AgendaPersonal(root)
    root.mainloop()
    
