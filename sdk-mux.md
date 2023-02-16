### Installing SDK in **Go Mux**  Web Application.

**Requirements:**
Go version - 1.17 or higher

**1. `shortloop-go` can be installed like any other Go Package through `go get`:**

```bash
$ go get github.com/short-loop/shortloop-go@v0.0.4
```

**2. Add the following piece of code on top of you root level main.go file where you initialize your router**

```go
import "github.com/short-loop/shortloop-go/shortloopmux"
```

**3. Initialize the shortloop sdk**  
To use shortloop-go sdk, you’ll need to initialize it with options - ShortloopEndpoint and ApplicationName as shown in below example.
```go
router := mux.NewRouter()

shortloopSdk, err := shortloopmux.Init(shortloopmux.Options{
    ShortloopEndpoint: "https://shortloop.company-name.com",   // the shortloop url for your org. (Provided by ShortLoop team.)
    ApplicationName:   "service-name",                         // your application name here.
    AuthKey:           "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", // ShortLoop Auth Key. (Provided by ShortLoop team.)
    Environment:       "your-environment",                     // for e.g stage or prod
})
if err != nil {
    fmt.Println("Error initializing shortloopmux: ", err)
} else {
    router.Use(shortloopSdk.Filter)
}
```
*Quickly test if project is building after configuring SDK :  (Maybe custom to your project)
```bash
go build main.go
```

After the changes, redeploy your Go Application.