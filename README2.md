# Using Go Modules

## Creating a new module
` go module init example.com/username/repo`

## Adding a dependency
` go get rsc.io/quote`

## show all direct an indirect dependencies
`go list -m all`

## Upgrading dependencies
` go get golang.org/x/text`
`go list -m -versions rsc.io/sampler`
`go get rsc.io/sampler@v1.3.1`
`go doc rsc.io/quote/v3`

## Removing unused dependencies
`go mod tidy`

# Identifiers
https://go.dev/ref/spec#Identifiers

## Extended Backus–Naur form (EBNF)
https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form