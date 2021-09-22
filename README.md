# DZ-2
/*Задание 1.
1.1 Разработать один из классов, в соответствии с полученным вариантом.
1.2 Реализовать не менее пяти закрытых полей (различных типов), представляющих
основные характеристики рассматриваемого класса.
1.3 Создать не менее трех методов управления классом и методы доступа к его
закрытым полям.
1.4 Создать метод, в который передаются аргументы по ссылке.
1.5 Создать не менее двух статических полей (различных типов), представляющих
общие характеристики объектов данного класса.
1.6 Обязательным требованием является реализация нескольких перегруженных
конструкторов, аргументы которых определяются студентом, исходя из
специфики реализуемого класса, а так же реализация конструктора по
умолчанию.
1.7 Создать статический конструктор.
1.8 Создать массив (не менее 5 элементов) объектов созданного класса.
1.9 Создать дополнительный метод для данного класса в другом файле, используя
ключевое слово partial.
 */
using System;


namespace Lesson
{
    partial class WashingMachine {
        private readonly string name;
        private string download_type;         //тип загрузки
        private double loading;               //загрузка
        private int max_spin_speed;           //максимальна швидкість віджима
        private int water_per_cycle;          //розхід води за цикл
        private bool is_direct_drive_motor;   //прямий привід двигуна
        private int number;
        private static int counter;
        private static string producing_country;
        private static int high_voltage;

        public WashingMachine() { }
        public WashingMachine(string _name, string _download_type, double _loading, int _max_spin_speed, int _water_per_cycle, bool _is_direct_drive_motor)
        {
            name = _name;
            download_type = _download_type;
            loading = _loading;
            max_spin_speed = _max_spin_speed;
            water_per_cycle = _water_per_cycle;
            is_direct_drive_motor = _is_direct_drive_motor;
            counter++;
            number = counter;
        }
        static WashingMachine() { high_voltage = 220; }

        public string Download_type { get { return download_type; } set { download_type = value; } }
        public double Loading { get => loading; 
            set => loading = value>0 ? value : 0; }
        public int Max_spin_speed { get => max_spin_speed; set => max_spin_speed = value; }
        public int Water_per_cycle { get => water_per_cycle; set => water_per_cycle = value; }
        public bool Is_direct_drive_motor { get => is_direct_drive_motor; set => is_direct_drive_motor = value; }
        public string Name => name;
        public  int Number { get => number; }
        public static int Counter { get => counter;  }
        public static string Producing_country { get => producing_country; set => producing_country = value; }
        public static int High_voltage { get => high_voltage; }

        public void Print() {          
            Console.WriteLine("_____________________________________");
            Console.WriteLine($"Номер:                  {number}");
            PrintProdusingCountry();
            Console.WriteLine($"Напруга:                {High_voltage} V");
            Console.WriteLine($"Назва пральної машинки: {name}");
            Console.WriteLine($"Тип загрузки:           {download_type}");
            Console.WriteLine($"Загрузка:               {loading} кг");
            Console.WriteLine($"Макс.швидкiсть вiджима: {max_spin_speed} об/хв");
            Console.WriteLine($"Розхiд води за цикл:    {water_per_cycle} л");
            string str1=is_direct_drive_motor== true ? "YES" : "NO";
            Console.WriteLine($"Прямий привiд двигуна:  {str1} ");
            Console.WriteLine("_____________________________________");
            Console.WriteLine();
        }
        public static void PrintProdusingCountry() {
            Console.WriteLine($"Країна виробник:        {Producing_country}");
        }
        public bool TossLaundry(ref double val) {
            if (val <= this.loading && val>0) return true;
            else return false;
        }

    }
    partial class WashingMachine
    {
        public static void ChooceMachine(ref WashingMachine[] mach)
        {
            int num;
            Console.WriteLine($"Виберiть пральну машину по номеру вiд 1 до {mach.Length} : "); ;
            if (int.TryParse(Console.ReadLine(), out num) && num <= mach.Length && num > 0)
            {
                double mass;
                Console.WriteLine("Введiть масу одягу до прання:");
                if (double.TryParse(Console.ReadLine(), out mass) && mach[num - 1].TossLaundry(ref mass))
                {
                    Console.WriteLine($"Маса одягу до прання = {mass} кг");
                    Console.WriteLine($"Максимальна загрузка = {mach[num - 1].Loading} кг");
                    Console.WriteLine("Продовжуєм налаштування режиму прання! ОК");
                }
                else
                {
                    Console.WriteLine($"Маса одягу до прання = {mass} кг");
                    Console.WriteLine($"Максимальна загрузка = {mach[num - 1].Loading} кг");
                    Console.WriteLine("ERROR!!! Перевищена допустима норма загрузки одягу!");
                }
                Console.WriteLine();
            }
            else { Console.WriteLine("Помилка!!! Невiрний номер."); }
        }
    }
    class Program
    {    
        static void Main(string[] args)
        {
            WashingMachine[] machines =  {
            new WashingMachine("Whirlpool TDLR 65230","вертикальна",6.5,1200,40,true),
            new WashingMachine("Samsung WW60J32G0PW", "фронтальна",6,1200,36,false),
            new WashingMachine("Gorenje WHP 60 SF", "фронтальна", 6,1000,40,false),
            new WashingMachine("Samsung WW80R42LHFWD", "фронтальна", 8,1200,48,false),
            new WashingMachine("LG FH0J3NDN0", "фронтальна", 6,1000,48,true)
            };
            WashingMachine.Producing_country = "Китай";
            foreach (var item in machines)  { item.Print();}

            Console.WriteLine(WashingMachine.High_voltage+" V");
            WashingMachine.ChooceMachine(ref machines);
        }   
    }
}
