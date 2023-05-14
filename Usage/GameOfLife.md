## Option 1
class GameOfLife
  def initialize(width, height)
    @width = width
    @height = height
    @grid = Array.new(height) { Array.new(width) { rand(2) } }
  end

  def evolve
    new_grid = Array.new(height) { Array.new(width) }
    height.times do |y|
      width.times do |x|
        alive_neighbors = count_alive_neighbors(x, y)
        if alive?(x, y)
          new_grid[y][x] = (2..3).include?(alive_neighbors) ? 1 : 0
        else
          new_grid[y][x] = alive_neighbors == 3 ? 1 : 0
        end
      end
    end
    @grid = new_grid
  end

  def count_alive_neighbors(x, y)
    count = 0
    (-1..1).each do |dy|
      (-1..1).each do |dx|
        next if dx == 0 && dy == 0
        count += 1 if alive?(x + dx, y + dy)
      end
    end
    count
  end

  def alive?(x, y)
    x %= @width
    y %= @height
    @grid[y][x] == 1
  end

  def to_s
    @grid.map { |row| row.map { |cell| cell == 1 ? 'o' : '.' }.join }.join("\n")
  end
end

## Option 2
require 'matrix'

class GameOfLife
  def initialize(width, height)
    @width = width
    @height = height
    @grid = Matrix.build(height, width) { rand(2) }
  end

  def evolve
    new_grid = Matrix.build(height, width) do |y, x|
      alive_neighbors = count_alive_neighbors(x, y)
      if alive?(x, y)
        (2..3).include?(alive_neighbors) ? 1 : 0
      else
        alive_neighbors == 3 ? 1 : 0
      end
    end
    @grid = new_grid
  end

  def count_alive_neighbors(x, y)
    count = 0
    (-1..1).each do |dy|
      (-1..1).each do |dx|
        next if dx == 0 && dy == 0
        count += 1 if alive?(x + dx, y + dy)
      end
    end
    count
  end

  def alive?(x, y)
    x %= @width
    y %= @height
    @grid[y, x] == 1
  end

  def to_s
    @grid.to_a.map { |row| row.map { |cell| cell == 1 ? 'o' : '.' }.join }.join("\n")
  end
end
