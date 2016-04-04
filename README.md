# SharpJSON
C# Single file JSON parser/writer

## License
MIT

## Sample Code
~~~cs
public static void TestVarVarVar()
{
    string s = @"{ a:123, b:456, c:""789"", list: [1, 2, ""!Xyz<> \u0030 \u3042"", true, null, ], }";
 
    // From Stream
    using (var ms = new MemoryStream(Encoding.UTF8.GetBytes(s)))
    {
        Var importedFromStream = Var.FromFormattedStream(ms);
        Console.WriteLine("parsed: \n" + importedFromStream.ToFormattedString());
    }

    // From String
    Var imported = Var.FromFormattedString(s);
    Console.WriteLine("parsed: " + imported.ToCompressedFormattedString());

    // test
    Console.WriteLine("a+b = " + (string)(imported["a"] + imported["b"])); // 579
    Console.WriteLine("a+c = " + (string)(imported["a"] + imported["c"])); // "123789"

    // "123789" is true like JavaScript.
    bool result = imported["a"] + imported["c"];
    Console.WriteLine(@"(bool)(imported[""a""] + imported[""c""]) = " + result);

    // list is VarList. VarList is true like JavaScript. 
    Console.WriteLine(@"imported[""list""] ? true : false = " + (imported["list"] ? true : false));

    if (imported["list"].IsList)
    {
        Console.WriteLine("in list... : ");
        foreach (var v in imported["list"].AsList)
        {
            Console.WriteLine("\t" + v.ToString());
        }
    }
}
~~~
