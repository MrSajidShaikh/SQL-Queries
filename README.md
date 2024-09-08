
# DataBase

## 👉 Adding Field Names (Table) 👈
### Ex: field names: id, name, role, salary, age, phone
```
CREATE TABLE "employees" (
	"id"	INTEGER NOT NULL UNIQUE,
	"name"	TEXT NOT NULL,
	"role"	TEXT NOT NULL,
	"salary"	INTEGER NOT NULL,
	"age"	INTEGER NOT NULL,
	"address"	TEXT,
	"phone"	INTEGER NOT NULL,
	PRIMARY KEY("id" AUTOINCREMENT)
);
```
## 👉Adding Details in Field Names (Table)👈
```
INSERT INTO employees
	(id, name, role, salary, age, address, phone)
VALUES
	(1, "RaviNarayan", "Launch Executive", 14000, 21, "Surat", 9510421589);
```

## 👉Adding Multiple Details in Field Names (Table)👈
```
INSERT INTO employees
	(id, name, role, salary, age, address, phone)
VALUES
	(1, "RaviNarayan", "Launch Executive", 14000, 21, "Surat", 9510421589),
	(2, "Sajid", "Electrician", 24000, 22, "Surat", 9316555468);
```

## 👉Read or Find all Details in Field Names (Table)👈
```
SELECT * FROM employees;
```

## 👉Read or Find specific Details in Field Names (Table)👈
```
SELECT name, salary FROM employees;
```

## 👉Read or Find specific Details in Field Names (Table) by their Category 👈
```
SELECT name, salary FROM employees WHERE role = "Launch Executive";
```

## 👉Search Details in Field Names (Table) by name Containing "Ra" 👈
```
SELECT * FROM employees WHERE name like "Ra%";
```

## 👉Find employees details who are older than 20 and earning more than 20,000👈
```
SELECT * FROM employees WHERE age > 20 AND salary > 20000;
```

## 👉Change the salary of an employee with ID 1👈
```
UPDATE employees SET salary = 17000 WHERE id = 1;
```

## 👉Update the address for employees "Launch Executive" role:👈
```
UPDATE employees SET address = "Udhana, Surat" WHERE role = "Launch Executive";
```

## 👉Remove an employee with ID 4👈
```
DELETE FROM employees WHERE id = 4;
```

## 👉Delete all employees under 20 👈
```
DELETE FROM employees WHERE age < 20;
```

# DataBase in Flutter
### DataBase Table

<h1 align = "center">
  <img src="https://github.com/user-attachments/assets/37dfe753-b938-4d87-a7f8-45c82b219648" height=65%  width=80%>
</h1>

## DataBase Helper Class

```
import 'package:path/path.dart';
import 'package:sqflite/sqflite.dart';

class DbHelper {
  static DbHelper dbHelper = DbHelper._();

  DbHelper._();

  Database? _db;

  Future get database async => _db ?? await initDatabase();

  // Future getDatabase()
  // async {
  //   if(_db!=null)
  //     {
  //       return _db;
  //     }
  //   else
  //     {
  //       return await initDatabase();
  //     }
  // }

  Future initDatabase() async {
    final path = await getDatabasesPath();
    final dbPath = join(path, 'finance.db');
    _db = await openDatabase(
      dbPath,
      version: 1,
      onCreate: (db, version) async {
        String sql = '''CREATE TABLE finance(
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        amount REAL NOT NULL,
        isIncome INTEGER NOT NULL,
        category TEXT);
        ''';
        await db.execute(sql);
      },
    );
    return _db;
  }

  Future insertData() async {
    Database? db = await database;
    String sql = '''INSERT INTO finance (amount,isIncome,category)
    VALUES (1999,0,"Watch");
    ''';
    await db!.rawInsert(sql);
  }

  Future updateData() async {
    Database? db = await database;
    String sql = '''UPDATE finance SET category = "Laptop" WHERE amount = 82500;
    ''';
    await db!.rawUpdate(sql);
  }

  Future deleteData() async {
    Database? db = await database;
    String sql = '''DELETE FROM finance WHERE id = 22;
    ''';
    await db!.rawDelete(sql);
  }
}
```


## DataBase Controller
```
import 'package:get/get.dart';
import '../api_helper/db_helper.dart';

class HomeController extends GetxController {
  @override
  void onInit() {
    super.onInit();
    initDb();
  }

  Future initDb() async {
    await DbHelper.dbHelper.database;
  }

  Future insertRecord() async {
    await DbHelper.dbHelper.insertData();
  }

  Future updateRecord() async {
    await DbHelper.dbHelper.updateData();
  }

  Future deleteRecord() async {
    await DbHelper.dbHelper.deleteData();
  }
}

```
