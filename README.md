# Go Poloniex API wrapper
This API should be a complete wrapper for the [Poloniex api](https://poloniex.com/support/api/), including the public, private and websocket APIs.

## Install

```
go get -u github.com/pharrisee/poloniex-api
```

## Usage
To use create a copy of config-example.json and fill in your API key and secret.

```json
{
    "key":"put your key here",
    "secret":"put your secret here"
}
```

You can also pass your key/secret pair in code rather than creating a config.json.

# Examples

## Public API

```go
package main

import (
	"log"

	"github.com/k0kubun/pp"
	poloniex "gitlab.com/wmlph/poloniex-api"
)

func main() {
	p := poloniex.NewPublicOnly()
	ob, err := p.OrderBook("BTC_ETH")
	if err != nil {
		log.Fatalln(err)
	}
	pp.Println(ob.Asks[0], ob.Bids[0])
}
```

## Private API

```go
package main

import (
	"fmt"
	"log"

	"gitlab.com/wmlph/poloniex-api"
)

func main() {
	p := poloniex.New("config.json")
	balances, err := p.Balances()
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Printf("%+v\n", balances)
}
```

## Websocket API

```go
package main

import (
	"log"

	poloniex "github.com/pharrisee/poloniex-api"

	"github.com/k0kubun/pp"
)

func main() {
	p := poloniex.NewWithCredentials("Key goes here", "secret goes here")
	log.SetFlags(log.Ldate | log.Ltime | log.Lshortfile)

	ch := p.SubscribeOrder("BTC_ETH")
	for orders := range ch {
		pp.Println(orders)
	}
}

```
