# Amaze Game Using The Hill Climbing Algorithm

## We will build an amaze game as an example to explain moving or directing robot navigation using the Hill Climbing algorithm and its branches. In this game, you will play the role of a robot trying to reach the goal in a text maze. You have to think smartly to guide the robot through the maze and reach the goal without crashing into the walls.

### Imports and Libraries:
```python
import random
```
- This line imports the `random` module, which is used for generating random numbers. It will be utilized to randomly generate the maze layout.

### MazeGame Class:
```python
class MazeGame:
    def __init__(self, width, height):
        self.width = width
        self.height = height
        self.maze = [[0] * width for _ in range(height)]
        self.robot_position = (0, 0)
        self.goal_position = (width - 1, height - 1)
        self._generate_maze(0.2)
```
- This class represents the maze game.
- `__init__` method initializes the game with a given width and height.
- `width` and `height` represent the dimensions of the maze.
- `maze` is a 2D list representing the maze layout where 0 represents an empty space and 1 represents a wall.
- `robot_position` stores the current position of the robot.
- `goal_position` stores the position of the goal.
- `_generate_maze` method generates the maze layout with a given wall density.

### _generate_maze Method:
```python
def _generate_maze(self, wall_density):
    for y in range(self.height):
        for x in range(self.width):
            if random.random() < wall_density and (x, y) != self.robot_position and (x, y) != self.goal_position:
                self.maze[y][x] = 1
```
- This method generates the maze layout based on a specified wall density.
- It iterates through each cell in the maze and randomly decides whether to place a wall based on the wall density.
- Walls are not placed at the robot's starting position or the goal position.

### print_maze Method:
```python
def print_maze(self):
    for row in self.maze:
        print(''.join(['#' if cell == 1 else ' ' for cell in row]))
```
- This method prints the maze layout in the console.
- It iterates through each row of the maze and prints '#' for walls and ' ' for empty spaces.

### move_robot Method:
```python
def move_robot(self, direction):
    dx, dy = direction
    x, y = self.robot_position
    new_x, new_y = x + dx, y + dy

    if 0 <= new_x < self.width and 0 <= new_y < self.height and self.maze[new_y][new_x] != 1:
        self.robot_position = (new_x, new_y)
```
- This method moves the robot in the maze.
- It takes a `direction` tuple as input representing the movement direction.
- It calculates the new position based on the current position and the direction.
- If the new position is within the maze boundaries and does not collide with a wall, the robot's position is updated.

### is_goal_reached Method:
```python
def is_goal_reached(self):
    return self.robot_position == self.goal_position
```
- This method checks if the robot has reached the goal position.
- It returns `True` if the robot's position matches the goal position, indicating that the goal has been reached.

### hill_climbing Function:
```python
def hill_climbing(game):
    directions = [(0, -1), (0, 1), (-1, 0), (1, 0)]
    while not game.is_goal_reached():
        best_score = float('-inf')
        best_move = None
        for direction in directions:
            game.move_robot(direction)
            score = evaluate_move(game)
            if score > best_score:
                best_score = score
                best_move = direction
            game.move_robot((-direction[0], -direction[1]))
        if best_move:
            game.move_robot(best_move)
```
- This function implements the hill climbing algorithm to guide the robot towards the goal.
- It iteratively moves the robot in the direction that maximizes the evaluation score (distance to the goal).
- The `directions` list contains possible movement directions: Up, Down, Left, Right.
- It continues until the goal is reached (`is_goal_reached()` returns `True`).

### evaluate_move Function:
```python
def evaluate_move(game):
    robot_x, robot_y = game.robot_position
    goal_x, goal_y = game.goal_position
    return -(abs(robot_x - goal_x) + abs(robot_y - goal_y))
```
- This function evaluates a potential move based on the distance from the robot's current position to the goal.
- It returns a negative value since hill climbing aims to maximize the score, and minimizing the distance to the goal is the objective.

### Game Initialization and Execution:
```python
maze_width = 10
maze_height = 10

game = MazeGame(maze_width, maze_height)
game.print_maze()

hill_climbing(game)
print("\nRobot's path:")
game.print_maze()
```
- Set the maze dimensions (`maze_width` and `maze_height`).
- Create an instance of `MazeGame`.
- Print the initial maze layout.
- Apply the hill climbing algorithm to guide the robot.
- Print the final maze layout showing the path taken by the robot.
