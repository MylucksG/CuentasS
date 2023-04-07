import tkinter.simpledialog as sd
import tkinter as tk
from tkinter import ttk
from tkinter import messagebox

class MainApplication(tk.Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.valores = []
        self.suma = 0
        self.contador = 1
        self.create_widgets()
        self.label5 = tk.Label(self, text="")
        self.label5.grid(row=5, column=1)


    def create_widgets(self):
        self.label1 = tk.Label(self.master, text=f"{self.contador}. Ingrese un valor (o presione enter para finalizar):")
        self.label1.pack()
        self.entry1 = tk.Entry(self.master)
        self.entry1.pack()
        self.button1 = tk.Button(self.master, text="Agregar valor", command=self.add_value)
        self.button1.pack()
        self.label2 = tk.Label(self.master, text="El saldo total es: ")
        self.label2.pack()
        self.label3 = tk.Label(self.master, text=f"{self.suma:.2f}")
        self.label3.pack()
        self.label4 = tk.Label(self.master, text="Valores ingresados:")
        self.label4.pack()
        self.entry1.bind("<Return>", lambda event: self.add_value())

        self.original_valores = [] # Crear el atributo original_valores aquí

        self.button_mostrar_inverso = tk.Button(self.master, text="Mostrar valores en orden inverso", command=self.mostrar_valores_inverso)
        self.button_mostrar_inverso.pack()


        self.button_original = tk.Button(self.master, text="Mostrar valores originales", command=self.show_original_values)
        self.button_original.pack()


        # Crear la tabla con Treeview
        self.table = ttk.Treeview(self.master)
        self.table.pack()
        self.table["columns"] = ("Posición", "Valor")
        self.table.heading("#0", text="")
        self.table.column("#0", width=0, stretch=tk.NO)
        self.table.heading("Posición", text="Posición")
        self.table.column("Posición", anchor=tk.CENTER, width=100)
        self.table.heading("Valor", text="Valor")
        self.table.column("Valor", anchor=tk.CENTER, width=100)

        # Crear botón para eliminar una posición
        self.button_eliminar = tk.Button(self.master, text="Eliminar valor", command=self.delete_value)
        self.button_eliminar.pack()

        # Crear etiqueta para mostrar mensajes
        self.label5 = tk.Label(self.master, text="")
        self.label5.pack()

        self.button2 = tk.Button(self.master, text="Editar valor", command=self.edit_value)
        self.button2.pack()
        self.button3 = tk.Button(self.master, text="Salir", command=self.master.quit)


    def update_values_label(self, message):
        self.label5.configure(text=message)

 
    def edit_value(self):
        respuesta = messagebox.askquestion("Editar valor", "Desea editar algún valor?")
        if respuesta == "yes":
            posicion = sd.askinteger(title="Editar valor", prompt="Ingrese la posición del valor que desea editar:", minvalue=1, maxvalue=len(self.valores))
            if posicion is not None:
                nuevo_valor = sd.askfloat(title="Editar valor", prompt="Ingrese el nuevo valor:")
                if nuevo_valor is not None:
                    diferencia = nuevo_valor - self.valores[posicion-1]
                    self.valores[posicion-1] = nuevo_valor
                    self.suma += diferencia
                    self.label3.configure(text=f"{self.suma:.2f}")
                    self.update_values_label("Valor editado correctamente")       


    def add_value(self):
        
        entrada = self.entry1.get()
        if entrada.strip() == "":
            messagebox.showerror("Error", "Debe ingresar al menos un valor")
        else:
            try:
                valor = float(entrada)
                self.valores.append(valor)
                self.suma += valor
                self.contador += 1
                self.label1.configure(text=f"{self.contador}. Ingrese un valor (o presione enter para finalizar):")
                self.label3.configure(text=f"{self.suma:.2f}")
                self.entry1.delete(0, tk.END)
                self.update_values_table()
            except ValueError:
                messagebox.showerror("Error", "Debe ingresar un número")

        self.original_valores.append(valor)

    def show_original_values(self):
        # Eliminar todos los items en la tabla
        for i in self.table.get_children():
            self.table.delete(i)

        # Agregar cada valor original a la tabla
        for i, valor in enumerate(self.original_valores):
            self.table.insert("", "end", text="", values=(i+1, valor))

        # Actualizar la etiqueta de la suma
        self.label3.configure(text=f"{sum(self.original_valores):.2f}")
        self.update_values_label("Valores originales mostrados")        

    def delete_value(self):
        # Obtener el item seleccionado en la tabla
        selection = self.table.selection()
        if not selection:
            self.update_values_label("Debe seleccionar un valor para eliminar")
            return
        item = self.table.item(selection[0])
        posicion = int(item["values"][0])

        # Eliminar el valor de la lista y actualizar la suma
        valor = self.valores[posicion-1]
        self.valores.pop(posicion-1)
        self.suma -= valor

        # Actualizar la tabla y la etiqueta de la suma
        self.update_values_table()
        self.label3.configure(text=f"{self.suma:.2f}")
        self.update_values_label("Valor eliminado correctamente")

    def mostrar_valores_inverso(self):
        self.update_values_table(reverse=True)
        self.update_values_label("Valores mostrados en orden inverso")
        

        
    def update_values_table(self, reverse=False):
        # Eliminar todos los items en la tabla
        for i in self.table.get_children():
            self.table.delete(i)

        # Obtener la lista de valores en el orden adecuado
        valores = self.valores[::-1] if reverse else self.valores

        # Agregar cada valor ingresado a la tabla
        for i, valor in enumerate(valores):
            posicion = i+1 if not reverse else len(self.valores) - i
            self.table.insert("", "end", text="", values=(posicion, valor))


                            
root = tk.Tk()
root.title("CUENTAS") 
app = MainApplication(master=root)
app.mainloop()
