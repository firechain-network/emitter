# Emitter.

A flexible event emitter inspired by NodeJS's `event-emitter`.

Emits events. Doesn't ask for raises.

## Usage

```go
package main

import(
  "fmt"
  "github.com/firechain-network/emitter"
)

func main(){
  echo := fmt.Println
  emitter := Emitter.Construct()

  // with inline functions:
  emitter.On("0x021f…400f:Tick", func(args ... interface{}){
    // ...
  })

  emitter.Once("0x021f…400f:Tick", func(args ...interface{}){
    // ...
  })

  // with defined functions:
  handler := func(args ...interface{}){
    // ...
  }

  emitter.On("0x021f…400f:Tick", handler)

  // emitting:
  emitter.EmitAsync("0x021f…400f:Tick", nil)
  emitter.EmitAsync("0x021f…400f:Tick", []interface{}{"foo", "bar"})
  emitter.EmitSync("0x021f…400f:Tick", nil)
  emitter.EmitSync("0x021f…400f:Tick", []interface{}{"foo", "bar"})

  // wildcards:
  emitter.On("0x021f…400f:*", handler)
  emitter.On("**", handler)

  // removing listeners:
  emitter.RemoveListener("0x021f…400f:Tick", handler)

  // remove all listeners from a specific event:
  emitter.RemoveAllListeners("0x021f…400f:Tick")

  // remove all listener registrations globally:
  emitter.RemoveAllListeners()

  // fetch listeners:
  emitter.Listeners("0x021f…400f:Tick") // nil if empty

  // fetch the number of listeners for a specific event:
  emitter.ListenersCount("0x021f…400f:Tick")

}
```

## Types

```go
type Emitter struct {
  listeners  map[interface{}][]Listener
}

type Listener struct {
  callback  func(...interface{})
  once    bool
}
```
