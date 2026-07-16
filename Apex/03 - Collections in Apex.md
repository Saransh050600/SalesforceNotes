
Collections are data structures used to store and manipulate groups of data efficiently. Apex provides three primary collection types:

1. List
2. Set
3. Map

Collections are heavily used in Apex development, especially for bulkification, SOQL queries, and trigger processing.

---

# Why Collections?

Instead of creating multiple variables:

```apex
String name1 = 'John';
String name2 = 'Mike';
String name3 = 'Sara';
```

Use a collection:

```apex
List<String> names = new List<String>{
    'John',
    'Mike',
    'Sara'
};
```

Benefits:

- Better performance
- Easier maintenance
- Bulk processing support
- Reduced code complexity

---

# 1. List

A List is an ordered collection of elements.

### Characteristics

- Maintains insertion order
- Allows duplicate values
- Indexed collection
- Index starts from 0

---

## Creating a List

### Empty List

```apex
List<String> names = new List<String>();
```

### List with Values

```apex
List<String> names = new List<String>{
    'John',
    'Mike',
    'Sara'
};
```

---

## Adding Elements

```apex
List<String> names = new List<String>();

names.add('John');
names.add('Mike');
names.add('Sara');
```

Output:

```text
(John, Mike, Sara)
```

---

## Accessing Elements

```apex
List<String> names = new List<String>{
    'John',
    'Mike',
    'Sara'
};

System.debug(names[0]);
```

Output:

```text
John
```

---

## Updating Elements

```apex
names[1] = 'David';
```

Output:

```text
(John, David, Sara)
```

---

## List Size

```apex
Integer total = names.size();

System.debug(total);
```

Output:

```text
3
```

---

## Check Empty

```apex
if(names.isEmpty()) {
    System.debug('List Empty');
}
```

---

## Contains

```apex
Boolean exists = names.contains('John');
```

---

## Remove Element

```apex
names.remove(0);
```

---

## Clear List

```apex
names.clear();
```

---

## Loop Through List

### Traditional For Loop

```apex
for(Integer i = 0; i < names.size(); i++) {
    System.debug(names[i]);
}
```

### Enhanced For Loop

```apex
for(String name : names) {
    System.debug(name);
}
```

---

## List of sObjects

```apex
List<Account> accounts = [
    SELECT Id, Name
    FROM Account
    LIMIT 5
];
```

```apex
for(Account acc : accounts) {
    System.debug(acc.Name);
}
```

---

## Sorting a List

```apex
List<Integer> numbers = new List<Integer>{
    5,1,3,2,4
};

numbers.sort();

System.debug(numbers);
```

Output:

```text
(1,2,3,4,5)
```

---

# Common List Methods

| Method | Description |
|----------|----------|
| add() | Add element |
| addAll() | Add collection |
| remove() | Remove by index |
| clear() | Remove all |
| contains() | Check existence |
| size() | Count elements |
| isEmpty() | Check empty |
| sort() | Sort list |

---

# 2. Set

A Set stores unique values.

### Characteristics

- No duplicates
- Unordered
- Fast searching
- Excellent for removing duplicates

---

## Creating a Set

```apex
Set<String> cities = new Set<String>();
```

---

## Add Values

```apex
cities.add('Delhi');
cities.add('Mumbai');
cities.add('Delhi');
```

Output:

```text
(Delhi, Mumbai)
```

Duplicate values are automatically ignored.

---

## Check Contains

```apex
if(cities.contains('Delhi')) {
    System.debug('Found');
}
```

---

## Remove Value

```apex
cities.remove('Delhi');
```

---

## Set Size

```apex
System.debug(cities.size());
```

---

## Loop Through Set

```apex
for(String city : cities) {
    System.debug(city);
}
```

---

## Convert List to Set

Remove duplicates from a list.

```apex
List<String> names = new List<String>{
    'John',
    'Mike',
    'John',
    'Sara'
};

Set<String> uniqueNames =
    new Set<String>(names);
```

Output:

```text
(John, Mike, Sara)
```

---

## Convert Set to List

```apex
List<String> nameList =
    new List<String>(uniqueNames);
```

---

## Set of IDs

Very common in triggers.

```apex
Set<Id> accountIds = new Set<Id>();

for(Contact con : Trigger.new) {
    accountIds.add(con.AccountId);
}
```

---

# Common Set Methods

| Method | Description |
|----------|----------|
| add() | Add value |
| remove() | Remove value |
| contains() | Check value |
| size() | Count |
| clear() | Remove all |
| addAll() | Add collection |

---

# 3. Map

A Map stores data as Key-Value pairs.

---

## Characteristics

- Unique keys
- Fast retrieval
- Key-value structure
- Highly used in bulk processing

---

## Creating a Map

```apex
Map<Id, Account> accountMap =
    new Map<Id, Account>();
```

---

## Add Records

```apex
Map<Integer, String> empMap =
    new Map<Integer, String>();

empMap.put(101, 'John');
empMap.put(102, 'Mike');
```

---

## Retrieve Value

