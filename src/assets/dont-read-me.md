## TODO

### For v.1:
- Improve forms
  - autofocus text boxes 
    - ALMOST! The first one isn't being focused. It's not there when I wanna focus it (ERROR: this.$refs.nameInput[0] does not exist)
  - when you tab through, but then tab BACK to the color list, it focuses the final color but doesn't underline it. No clue about this one haha


- DONE: In your label... it shoulds say the color name, not the hex code
- DONE: Make selected color look better
- DONE: make body or #app 100vh and flex-col, justify-content evenly
- DONE: want color for last roll in GraphNumberBox? If so, pass player[last rolled] to get the color
- media query? 
- filtering rolls by player
- more graphs for d3 practice
  - scatterplot for roll order
  - pie chart



- Deleted from Graph
```
determineColor(rollNumber) {
      for (let i = this.colors.length - 1; i < 0; i--) {
        if (rollNumber % i === 0) {
          return this.colors[i]
        }
      }
    }
```
