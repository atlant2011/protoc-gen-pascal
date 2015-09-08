## Quick Example ##

First you need to compile `protoc-gen-pascal` and place it somewhere in the path.

Assume you have a `person.proto` file like this:

```
message Person {
  required int32 id = 1;
  required string name = 2;
  optional string email = 3;
}
```

To produce pascal code you compile it with `protoc` using `--pascal_out` argument:

```
protoc -I. --pascal_out=. person.proto
```

Now you can use that code like this:

```
uses
  person.pb;

...
var
  Person: TPerson;
  OutStream: TFileStream;

...
  Person.Init;
  Person.Id := 123;
  Person.Name := 'Bob';
  Person.Email := 'bob@example.com';

  OutStream := TFileStream.Create('person.pb', fmCreate or fmOpenWrite);
  Person.SerializeToOstream(@OutStream.Write);
  OutStream.Free;
```

Or like this:

```
uses
  person.pb;

...
var
  Person: TPerson;
  InStream: TFileStream;

...
  Person.Init;
  InStream := TFileStream.Create('person.pb', fmOpenRead);
  try
    Person.ParseFromIstream(@InStream.Read);
  except
    Writeln(stderr, 'Failed to parse person.pb.');
    Halt(1);
  end;

  Writeln('ID: ', Person.Id);
  Writeln('name: ', Person.Name);
  if Person.HasEmail then
    Writeln("e-mail: ", Person.Email);
  InStream.Free;
```

## Discussion ##

If you are proficient in russian you may visit [discussion topic](http://www.freepascal.ru/forum/viewtopic.php?f=6&t=9834) on the freepascal.ru forum.