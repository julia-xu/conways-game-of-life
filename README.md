# Conway's Game of Life
Ruby code simulating Conway's Game of Life
[Wikipedia Page](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)

## Test the game with Repl
[Repl page of code](https://repl.it/JJW5/12)

```ruby

#Conway's Game of Life
# 1. Any live cell with fewer than two live neighbours dies, as if caused by underpopulation.
# 2. Any live cell with two or three live neighbours lives on to the next generation.
# 3. Any live cell with more than three live neighbours dies, as if by overpopulation.
# 4. Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.

# Note: In this code, 1 represents 'alive' and 0 represents 'dead'.

########################################################################################################

# generates universe given x & y length and stores as 2D array
def generate_universe(x, y)
  universe = Array.new(x) { Array.new(y) }
  
  x.times do |x_coord|
    y.times do |y_coord|
      universe[x_coord][y_coord] = rand(0..1) # randomizes cell as alive or dead
    end
  end
  
  return universe
end

########################################################################################################

def count_neighbors(x_coord, y_coord, matrix)
  count = 0
  
  # creates buffer around matrix to account for edge cases
  buffer_matrix = Array.new(matrix.length + 2) { Array.new(matrix[0].length + 2, nil) } 
    matrix.length.times do |x|
      matrix[0].length.times do |y|
        buffer_matrix[x+1][y+1] = matrix[x][y]
      end
    end
  
  # counts the neighbors around each cell
    (-1..1).each do |i|
      (-1..1).each do |j|
        next if (i == 0 && j == 0)
        count += 1 if buffer_matrix[x_coord + 1 + i][y_coord + 1 + j] == 1
      end
    end
    
    return count
end

########################################################################################################

# simulates generation
def run_generation(matrix)
  next_generation = Array.new(matrix.length) { Array.new(matrix[0].length) } #making new generation matrix same size
  
  matrix.length.times do |x|
    matrix[0].length.times do |y|
      alive_neighbors = count_neighbors(x, y, matrix)
      
      # follows rules
      if alive_neighbors < 2 || alive_neighbors > 3
        next_generation[x][y] = 0
      else
        next_generation[x][y] = 1
      end
      
    end
  end

  return next_generation
end

########################################################################################################

# displays universe prettily as orthogonal grid
def prettify_universe(matrix)
  matrix.length.times do |row|
    puts matrix[row].join(" ")
  end
end

########################################################################################################


generation1 = generate_universe(3, 3) #change size of universe here
prettify_universe(generation1)

puts "-----"

generation2 = run_generation(generation1)
prettify_universe(generation2)

puts "-----"

generation3 = run_generation(generation2)
prettify_universe(generation3)
```
