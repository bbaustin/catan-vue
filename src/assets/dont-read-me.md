## TODO

- make body or #app 100vh and flex-col, justify-content evenly
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
