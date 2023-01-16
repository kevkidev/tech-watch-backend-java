# Modules

## naming :

- same requirements and guidelines as package names
- legal letters include A-Z, a-z, 0-9, \_, and $, separated by .
- lower case
- $ is only used for mechanically generated code
- should be globally unique
- Pick a URL that's associated with the project and invert it to come up with the first part of the module name, then refine from there

## Required deps

- `requires`
- `requires static` : optinal deps
- `requires transitive` : forward deps

## exporting and opening packages

- `exports xxx.pacakge` : public field and types available at compile and run; others only at run.
- `opens xxx.pacakge` :
