# Go Context Sample

## WithTimeout

```go
package main

import (
	"context"
	"log"
	"time"
)

func main() {
	ctx, _ := context.WithTimeout(context.Background(), 500*time.Millisecond)

	ch := time.After(400 * time.Millisecond) // เปลี่ยน 100 เป็น 600 เพื่อลอง case ctx.Done()
	select {
	case <-ctx.Done():
		log.Println("ctx.Done()")
		log.Printf("ctx.Error() : %q\n", ctx.Err())
	case <-ch:
		log.Println("ch Done")
		log.Printf("ctx.Error() : %q\n", ctx.Err())
	}
}

```

## WithCancel

```go
package main

import (
	"context"
	"log"
	"time"
)

func main() {
	ctx, cancel := context.WithCancel(context.Background())
	defer cancel()

	ch := time.After(2000 * time.Millisecond)

	time.AfterFunc(400*time.Millisecond, func() {
		log.Println("We are about to cancel the ctx.")
		cancel()
	})

	select {
	case <-ctx.Done():
		log.Println("ctx.Done()")
		log.Printf("ctx.Error() : %q\n", ctx.Err())
	case <-ch:
		log.Println("ch Done")
		log.Printf("ctx.Error() : %q\n", ctx.Err())
	}
}

```

## WithValue

```go
package main

import (
	"context"
	"fmt"
)

func main() {
	ctx, _ := context.WithCancel(context.Background())
	ctx2 := context.WithValue(ctx, "a", "1")
	ctx3 := context.WithValue(ctx2, "b", "2")

	lookup(ctx, "a")
	lookup(ctx2, "a")
	lookup(ctx3, "a")
	lookup(ctx3, "b")
}

func lookup(ctx context.Context, k string) {
	fmt.Println(ctx.Value(k))
}

```