```apex
String employee =
    empMap.get(101);

System.debug(employee);
```

Output:

```text
John
```

---

## Update Value

```apex
empMap.put(101, 'David');
```

---

## Check Key Exists

```apex
if(empMap.containsKey(101)) {
    System.debug('Exists');
}
```

---

## Check Value Exists

```apex
empMap.containsValue('John');
```

---

## Remove Entry

```apex
empMap.remove(101);
```

---

## Map Size

```apex
System.debug(empMap.size());
```

---

## Get All Keys

```apex
Set<Integer> keys =
    empMap.keySet();
```

---

## Get All Values

```apex
List<String> values =
    empMap.values();
```

---

## Loop Through Map

### Using KeySet

```apex
for(Integer key : empMap.keySet()) {

    System.debug(
        key + ' : ' + empMap.get(key)
    );
}
```

---

## Map from SOQL Query

Most common Apex pattern.

```apex
Map<Id, Account> accountMap =
    new Map<Id, Account>(
        [SELECT Id, Name
         FROM Account]
    );
```

---

## Bulk Trigger Example

```apex
Set<Id> accountIds = new Set<Id>();

for(Contact con : Trigger.new) {

    if(con.AccountId != null) {
        accountIds.add(con.AccountId);
    }
}

Map<Id, Account> accountMap =
    new Map<Id, Account>(
        [SELECT Id, Name
         FROM Account
         WHERE Id IN :accountIds]
    );
```

---

# Common Map Methods

| Method | Description |
|----------|----------|
| put() | Add/Update |
| get() | Retrieve |
| remove() | Delete |
| containsKey() | Key exists |
| containsValue() | Value exists |
| keySet() | All keys |
| values() | All values |
| size() | Count |
| clear() | Remove all |

---

# Collection Conversion

## List → Set

```apex
Set<String> uniqueNames =
    new Set<String>(nameList);
```

---

## Set → List

```apex
List<String> names =
    new List<String>(nameSet);
```

---

## Map Keys → Set

```apex
Set<Id> accountIds =
    accountMap.keySet();
```

---

## Map Values → List

```apex
List<Account> accounts =
    accountMap.values();
```

---

# 4. Iterators

An Iterator provides a way to traverse a collection sequentially.

---

## Why Use Iterators?

- Read-only access
- Safe traversal
- Prevent accidental modification
- Better control over iteration

---

## Creating Iterator

```apex
List<String> names = new List<String>{
    'John',
    'Mike',
    'Sara'
};

Iterator<String> itr =
    names.iterator();
```

---

## Iterate Using hasNext() and next()

```apex
while(itr.hasNext()) {

    String name = itr.next();

    System.debug(name);
}
```

Output:

```text
John
Mike
Sara
```

---

## Iterator with Set

```apex
Set<String> cities = new Set<String>{
    'Delhi',
    'Mumbai',
    'Bangalore'
};

Iterator<String> itr =
    cities.iterator();

while(itr.hasNext()) {
    System.debug(itr.next());
}
```

---

## Read-Only Nature

```apex
Iterator<String> itr =
    names.iterator();
```

You cannot modify the collection through the iterator.

This prevents concurrent modification issues.

---

# List vs Set vs Map

| Feature | List | Set | Map |
|----------|----------|----------|----------|
| Ordered | ✅ | ❌ | ❌ |
| Duplicates | ✅ | ❌ | Keys ❌ |
| Index Access | ✅ | ❌ | ❌ |
| Key-Value Pair | ❌ | ❌ | ✅ |
| Fast Lookup | Medium | High | Very High |
| Bulk Trigger Usage | High | Very High | Very High |

---

# Real Salesforce Trigger Pattern

```apex
Set<Id> accountIds = new Set<Id>();

for(Contact con : Trigger.new) {

    if(con.AccountId != null) {
        accountIds.add(con.AccountId);
    }
}

Map<Id, Account> accountMap =
    new Map<Id, Account>(
        [SELECT Id, Name
         FROM Account
         WHERE Id IN :accountIds]
    );

for(Contact con : Trigger.new) {

    Account acc =
        accountMap.get(con.AccountId);

    if(acc != null) {
        System.debug(acc.Name);
    }
}
```

---

# Interview Questions

### Why are collections important in Apex?

Collections help process records in bulk and avoid governor limit violations.

---

### Difference between List and Set?

| List | Set |
|--------|--------|
| Allows duplicates | Unique values only |
| Ordered | Unordered |
| Indexed | Not indexed |

---

### Difference between Set and Map?

| Set | Map |
|--------|--------|
| Stores values | Stores key-value pairs |
| Unique values | Unique keys |

---

### Why use Map in Triggers?

Maps provide O(1) lookup performance and avoid nested loops.

---

### What is an Iterator?

An object that traverses collection elements one at a time using:

```apex
hasNext()
next()
```

---

### Which collection is most commonly used in Salesforce triggers?

Combination of:

```apex
List<sObject>
Set<Id>
Map<Id, sObject>
```

This is the standard bulkification pattern used in production Apex.
