using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;

namespace WackyShell
{
    internal class Program
    {
        static readonly HashSet<string> Builtins = new(new[] { "echo", "exit", "type", "cd", "pwd", "rm", "clear", "mkdir", "mkfile", "ls"});

        static int Main()
        {
            Console.CancelKeyPress += (s, e) => { e.Cancel = true; };

            while (true)
            {
                Console.Write($"{Directory.GetCurrentDirectory()} $ ");
                string? input = Console.ReadLine()?.Trim();
                if (string.IsNullOrEmpty(input)) continue;

                string[] parts = input.Split(' ', 2);
                string cmd = parts[0];
                string arg = parts.Length > 1 ? parts[1].Trim() : "";

                if (cmd == "exit") return 0;
                else if (cmd == "echo") Console.WriteLine(arg);
                else if (cmd == "cd")
                {
                    try { Directory.SetCurrentDirectory(arg); }
                    catch (Exception ex) { Console.WriteLine($"Error: {ex.Message}"); }
                }
                else if (cmd == "pwd") Console.WriteLine(Directory.GetCurrentDirectory());
                else if (cmd == "ls")
                {
                    foreach (var d in Directory.GetDirectories(".")) Console.WriteLine(d);
                    foreach (var f in Directory.GetFiles(".")) Console.WriteLine(f);
                }
                else if (cmd == "clear") Console.Clear();
                else if (cmd == "mkdir") Directory.CreateDirectory(arg);
                else if (cmd == "mkfile") File.Create(arg).Close();
                else if (cmd == "rm") Delete(arg);
                else if (cmd == "type") Console.WriteLine(Builtins.Contains(arg) ? $"{arg} is a shell builtin" : $"{arg}: not found");
                else Console.WriteLine($"{cmd}: command not found");
            }
        }

        static void Delete(string t)
        {
            if (File.Exists(t)) File.Delete(t);
            else if (Directory.Exists(t)) Directory.Delete(t, true);
            else if (t == "*") foreach (var f in Directory.GetFiles(".")) File.Delete(f);
            else Console.WriteLine("File or directory not found.");
        }

    }
}
