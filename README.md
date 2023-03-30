# 2023_03_30_CantBeEmpty
This project Visual Studio allows to have an exe to drop in project that empty the folder with not file of ".notempty" file. and remove them when filled.

´´´

﻿// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
string errorHappened="";
while (true) {
    if (errorHappened.Length > 0)
        Console.Write("ERROR:" + errorHappened);
    Console.WriteLine("Give folder to target(empty is current):");
    string folderPath = Console.ReadLine();
    Console.WriteLine("Given:\""+folderPath+ "\"");
    errorHappened = "";
    folderPath = folderPath.Trim();
    if (folderPath == null || folderPath.Length == 0) {
        folderPath = Directory.GetCurrentDirectory();
    }
    int created = 0;
    try
    {
        List<string> directories = Directory.GetDirectories(folderPath, "*", SearchOption.AllDirectories).ToList();
        directories.Add(folderPath);
        foreach (var dir in directories)
        {
            if (Directory.Exists(dir))
            {
                string emptyFilePath = dir + "\\.notempty";
                string[] files = Directory.GetFiles(dir);
                files = files.Where(k => k != emptyFilePath).ToArray();
                if (files.Length == 0)
                {
                    created++;
                    if(!File.Exists(emptyFilePath))
                        File.WriteAllText(emptyFilePath, "Not Empty");
                }
                if (files.Length > 0 && File.Exists(emptyFilePath))
                {
                    created++;
                    File.Delete(emptyFilePath);
                }
            }
        }
        Console.WriteLine("Empty folder found .notempty file = " + created);
    }
    catch (Exception e) {
        errorHappened = "Something when wrong  >>>>>>>>>>>\n" + e.StackTrace;
    }
}

´´´
