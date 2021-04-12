# \[Medium\] Robot Bounded In Circle

`Amazon Tag 265`

## [\[Medium\] Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle/)       \(1040/309\)

On an infinite plane, a robot initially stands at `(0, 0)` and faces north. The robot can receive one of three instructions:

* `"G"`: go straight 1 unit;
* `"L"`: turn 90 degrees to the left;
* `"R"`: turn 90 degrees to the right.

The robot performs the `instructions` given in order, and repeats them forever.

Return `true` if and only if there exists a circle in the plane such that the robot never leaves the circle.

```text
Ex1:
Input: instructions = "GGLLGG"
Output: true
Explanation: The robot moves from (0,0) to (0,2), turns 180 degrees, and then returns to (0,0).
When repeating these instructions, the robot remains in the circle of radius 2 centered at the origin.

Ex2:
Input: instructions = "GG"
Output: false
Explanation: The robot moves north indefinitely.

Ex3:
Input: instructions = "GL"
Output: true
Explanation: The robot moves from (0, 0) -> (0, 1) -> (-1, 1) -> (-1, 0) -> (0, 0) -> ...
```

### 1. instructions\*4 法則 ＋ Directions：O\(N\) / O\(N\)

```python
def isRobotBounded(self, instructions: str) -> bool:
    
    # robot should be able to go back to original, (0, 0).
    ins = list(instructions)*4  

    # (x,y) to record where it is at right now.
    x, y = 0, 0
    DIRECTIONS = [(1,0), (0,1), (-1,0), (0,-1)]  # East, North, West, South 
    dir_pointer = 0                              # direction pointer       
    direction = DIRECTIONS[dir_pointer]
    
#        print('direction/x/y')
    for idx in range(len(ins)):
        if ins[idx] == 'G':
            (x,y) = self.move(direction, x, y)
#                print(direction, x, y)
            
        elif ins[idx] == 'L':
            dir_pointer += 1
            if dir_pointer > 3:
                dir_pointer = 0
            direction = DIRECTIONS[dir_pointer]
#                print(direction)
        elif ins[idx] == 'R':
            dir_pointer -= 1
            if dir_pointer < 0:
                dir_pointer = 3
            direction = DIRECTIONS[dir_pointer]
    
    # check if the robot goes back to original.
    if (x,y) == (0,0):
        return True
    return False
        
        
def move(self, direction, x, y):
    DIRECTIONS = [(1,0), (0,1), (-1,0), (0,-1)] 
    if direction == DIRECTIONS[0]:
        return (x+1,y)
    elif direction == DIRECTIONS[1]:
        return (x,y+1)
    elif direction == DIRECTIONS[2]:
        return (x-1,y)
    else:
        return (x,y-1)
```

