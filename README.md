<p align=center>
  <img alt=logo src="https://github.com/totaldebug/octopusenergy/raw/main/.docs/assets/workswith.png" height=150 />
  <h3 align=center>Octopus Energy Golang API client</h3>
</p>

---
[![PkgGoDev](https://pkg.go.dev/badge/github.com/totaldebug/octopusenergy/)](https://pkg.go.dev/github.com/totaldebug/octopusenergy/)
[![License](https://img.shields.io/github/license/totaldebug/octopusenergy)](/LICENSE)
[![Release](https://img.shields.io/github/release/totaldebug/octopusenergy.svg)](https://github.com/totaldebug/octopusenergy/releases/latest)
[![tests](https://github.com/totaldebug/octopusenergy/actions/workflows/build.yaml/badge.svg)](https://github.com/totaldebug/octopusenergy/actions/workflows/build.yaml)
[![Go Report Card](https://goreportcard.com/badge/github.com/totaldebug/octopusenergy)](https://goreportcard.com/report/github.com/totaldebug/octopusenergy)
![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/totaldebug/octopusenergy)
![GitHub issues](https://img.shields.io/github/issues/totaldebug/octopusenergy)

This package provides a Golang client to [Octopus Energy's API](https://developer.octopus.energy/docs/api/). Octopus Energy provides a REST API for customers to interact with our platform. Amongst other things, it provides functionality for:

- Browsing energy products, tariffs and their charges.
- Retrieving details about a UK electricity meter-point.
- Browsing the half-hourly consumption of an electricity or gas meter.
- Determining the grid-supply-point (GSP) for a UK postcode.

If you are an Octopus Energy customer, you can generate an API key from your [online dashboard](https://octopus.energy/dashboard/developer/).

### Authentication
Authentication is required for all API end-points when using this API client. This is performed via [HTTP Basic Auth](https://en.wikipedia.org/wiki/Basic_access_authentication). This is configured when you instantiate a new client with a config object.
**Warning: Do not share your secret API keys with anyone.**

### Not an Octopus Energy customer?
Please read about the Octopus tariffs and ensure they are right for you, if you think they are, then please use my [referral link](https://share.octopus.energy/topaz-moon-377). (at the time of writing this we will both receive £50 credit)

### Usage
More in the examples folder

```golang
ctx, cancel := context.WithTimeout(context.Background(), time.Second*30)
defer cancel()

var netClient = http.Client{
    Timeout: time.Second * 10,
}

client := octopusenergy.NewClient(octopusenergy.NewConfig().
    WithApiKeyFromEnvironments().
    WithHTTPClient(netClient),
)

consumption, err := client.Consumption.GetPagesWithContext(ctx, &octopusenergy.ConsumptionGetOptions{
    MPN:          "1111111111", // <--- replace
    SerialNumber: "1111111111", // <--- replace
    FuelType:     octopusenergy.FuelTypeElectricity,
    PeriodFrom:   octopusenergy.Time(time.Now().Add(-48 * time.Hour)),
})

if err != nil {
    log.Fatalf("failed to getting consumption: %s", err.Error())
}
```

### Links
- [Octopus Energy API Docs](https://developer.octopus.energy/docs/api/)
- [Get API Key](https://octopus.energy/dashboard/developer/)
