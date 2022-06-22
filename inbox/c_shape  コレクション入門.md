#c_shape

---
2021-12-02


Unity で C#。


## List の使い方

```c#
        List<string> questPartyMembers = new List<string>()
        {"Brim the Barbarian", "Merlin the Wise", "Sterling the Knight"};
        
        questPartyMembers.Add("Craven the Necromancer");

        questPartyMembers.Insert(1, "Tanis the Thief");

        questPartyMembers.RemoveAt(0);

        foreach( string member in questPartyMembers ) {
            Debug.Log(member);
        }
```

foreach の型はないとエラー。ここでは、string




## Dictionary の使い方


 ```c#
        Dictionary<string, int> itemInventory = new Dictionary<string, int>()
        {
            {"Potion", 5},
            {"Antidate", 8},
            {"Aspirin", 1}
        };

        itemInventory["Potion"] = 10;
        itemInventory.Add("THrowing Knife", 3);
        itemInventory["Bandage"] = 5;
        if(itemInventory.ContainsKey("Aspirin")) {
            itemInventory["Aspirin"] = 3;
        }
        itemInventory.Remove("Antidate");

        foreach( KeyValuePair<string, int> item in itemInventory ) {
            Debug.LogFormat("Key: {0}  Value: {1}", item.Key, item.Value);
        }
```

foreach の型は、ここでは、KeyValuePair<string, int> 




