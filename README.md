# Практическая работа №9: Введение в ООП — Классы, объекты, поля, свойства, конструкторы и методы в C#
# Вариант 6. Банковский депозитный сейф (BankSafeBox)
## Выполнили студенты группы П25-2.1 Карпухин Дмитрий и Есин Данила
---
<picture>
  <img src="https://github.com/Karpukhin-D/Task-9/blob/main/screens/1.png">
</picture>

<picture>
  <img src="https://github.com/Karpukhin-D/Task-9/blob/main/screens/2.png">
</picture>

<picture>
  <img src="https://github.com/Karpukhin-D/Task-9/blob/main/screens/3.png">
</picture>

```csharp


using System;

class BankSafeBox
{
    // Закрытые поля
    private int _boxNumber;
    private decimal _deposit;
    private decimal _interestRate;
    private bool _isLocked;

    // Номер ячейки
    public int BoxNumber
    {
        get { return _boxNumber; }
        set
        {
            if (value > 0) _boxNumber = value;
            else _boxNumber = 1;
        }
    }

    // Сумма вклада
    public decimal DepositAmount
    {
        get { return _deposit; }
        set
        {
            if (value >= 0) _deposit = value;
            else _deposit = 0;
        }
    }

    // Процентная ставка от 0 до 30
    public decimal InterestRate
    {
        get { return _interestRate; }
        set
        {
            if (value >= 0 && value <= 30)
                _interestRate = value;
            else
                _interestRate = 0;
        }
    }

    // Статус блокировки замка
    public bool IsLocked
    {
        get { return _isLocked; }
        set { _isLocked = value; }
    }

    // Доступен ли сейф
    public bool IsAccessible
    {
        get { return !_isLocked && _deposit >= 0; }
    }

    // Конструктор по умолчанию
    public BankSafeBox()
    {
        BoxNumber = 101;
        DepositAmount = 1000;
        InterestRate = 5;
        IsLocked = false;
    }

    // Конструктор с параметрами
    public BankSafeBox(int boxNumber, decimal deposit, decimal rate, bool isLocked)
    {
        this.BoxNumber = boxNumber;
        this.DepositAmount = deposit;
        this.InterestRate = rate;
        this.IsLocked = isLocked;
    }

    // Внесение денег
    public void Deposit(decimal amount)
    {
        if (amount > 0 && IsAccessible)
        {
            DepositAmount = DepositAmount + amount;
            Console.WriteLine($"Внесено: {amount:F2} руб.");
        }
        else
        {
            Console.WriteLine("Внесение невозможно.");
        }
    }

    // Снятие денег
    public void Withdraw(decimal amount)
    {
        if (amount > 0 && amount <= DepositAmount && IsAccessible)
        {
            DepositAmount = DepositAmount - amount;
            Console.WriteLine($"Снято: {amount:F2} руб.");
        }
        else
        {
            Console.WriteLine("Снятие невозможно.");
        }
    }

    // Вывод информации о сейфе
    public void PrintStatus()
    {
        Console.WriteLine($"Номер ячейки: {BoxNumber}");
        Console.WriteLine($"Сумма вклада: {DepositAmount:F2} руб.");
        Console.WriteLine($"Процентная ставка: {InterestRate:F2}%");
        Console.WriteLine($"Замок заблокирован: {(IsLocked ? "Да" : "Нет")}");
        Console.WriteLine($"Сейф доступен: {(IsAccessible ? "Да" : "Нет")}");
        Console.WriteLine("--------------------------------");
    }
}

class Program
{
    static void Main()
    {
        Console.ForegroundColor = ConsoleColor.Cyan;
        Console.WriteLine("========================================");
        Console.WriteLine("       БАНКОВСКИЕ ДЕПОЗИТНЫЕ СЕЙФЫ");
        Console.WriteLine("========================================");
        Console.ResetColor();

        // Задание 1. Создание трех объектов
        BankSafeBox[] boxes =
        {
            new BankSafeBox(),
            new BankSafeBox(102, 2500, 7, false),
            new BankSafeBox(103, 5000, 10, false)
        };

        Console.WriteLine("\nНАЧАЛЬНОЕ СОСТОЯНИЕ");
        Console.WriteLine();

        for (int i = 0; i < boxes.Length; i++)
        {
            boxes[i].PrintStatus();
        }

        // Задание 2. Моделирование четырех шагов
        for (int step = 1; step <= 4; step++)
        {
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine($"\n========== ШАГ {step} ==========");
            Console.ResetColor();

            switch (step)
            {
                case 1:
                    Console.WriteLine("Пополнение вкладов:");
                    boxes[0].Deposit(500);
                    boxes[1].Deposit(700);
                    boxes[2].Deposit(1000);
                    break;

                case 2:
                    Console.WriteLine("Начисление процентов:");

                    for (int i = 0; i < boxes.Length; i++)
                    {
                        decimal interest =
                            boxes[i].DepositAmount * boxes[i].InterestRate / 100;

                        Console.WriteLine($"Ячейка {boxes[i].BoxNumber}:");
                        boxes[i].Deposit(interest);
                    }
                    break;

                case 3:
                    Console.WriteLine("Снятие наличных:");
                    boxes[0].Withdraw(300);
                    boxes[1].Withdraw(400);
                    boxes[2].Withdraw(800);
                    break;

                case 4:
                    Console.WriteLine("Блокировка третьего сейфа:");
                    boxes[2].IsLocked = true;
                    boxes[2].Withdraw(100);
                    break;
            }
        }

        // Задание 3. Итоговое состояние
        Console.ForegroundColor = ConsoleColor.Green;
        Console.WriteLine("\n========================================");
        Console.WriteLine("          ИТОГОВОЕ СОСТОЯНИЕ");
        Console.WriteLine("========================================");
        Console.ResetColor();

        for (int i = 0; i < boxes.Length; i++)
        {
            boxes[i].PrintStatus();
        }

        Console.WriteLine("Работа программы завершена.");
    }
}
```
