---
# permalink: /about/
layout: single
title: "Snake"
classes: wide
header:
  image: /assets/images/teaser/teaser.png
  caption: "Image credit: [**Yun**](http://yun-vis.net)"  
last_modified_at: 2023-03-06
---

# Console Game Snake

```csharp
using System;
using System.Collections.Generic;

/*
ConsoleGame: My first console game Snake
*/
namespace ConsoleGame
{
    /*
    Position: Handle 2D coordinates
    */
    public class Position
    {
        public int left;
        public int top;
    }

    /*
    Snake: Handle snake properties and behaviors
    */
    public class Snake
    {
        private int _length = 6;

        // snake body positioins
        private List<Position> _points = new List<Position>();

        public int Length
        {
            get
            {
                return _length;
            }
            set
            {
                _length = value;
            }
        }
        public List<Position> Points
        {
            get
            {
                return _points;
            }
            set
            {
                _points = value;
            }
        }

        // Check if the snake is self-overlapped when adding a new position
        public bool IfOverlapped(Position currentPos)
        {
            for (int i = 0; i < _points.Count; i++)
            {
                if ((_points[i].left == currentPos.left) && (_points[i].top == currentPos.top))
                {
                    return true;
                }
            }
            return false;
        }

        // Set snake initial position
        public Position SetInitialPosition()
        {
            return new Position()
            {
                left = 0,
                top = 0
            };
        }

        // Clean up the snake body
        public void CleanUp()
        {
            while (_points.Count() > _length)
            {
                _points.Remove(_points.First());
            }
        }
    }

    /*
    Food: Handle food properties and behaviors
    */
    public class Food
    {
        private Position? _position = null;
        private Random _rnd = new Random();
        public Position? Position
        {
            // get => _position;
            get
            {
                return _position;
            }
            // set => _position = value;
            set
            {
                _position = value;
            }
        }

        public Random Rnd
        {
            get
            {
                return _rnd;
            }
            set
            {
                _rnd = value;
            }
        }
    }

    class Program
    {
        // Record if the game is still on-going
        private static bool _inPlay = true;
        private static int _score = 0;

        private static Snake _snake = new Snake();
        private static Food _food = new Food();

        static void Main(string[] args)
        {
            Console.CursorVisible = false;
            while (_inPlay)
            {
                if (AcceptInput() || UpdateGame())
                    DrawScreen();
            }
        }

        private static DateTime nextUpdate = DateTime.MinValue;

        // Update objects in the scene
        private static bool UpdateGame()
        {
            if (DateTime.Now < nextUpdate) return false;

            if (_food.Position == null)
            {
                _food.Position = new Position()
                {
                    left = _food.Rnd.Next(Console.WindowWidth),
                    top = _food.Rnd.Next(Console.WindowHeight)
                };
            }

            if (_lastKey.HasValue)
            {
                Move(_lastKey.Value);
            }

            // nextUpdate = DateTime.Now.AddMilliseconds(500);
            nextUpdate = DateTime.Now.AddMilliseconds(500 / (_score + 1));
            return true;
        }

        // Draw objects in the scene
        private static void DrawScreen()
        {
            Console.Clear();
            // Use the SetCursorPosition method to specify where the next write operation in the console window is to begin
            Console.SetCursorPosition(Console.WindowWidth - 3, Console.WindowHeight - 1);
            Console.Write(_score);

            for (int i = 0; i < _snake.Points.Count; i++)
            {
                Console.SetCursorPosition(_snake.Points[i].left, _snake.Points[i].top);
                Console.Write('*');
            }

            if (_food.Position != null)
            {
                Console.SetCursorPosition(_food.Position.left, _food.Position.top);
                Console.Write('X');
            }
        }

        static ConsoleKeyInfo? _lastKey;
        private static bool AcceptInput()
        {
            if (!Console.KeyAvailable)
                return false;

            _lastKey = Console.ReadKey();

            return true;
        }

        // Move the snake based on keyboard
        private static void Move(ConsoleKeyInfo key)
        {
            Position currentPos;
            if (_snake.Points.Count != 0)
                currentPos = new Position() { left = _snake.Points.Last().left, top = _snake.Points.Last().top };
            else
                currentPos = _snake.SetInitialPosition();

            switch (key.Key)
            {
                case ConsoleKey.LeftArrow:
                    currentPos.left--;
                    break;
                case ConsoleKey.RightArrow:
                    currentPos.left++;
                    break;
                case ConsoleKey.UpArrow:
                    currentPos.top--;
                    break;
                case ConsoleKey.DownArrow:
                    currentPos.top++;
                    break;
                default:
                    // Undefined keys, so the signal should be ignored
                    break;
            }

            // Check if the new position collide with scene boundary or self-overlapped
            if (DetectCollision(currentPos) == false)
            {
                _snake.Points.Add(currentPos);
                _snake.CleanUp();
            }

        }

        // Handle unexpected cases when a new position is added
        private static bool DetectCollision(Position currentPos)
        {
            // Check if we are off the screen
            if (currentPos.top < 0 || currentPos.top > Console.WindowHeight - 1
                || currentPos.left < 0 || currentPos.left > Console.WindowWidth - 1)
            {
                GameOver();
                return true;
            }

            // Check if we have crashed into the snake body
            // if (_snake.Points.Any(p => p.left == currentPos.left && p.top == currentPos.top))
            if (_snake.IfOverlapped(currentPos) == true)
            {
                GameOver();
                return true;
            }

            // Check if we have eaten the food
            if ((_food.Position != null) && (_food.Position.left == currentPos.left) && (_food.Position.top == currentPos.top))
            {
                _snake.Length++;
                _score++;
                _food.Position = null;
            }

            return false;
        }

        // Setting when the game is over
        private static void GameOver()
        {
            _inPlay = false;
            Console.Clear();
            Console.WriteLine("Game over.");
            Console.ReadLine();

            // throw new NotImplementedException();
        }
    }
}
```


---
# Selected Theory

