# Go Nullable with Generics

[![PkgGoDev](https://pkg.go.dev/badge/github.com/lomsa-dev/gonull)](https://pkg.go.dev/github.com/lomsa-dev/gonull) ![mod-verify](https://github.com/lomsa-dev/gonull/workflows/mod-verify/badge.svg) ![golangci-lint](https://github.com/lomsa-dev/gonull/workflows/golangci-lint/badge.svg) ![staticcheck](https://github.com/lomsa-dev/gonull/workflows/staticcheck/badge.svg) ![gosec](https://github.com/lomsa-dev/gonull/workflows/gosec/badge.svg) [![codecov](https://codecov.io/gh/lomsa-dev/gonull/branch/main/graph/badge.svg?token=76089e7b-f137-4459-8eae-4b48007bd0d6)](https://codecov.io/gh/lomsa-dev/gonull)


## Go package simplifies nullable fields handling with Go Generics.

Package gonull provides a generic `Nullable` type for handling nullable values in a convenient way.
This is useful when working with databases and JSON, where nullable values are common.
Unlike other nullable libraries, gonull leverages Go's generics feature, enabling it to work seamlessly with any data type, making it more versatile and efficient.

## Why gonull

- Use of Go's generics allows for a single implementation that works with any data type.
- Seamless integration with `database/sql` and JSON marshalling/unmarshalling.
- Reduces boilerplate code and improves code readability.

## Usage

```bash
go get https://github.com/lomsa-dev/gonull
```

```go
type User struct {
	Name     gonull.Nullable[string]
	Age      gonull.Nullable[int]
}

func main() {
    // ...
	rows, err := db.Query("SELECT id, name, age FROM users")
	if err != nil {
		log.Fatal(err)
	}
	defer rows.Close()

	for rows.Next() {
		var user User
		err := rows.Scan( &user.Name, &user.Age)
		if err != nil {
			log.Fatal(err)
		}
		fmt.Printf("ID: %d, Name: %v, Age: %v\n", user.Name.Val, user.Age.Val)
	}
    // ...
}
```

Another example

```go
type Person struct {
	Name    string
	Age     int
	Address gonull.Nullable[string]
}

func main() {
	jsonData := []byte(`{"Name":"Alice","Age":30,"Address":null}`)

	var person Person
	err := json.Unmarshal(jsonData, &person)
	if err != nil {
		panic(err)
	}
	fmt.Printf("Unmarshalled Person: %+v\n", person)

	marshalledData, err := json.Marshal(person)
	if err != nil {
		panic(err)
	}
	fmt.Printf("Marshalled JSON: %s\n", string(marshalledData))
}
```
