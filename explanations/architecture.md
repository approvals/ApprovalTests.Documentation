# Architecture

```mermaid
flowchart

input & options --> verify
verify --> namer
verify --> reporter
verify --> options2
verify --> writer
verify --> comparator
options --> scrubber
```

options contains namer

## Namer
approval file - `guilded_rose_test.approval_test.approved.txt`  
received file - `guilded_rose_test.approval_test.received.txt`  
general pattern  - `file_name.test_name.approved.extension`

(access name of test in exunit?)

## Reporter
```elixir
def report(approved, received) do
    true
end
```

```mermaid
flowchart
diff_reporter --> windows_reporter & linux_reporter & mac_reporter
windows_reporter --> vs_code_reporter --> beyond_compare_reporter --> cmd
```

## Writer
Takes content + namer and produces two files  
mostly text but may be other types

```mermaid
flowchart
Namer & Contents & Extension --> Writer --> received_file & approved_file
```

## Comparator
```mermaid
flowchart
received_file & approved_file & reporter --> comparator --> result["Test Result\nnothing or throws exception"] & reporter_call["Reporter.report()\nOnly on test failiure"]
```

Line endings! - still a thing
usually a file comparator

## Scrubbers
Makes input consistent

```elixir
def scrub(text) do
    text
end
```

```mermaid
flowchart
input["created on\n2020-1-29"] --> scrubber --> output["created on\n[date_1]"]
```

## Options
Immutable bag (just a hashmap of properties) with reasonable defaults  
Most verify functions hava an optional options (if you pass it in it is taken into account)  
