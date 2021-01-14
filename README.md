# No2DarkBlue - DTableEntity

Dependency :

[Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)

[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)


----



1. decimal 型別的處理，自動變成 string 儲存，還原後變回 decimal 型別

2. 將 DateTime 型別取回後轉成 local datetime

3. 複雜類別序列化後存入 Azure Table Storage，讀取回來後自動還原

Example :

```csharp

    public class Foo : DTableEntity
    {

        public string Name { get; set; }
        public DateTime Birth { get; set; }
        public decimal Salary { get; set; }
        public List<Foo> Friend { get; set; }
        
    }
    
```

Insert Data Sample：

```csharp

            var obj = new Foo();

            obj.PartitionKey = "PK1";
            obj.RowKey = "RK1";
            obj.Name = "許當麻";
            obj.Salary = 123456789.987654321m;
            obj.Birth = new DateTime(1983, 05, 11);

            var friend = new Foo();
            friend.Name = "許當麻的朋友1";
            friend.Salary = 123456789.987654321m;
            friend.Birth = new DateTime(1983, 05, 11);

            obj.Friend = new System.Collections.Generic.List<Foo>();
            obj.Friend.Add(friend);

            var operation = Microsoft.WindowsAzure.Storage.Table.TableOperation.InsertOrReplace(obj);
            var res = CTable.ExecuteAsync(operation).Result;
    
```

Result : 

![alt 預覽](https://i.imgur.com/0ZoqIuZ.jpg)

