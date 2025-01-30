import tkinter as tk

class MapGUI:
    def __init__(self, root, map_data, tile_size=50):
        """
        Initialize the GUI map.
        :param root: Tkinter root window
        :param map_data: 2D list representing the map
        :param tile_size: Size of each grid tile
        """
        self.root = root
        self.map_data = map_data
        self.tile_size = tile_size
        self.rows = len(map_data)
        self.cols = len(map_data[0])
        self.canvas = tk.Canvas(root, width=self.cols * tile_size, height=self.rows * tile_size)
        self.canvas.pack()

        self.player_pos = [0, 0]  # Starting position of the player (x, y)
        self.player = None  # Initialize player object
        self.draw_map()
        self.draw_player()

        # Bind arrow keys for movement
        root.bind("<Up>", lambda event: self.move_player(0, -1))
        root.bind("<Down>", lambda event: self.move_player(0, 1))
        root.bind("<Left>", lambda event: self.move_player(-1, 0))
        root.bind("<Right>", lambda event: self.move_player(1, 0))

    def draw_map(self):
        """Draw the map on the canvas."""
        for y in range(self.rows):
            for x in range(self.cols):
                color = "white" if self.map_data[y][x] == 0 else "gray"
                self.canvas.create_rectangle(
                    x * self.tile_size, y * self.tile_size,
                    (x + 1) * self.tile_size, (y + 1) * self.tile_size,
                    fill=color, outline="black"
                )

    def draw_player(self):
        """Draw the player on the canvas."""
        x, y = self.player_pos
        self.player = self.canvas.create_oval(
            x * self.tile_size + 5, y * self.tile_size + 5,
            (x + 1) * self.tile_size - 5, (y + 1) * self.tile_size - 5,
            fill="blue"
        )

    def move_player(self, dx, dy):
        """
        Move the player if the target position is walkable.
        :param dx: Change in x-coordinate
        :param dy: Change in y-coordinate
        """
        new_x = self.player_pos[0] + dx
        new_y = self.player_pos[1] + dy

        # Check if the move is within bounds and not colliding
        if 0 <= new_x < self.cols and 0 <= new_y < self.rows and self.map_data[new_y][new_x] == 0:
            self.player_pos = [new_x, new_y]
            self.canvas.move(self.player, dx * self.tile_size, dy * self.tile_size)
        else:
            print(f"Collision detected at ({new_x}, {new_y})")


if __name__ == "__main__":
    # Define a map (0 = walkable, 1 = obstacle)
    map_data = [
        [0, 0, 0, 1, 0],
        [0, 1, 0, 1, 0],
        [0, 0, 0, 0, 0],
        [1, 1, 1, 0, 1],
        [0, 0, 0, 0, 0],
    ]

    # Create the Tkinter window
    root = tk.Tk()
    root.title("Map Collision GUI")
    app = MapGUI(root, map_data)
    root.mainloop()
