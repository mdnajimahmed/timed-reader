# timed-reader
A reader program that exits if the user takes more than 3 seconds between two consecutive inputs
```
package main

import (
	"fmt"
	"os"
	"time"
)

func main() {
	for {
		ch := make(chan string)
		to := make(chan int, 1) // necessary to prevent go routine leak
		go readFromConsole(ch)
		go waitForTimeout(to)
		select {
		case inp := <-ch:
			fmt.Println("user input", inp)
		case quit := <-to:
			fmt.Println("quiting", quit)
			os.Exit(0)
		}
	}
}

func waitForTimeout(to chan int) {
	time.Sleep(3 * time.Second)
	to <- 1
}

func readFromConsole(ch chan string) {
	var s string
	fmt.Scanf("%s\n", &s)
	ch <- s
}

```
