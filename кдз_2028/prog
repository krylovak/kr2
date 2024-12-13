using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
class Game2048
{
    //размер игрового поля
    private const int Size = 4;
    //игровое поле
    private int[,] board = new int[Size, Size];
    //количество очков
    private int score = 0;
    //сохранение состояния игры
    private bool saveState = false;

    public void Start()
    {
        Console.WriteLine("игра 2048\nдля перемещения используйте стрелки\nчтобы сохранить результаты игры, нажимайте клавишу 's'");

        if (File.Exists("save.txt"))
        {
            Console.WriteLine("найден файл сохранения. Загрузить?\n(нажмите клавиши 'Y' и затем 'Enter', если хотите загрузить, или 'N' и затем 'Enter', если хотите начать игру с начала)");
            string input = Console.ReadLine().ToUpper();

            if (input == "Y")
            {
                LoadGame();
            }
        }

        InitializeBoard();
        PrintBoard();

        while (true)
        {
            ConsoleKey key = GetInput();

            switch (key)
            {
                case ConsoleKey.UpArrow:
                    MoveUp();
                    break;
                case ConsoleKey.DownArrow:
                    MoveDown();
                    break;
                case ConsoleKey.LeftArrow:
                    MoveLeft();
                    break;
                case ConsoleKey.RightArrow:
                    MoveRight();
                    break;
                case ConsoleKey.S:
                    SaveGame();
                    break;
                case ConsoleKey.Q:
                    QuitGame();
                    return;
            }

            if (!IsMovePossible())
            {
                Console.WriteLine("Конец игры! Ваш счет: {0}", score);
                break;
            }

            AddRandomTile();
            PrintBoard();
        }

        Console.WriteLine("Спасибо за игру!");
    }

    private void InitializeBoard()
    {
        for (int i = 0; i < Size; i++)
        {
            for (int j = 0; j < Size; j++)
            {
                board[i, j] = 0;
            }
        }

        AddRandomTile();
        AddRandomTile();
    }

    //добавление случайной плитки(плитки с 2 или 4)
    private void AddRandomTile()
    {
        List<Tuple<int, int>> emptyCells = FindEmptyCells();

        if (emptyCells.Count > 0)
        {
            Tuple<int, int> cell = emptyCells[new Random().Next(emptyCells.Count)];
            board[cell.Item1, cell.Item2] = new Random().NextDouble() < 0.9 ? 2 : 4;
        }
    }

    //поиск пустых клеток
    private List<Tuple<int, int>> FindEmptyCells()
    {
        var result = new List<Tuple<int, int>>();

        for (int i = 0; i < Size; i++)
        {
            for (int j = 0; j < Size; j++)
            {
                if (board[i, j] == 0)
                {
                    result.Add(new Tuple<int, int>(i, j));
                }
            }
        }

        return result;
    }
    private void PrintBoard()
    {
        Console.Clear();
        Console.WriteLine("Ваш счет: {0}\n", score);

        for (int i = 0; i < Size; i++)
        {
            for (int j = 0; j < Size; j++)
            {
                if (board[i, j] == 0)
                {
                    Console.Write("   ");
                }
                else
                {
                    Console.Write("{0,3}", board[i, j]);
                }

                if (j != Size - 1)
                {
                    Console.Write(" |");
                }
            }

            Console.WriteLine();
            if (i != Size - 1)
            {
                Console.WriteLine("-------------------");
            }
        }
    }
    private ConsoleKey GetInput()
    {
        ConsoleKeyInfo keyInfo = Console.ReadKey(true);

        while (keyInfo.Key != ConsoleKey.UpArrow &&
               keyInfo.Key != ConsoleKey.DownArrow &&
               keyInfo.Key != ConsoleKey.LeftArrow &&
               keyInfo.Key != ConsoleKey.RightArrow &&
               keyInfo.Key != ConsoleKey.S &&
               keyInfo.Key != ConsoleKey.Q)
        {
            keyInfo = Console.ReadKey(true);
        }

        return keyInfo.Key;
    }

