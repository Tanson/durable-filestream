# Welcome to Durable Filestream #

## A file stream library that uses write-ahead logging (WAL) to guarantee the properties of atomicity and durability. (C#). ##

Recover from crashes using deferred update recovery technique.

Durability: Once a write operation is committed, it will survive permanently even if a crash, power loss, or error occurred.

Atomicity: a series of write operations either all occur or nothing occurs.
Deferred update technique: postpone any actual write to the file stream until the commit operation is executed successfully.

Deferred update technique: postpone any actual write to the file stream until the commit operation is executed successfully. Deferred update properties:
  1. A write operation cannot change the file on disk until the commit operation is executed successfully.
  1. The commit point is not reached until all the write operations are persisted in the commit log and the commit log is force-written to disk.


## Example 1 - Writer ##
```csharp

DurableFile.DurableFileStream dfs = new DurableFile.DurableFileStream("example1.dat", true);

long data = 78739;
// write to cache buffer only
dfs.Write(BitConverter.GetBytes(data), 0, 8);

data = 10254;
// write to cache buffer only
dfs.Write(BitConverter.GetBytes(data), 0, 8);

data = 85471;
// write to cache buffer only
dfs.Write(BitConverter.GetBytes(data), 0, 8);

// persist writes permanently to disk
dfs.Commit();

dfs.Close();
```

## Example 2 - Reader ##
```csharp

DurableFile.DurableFileStream dfs = new DurableFile.DurableFileStream("example1.dat", false);
byte[] buffer = new byte[8];
dfs.Read(buffer, 0, 8);
long data = BitConverter.ToInt64(buffer, 0);
Console.WriteLine(data);

dfs.Read(buffer, 0, 8);
data = BitConverter.ToInt64(buffer, 0);
Console.WriteLine(data);

dfs.Read(buffer, 0, 8);
data = BitConverter.ToInt64(buffer, 0);
Console.WriteLine(data);

dfs.Close();
```

## Downloads ##
[download library](https://drive.google.com/file/d/0BzWx1nPrrNHQSjdoOWZtRndsNE0/edit?usp=sharing)


## Upcoming ##
Implementing this library in Java.