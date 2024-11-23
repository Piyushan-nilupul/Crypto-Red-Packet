import random
import tkinter as tk

class LuckyWheel:
    def __init__(self, master):
        self.master = master
        self.master.title("Lucky Wheel")
        
        self.canvas = tk.Canvas(master, width=400, height=400)
        self.canvas.pack()
        
        self.segments = ["$100", "$200", "$300", "$400", "$500", "Lose a Turn", "Bankrupt"]
        self.angle = 0
        self.wheel = self.create_wheel()
        
        self.spin_button = tk.Button(master, text="Spin the Wheel", command=self.spin)
        self.spin_button.pack()
        
    def create_wheel(self):
        wheel = []
        for i, segment in enumerate(self.segments):
            start_angle = i * (360 / len(self.segments))
            end_angle = (i + 1) * (360 / len(self.segments))
            wheel.append((start_angle, end_angle, segment))
            self.canvas.create_arc(50, 50, 350, 350, start=start_angle, extent=(end_angle - start_angle), fill=self.get_color(i), outline="black")
            self.canvas.create_text(200, 200, text=segment, angle=start_angle + (end_angle - start_angle) / 2, fill="white")
        return wheel

    def get_color(self, index):
        colors = ["red", "blue", "green", "yellow", "orange", "purple", "pink"]
        return colors[index % len(colors)]

    def spin(self):
        self.angle = random.randint(0, 360)
        self.canvas.delete("all")
        self.create_wheel()
        self.canvas.create_arc(50, 50, 350, 350, start=self.angle, extent=360, fill="black", outline="black")

if __name__ == "__main__":
    root = tk.Tk()
    lucky_wheel = LuckyWheel(root)
    root.mainloop()