    //перемещение вверх
    private void MoveUp()
    {
        for (int col = 0; col < Size; col++)
        {
            List<int> column = new List<int>();

            for (int row = 0; row < Size; row++)
            {
                if (board[row, col] != 0)
                {
                    column.Add(board[row, col]);
                }
            }

            MergeAndUpdate(column, col);
        }
    }
    //вниз
    private void MoveDown()
    {
        for (int col = 0; col < Size; col++)
        {
            List<int> column = new List<int>();

            for (int row = Size - 1; row >= 0; row--)
            {
                if (board[row, col] != 0)
                {
                    column.Add(board[row, col]);
                }
            }
            MergeAndUpdate(column, col, true);
        }
    }
    //влево
    private void MoveLeft()
    {
        for (int row = 0; row < Size; row++)
        {
            List<int> rowList = new List<int>();

            for (int col = 0; col < Size; col++)
            {
                if (board[row, col] != 0)
                {
                    rowList.Add(board[row, col]);
                }
            }

            MergeAndUpdate(rowList, row);
        }
    }
    //вправо
    private void MoveRight()
    {
        for (int row = 0; row < Size; row++)
        {
            List<int> rowList = new List<int>();

            for (int col = Size - 1; col >= 0; col--)
            {
                if (board[row, col] != 0)
                {
                    rowList.Add(board[row, col]);
                }
            }

            MergeAndUpdate(rowList, row, true);
        }
    }

    //cлияние и обновление строки или столбца
    private void MergeAndUpdate(List<int> list, int index, bool reverse = false)
    {
        int startIndex = reverse ? Size - 1 : 0;
        int step = reverse ? -1 : 1;

        for (int i = 0; i < list.Count; i++)
        {
            if (i + 1 < list.Count && list[i] == list[i + 1])
            {
                list[i] *= 2;
                score += list[i];
                list.RemoveAt(i + 1);
                i--;
            }
        }

        for (int i = 0; i < Size; i++)
        {
            if (reverse)
            {
                if (i < list.Count)
                {
                    board[index, startIndex] = list[i]; 
                }
                else
                {
                    board[index, startIndex] = 0;       
                }
                startIndex += step;                    
            }
            else
            {
                if (i < list.Count)
                {
                    board[startIndex, index] = list[i]; 
                }
                else
                {
                    board[startIndex, index] = 0;       
                }
                startIndex += step;                     
            }
        }
    }
    private bool IsMovePossible()
    {
        for (int i = 0; i < Size; i++)
        {
            for (int j = 0; j < Size; j++)
            {
                if (board[i, j] == 0 ||
                    (i > 0 && board[i - 1, j] == board[i, j]) ||
                    (i < Size - 1 && board[i + 1, j] == board[i, j]) ||
                    (j > 0 && board[i, j - 1] == board[i, j]) ||
                    (j < Size - 1 && board[i, j + 1] == board[i, j]))
                {
                    return true;
                }
            }
        }

        return false;
    }
    //сохранение игры
    private void SaveGame()
    {
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < Size; i++)
        {
            for (int j = 0; j < Size; j++)
            {
                sb.AppendFormat("{0},", board[i, j]);
            }
        }
        sb.Append(score);

        File.WriteAllText("save.txt", sb.ToString());

        Console.WriteLine("Игра сохранена.");
    }

    //загрузка игры
    private void LoadGame()
    {
        string[] data = File.ReadAllText("save.txt").Split(',');

        for (int i = 0; i < Size; i++)
        {
            for (int j = 0; j < Size; j++)
            {
                board[i, j] = int.Parse(data[i * Size + j]);
            }
        }

        score = int.Parse(data[data.Length - 1]);

        Console.WriteLine("Игра загружена.");
    }

    //выход из игры
    private void QuitGame()
    {
        if (saveState)
        {
            SaveGame();
        }

        Environment.Exit(0);
    }
}
class Program
{
    static void Main(string[] args)
    {
        Game2048 game = new Game2048();
        game.Start();
    }
}
