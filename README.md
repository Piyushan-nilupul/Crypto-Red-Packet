import tkinter as tk
import random
from math import pi, cos, sin

# Lucky Wheel එක සඳහා GUI එක නිර්මාණය කිරීම
class LuckyWheel:
    def __init__(self, root):
        self.root = root
        self.root.title("Lucky Wheel")
        self.canvas = tk.Canvas(root, width=400, height=400)
        self.canvas.pack()

        # කොටස් 8ක් නිර්මාණය කිරීම
        self.segments = [
            {"label": "Prize 1", "color": "#FF5733"},
            {"label": "Prize 2", "color": "#33FF57"},
            {"label": "Prize 3", "color": "#3357FF"},
            {"label": "Prize 4", "color": "#FF33A8"},
            {"label": "Prize 5", "color": "#A833FF"},
            {"label": "Prize 6", "color": "#FFA833"},
            {"label": "Prize 7", "color": "#33FFA8"},
            {"label": "Prize 8", "color": "#FF5733"}
        ]
        self.draw_wheel()

        # Button එකක් නිර්මාණය කරන්න
        self.spin_button = tk.Button(root, text="Spin", command=self.spin_wheel)
        self.spin_button.pack(pady=20)

    def draw_wheel(self):
        self.canvas.delete("all")
        center_x, center_y = 200, 200
        radius = 150
        angle_per_segment = 2 * pi / len(self.segments)

        for i, segment in enumerate(self.segments):
            start_angle = i * angle_per_segment
            end_angle = (i + 1) * angle_per_segment

            x0 = center_x + radius * cos(start_angle)
            y0 = center_y - radius * sin(start_angle)
            x1 = center_x + radius * cos(end_angle)
            y1 = center_y - radius * sin(end_angle)

            self.canvas.create_arc(
                center_x - radius, center_y - radius, 
                center_x + radius, center_y + radius,
                start=start_angle * 180 / pi,
                extent=angle_per_segment * 180 / pi,
                fill=segment["color"], outline="black"
            )

            # Label එකක් එක් කරන්න
            mid_angle = (start_angle + end_angle) / 2
            label_x = center_x + (radius / 2) * cos(mid_angle)
            label_y = center_y - (radius / 2) * sin(mid_angle)
            self.canvas.create_text(label_x, label_y, text=segment["label"], font=("Helvetica", 10))

    def spin_wheel(self):
        # Randomව කුමන කැල්ලක් ජයග්‍රහණයද යන්න තෝරාගන්න
        winner = random.choice(self.segments)
        tk.messagebox.showinfo("Congratulations!", f"You won: {winner['label']}")

# Main window එක ආරම්භ කිරීම
if __name__ == "__main__":
    root = tk.Tk()
    app = LuckyWheel(root)
    root.mainloop()
