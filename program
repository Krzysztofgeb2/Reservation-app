using System;
using System.Text;

namespace Proj_object
{
    internal class Program
    {

        public abstract class ObiektRezerwacyjny
        {
            protected bool[][] miejsca;
            protected decimal saldo;

            public ObiektRezerwacyjny(int liczbaRzedow, int liczbaMiejscWRzedzie, decimal poczatkoweSaldo)
            {
                saldo = poczatkoweSaldo;
                InicjalizujMiejsca(liczbaRzedow, liczbaMiejscWRzedzie);
            }

            private void InicjalizujMiejsca(int liczbaRzedow, int liczbaMiejscWRzedzie)
            {
                miejsca = new bool[liczbaRzedow][];
                for (int i = 0; i < liczbaRzedow; i++)
                {
                    miejsca[i] = new bool[liczbaMiejscWRzedzie];
                }
            }

            public virtual bool ZarezerwujMiejsce(int rzad, int miejsce, decimal koszt)
            {
                if (saldo < koszt)
                {
                    Console.WriteLine("Brak wystarczających środków na koncie.");
                    return false;
                }

                if (rzad < 0 || rzad >= miejsca.Length || miejsce < 0 || miejsce >= miejsca[rzad].Length)
                {
                    Console.WriteLine("Nieprawidłowy numer rzędu lub miejsca.");
                    return false;
                }

                if (miejsca[rzad][miejsce])
                {
                    Console.WriteLine("Miejsce jest już zajęte.");
                    return false;
                }

                miejsca[rzad][miejsce] = true;
                saldo -= koszt;
                Console.WriteLine($"Miejsce zostało zarezerwowane. Pozostałe saldo: {saldo:C}");
                return true;
            }

            public void DodajSaldo(decimal kwota)
            {
                saldo += kwota;
                Console.WriteLine($"Dodano {kwota:C}. Aktualne saldo: {saldo:C}");
            }

            public override string ToString()
            {
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < miejsca.Length; i++)
                {
                    for (int j = 0; j < miejsca[i].Length; j++)
                    {
                        sb.Append(miejsca[i][j] ? "X" : "O");
                    }

                    sb.AppendLine();
                }

                return sb.ToString();
            }
        }

        public class Stadion : ObiektRezerwacyjny
        {
            public Stadion(int liczbaRzedow, int liczbaMiejscWRzedzie, decimal poczatkoweSaldo)
                : base(liczbaRzedow, liczbaMiejscWRzedzie, poczatkoweSaldo)
            {
            }
        }

        public static class Test
        {
            public static bool TestRezerwacji(Stadion stadion)
            {
                Console.WriteLine("Uruchamianie Testu Rezerwacji...");

                bool wynik1 = stadion.ZarezerwujMiejsce(2, 5, 10m);
                if (!wynik1)
                {
                    Console.WriteLine("TestRezerwacji: Niepowodzenie przy rezerwacji miejsca.");
                    return false;
                }

                bool wynik2 = stadion.ZarezerwujMiejsce(3, 7, 10m);
                if (!wynik2)
                {
                    Console.WriteLine("TestRezerwacji: Niepowodzenie - pozwolono na ponowną rezerwację.");
                    return false;
                }

                Console.WriteLine("TestRezerwacji: Test zakończony sukcesem.");
                return true;
            }

            public static bool TestSalda(Stadion stadion)
            {
                Console.WriteLine("Uruchamianie Testu Salda...");

                stadion.DodajSaldo(40m);

                bool wynik1 = stadion.ZarezerwujMiejsce(3, 10, 10m);
                if (!wynik1)
                {
                    Console.WriteLine("TestSalda: Niepowodzenie przy rezerwacji z wystarczającym saldem.");
                    return false;
                }

                stadion.DodajSaldo(-20m);
                bool wynik2 = stadion.ZarezerwujMiejsce(4, 7, 10m);
                if (!wynik2) 
                {
                    Console.WriteLine("TestSalda: Niepowodzenie - pozwolono na rezerwację przy braku środków.");
                    return false;
                }

                Console.WriteLine("TestSalda: Test zakończony sukcesem.");
                return true;
            }
        }

        static void Main(string[] args)
        {
            Stadion stadion = new Stadion(10, 20, 0m);

            while (true)
            {
                Console.Clear();
                Console.WriteLine("Menu Główne:");
                Console.WriteLine("1. Zarezerwuj miejsce (koszt: 20 zł)");
                Console.WriteLine("2. Sprawdź wolne miejsca");
                Console.WriteLine("3. Dodaj saldo");
                Console.WriteLine("4. Run Tests");
                Console.WriteLine("5. Wyjście");
                Console.Write("Wybierz opcję: ");

                if (!int.TryParse(Console.ReadLine(), out int menu))
                {
                    Console.WriteLine("Nieprawidłowy wybór. Naciśnij Enter, aby kontynuować.");
                    Console.ReadLine();
                    continue;
                }

                switch (menu)
                {
                    case 1:
                        Console.Clear();
                        Console.Write("Podaj numer rzędu (0-9): ");
                        if (!int.TryParse(Console.ReadLine(), out int rzad))
                        {
                            Console.WriteLine("Nieprawidłowy numer rzędu. Naciśnij Enter, aby wrócić do menu.");
                            Console.ReadLine();
                            break;
                        }

                        Console.Write("Podaj numer miejsca (0-19): ");
                        if (!int.TryParse(Console.ReadLine(), out int miejsce))
                        {
                            Console.WriteLine("Nieprawidłowy numer miejsca. Naciśnij Enter, aby wrócić do menu.");
                            Console.ReadLine();
                            break;
                        }

                        stadion.ZarezerwujMiejsce(rzad, miejsce, 20m);
                        Console.WriteLine("Naciśnij Enter, aby wrócić do menu.");
                        Console.ReadLine();
                        break;

                    case 2:
                        Console.Clear();
                        Console.WriteLine("Stan wolnych miejsc:");
                        Console.WriteLine(stadion);
                        Console.WriteLine("Naciśnij Enter, aby wrócić do menu.");
                        Console.ReadLine();
                        break;

                    case 3:
                        Console.Clear();
                        Console.Write("Podaj kwotę do dodania: ");
                        if (!decimal.TryParse(Console.ReadLine(), out decimal kwota) || kwota <= 0)
                        {
                            Console.WriteLine("Nieprawidłowa kwota. Naciśnij Enter, aby wrócić do menu.");
                            Console.ReadLine();
                            break;
                        }

                        stadion.DodajSaldo(kwota);
                        Console.WriteLine("Naciśnij Enter, aby wrócić do menu.");
                        Console.ReadLine();
                        break;

                    case 4:
                        Console.Clear();
                        bool testRezerwacji = Test.TestRezerwacji(stadion);
                        bool testSalda = Test.TestSalda(stadion);

                        if (testRezerwacji && testSalda)
                        {
                            Console.WriteLine("Wszystkie testy zakończone sukcesem!");
                        }
                        else
                        {
                            Console.WriteLine("Niektóre testy nie powiodły się.");
                        }

                        Console.WriteLine("Naciśnij Enter, aby wrócić do menu.");
                        Console.ReadLine();
                        break;

                    case 5:
                        Console.WriteLine("Do widzenia!");
                        return;

                    default:
                        Console.WriteLine("Nieprawidłowy wybór. Naciśnij Enter, aby kontynuować.");
                        Console.ReadLine();
                        break;
                }
            }
        }
    }
