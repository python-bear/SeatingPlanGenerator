# SeatingPlanGenerator
Generate basic seating plans with Tkinter and Python.
```
import tkinter as tk
from tkinter import messagebox
import random

class DrawTableApp:
    def __init__(self, master, rows, cols, names):
        self.master = master
        self.rows = rows
        self.cols = cols
        self.names = names
        self.cells = {}
        self.init_ui()
        
    def init_ui(self):
        self.canvas = tk.Canvas(self.master, width=600, height=400)
        self.canvas.pack()
        self.draw_grid()
        self.canvas.bind("<Button-1>", self.toggle_cell)
        
        self.generate_button = tk.Button(self.master, text="Generate", command=self.generate_names)
        self.generate_button.pack()

    def draw_grid(self):
        cell_width = 600 // self.cols
        cell_height = 400 // self.rows
        for i in range(self.rows):
            for j in range(self.cols):
                x1 = j * cell_width
                y1 = i * cell_height
                x2 = x1 + cell_width
                y2 = y1 + cell_height
                self.cells[(i, j)] = False  # Initialize cells as empty
                self.canvas.create_rectangle(x1, y1, x2, y2, outline="black", fill="white")

    def toggle_cell(self, event):
        cell_width = 600 // self.cols
        cell_height = 400 // self.rows
        col = event.x // cell_width
        row = event.y // cell_height
        if (row, col) in self.cells:
            self.cells[(row, col)] = not self.cells[(row, col)]
            color = "black" if self.cells[(row, col)] else "white"
            self.canvas.create_rectangle(col * cell_width, row * cell_height, (col + 1) * cell_width, (row + 1) * cell_height, outline="black", fill=color)
        
    def generate_names(self):
        filled_cells = [cell for cell, filled in self.cells.items() if filled]
        if len(filled_cells) > len(self.names):
            messagebox.showerror("Error", "Not enough names for the filled cells")
            return
        
        random.shuffle(self.names)
        for cell in filled_cells:
            name = self.names.pop()
            self.canvas.create_text((cell[1] * 600 // self.cols) + 30, (cell[0] * 400 // self.rows) + 20, text=name, fill="white", font=('Helvetica 15 bold'))
        
if __name__ == '__main__':
    root = tk.Tk()
    root.title("Draw Table and Generate Names")
    names = ["Alice", "Bob", "Charlie", "David", "Eve", "Faythe", "Grace", "Heidi", "Ivan", "Judy"]
    app = DrawTableApp(root, 5, 5, names)
    root.mainloop()
```
thank me later
