# PyCodable

In Swift 4,
a type that conforms to the `Codable` protocol
can be encoded to or decoded from representations
for any format that implements a corresponding `Encoder` or `Decoder` type.

PyCodable takes the same implementations as JSONDecoder / JSONEncoder and uses it to extract data from PyClasses / PyDictionaries into Swift Classes and vice versa. 

[JSONDecoder details](https://github.com/apple/swift/blob/master/stdlib/public/SDK/Foundation/JSONDecoder.swift)

[JSONEncoder details](https://github.com/apple/swift/blob/master/stdlib/public/SDK/Foundation/JSONEncoder.swift)

## Usage

### Decoder Structure

```python
# Python
person_dict = {
	"name": "Joe",
	"age": 25,
	"street": "PySwift Lane",
	"interests": ["coding","chilling","others"]
}
```

```swift
// Swift
enum Interest: String, Codable {
    case coding
    case chilling
    case others
}

class Person: Codable {
    var name: String
    var age: Int
    var street: String
    var interests: [Interest]
}

// decode person_dict (PyPointer) into Person Class
let dict_obj: PyPointer = SomePyFunction() // returns the person_dict from python
let pydecoder = PyDecoder()
let person = pydecoder.decode(Person.self, from: dict_obj)

print(person.name)
// Joe
print(person.age)
// 25
print(person.street)
// PySwift Lane

for interest in person.interests {
    switch interest {
    case .coding:
    	print("\(person.name) likes to code ")
    case .chilling:
    	print("\(person.name) LOVES to chill ")
    case .other:
    	print("\(person.name) likes other stuff 2 ")
    }
}

```

### list of "person"


```python
# Python
peoples_list = [
    {
        "name": "Joe",
        "age": 25,
        "street": "PySwift Lane 1",
        "interests": ["coding", "chilling", "others"]
    },
    {
        "name": "Matt",
        "age": 19,
        "street": "PySwift Lane 2",
        "interests": ["chilling", "others"]
    },
    {
        "name": "Sussie",
        "age": 32,
        "street": "PySwift Lane 3",
        "interests": ["others"]
    }
]
```

```swift
let list_obj: PyPointer = SomePyFunction() // returns the person_dict from python
let pydecoder = PyDecoder()

let peoples = pydecoder.decode([Person].self, from: dict_obj) 
// notice that we use [ type ].self now, to tell swift we want an Array of Person

for person in peoples {
    print(person.name)
    print(person.age)
    print(person.street)

    for interest in person.interests {
        switch interest {
        case .coding:
            print("\(person.name) likes to code ")
        case .chilling:
            print("\(person.name) LOVES to chill ")
        case .other:
            print("\(person.name) likes other stuff 2 ")
        }

    }
}
```




