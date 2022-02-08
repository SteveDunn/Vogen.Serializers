![Build](https://github.com/stevedunn/vogen.serialization/actions/workflows/build.yaml/badge.svg) [![GitHub release](https://img.shields.io/github/release/stevedunn/vogen.serialization.svg)](https://GitHub.com/stevedunn/vogen.serialization/releases/) [![GitHub license](https://img.shields.io/github/license/stevedunn/vogen.serialization.svg)](https://github.com/stevedunn/vogen.serialization/blob/main/LICENSE) 
[![GitHub issues](https://img.shields.io/github/issues/Naereen/StrapDown.js.svg)](https://GitHub.com/stevedunn/vogen.serialization/issues/) [![GitHub issues-closed](https://img.shields.io/github/issues-closed/Naereen/StrapDown.js.svg)](https://GitHub.com/stevedunn/vogen.serialization/issues?q=is%3Aissue+is%3Aclosed)
[![Vogen.Serialization stable version](https://badgen.net/nuget/v/vogen.serialization)](https://nuget.org/packages/vogen.serialization)

<p align="center">
  <img src="./assets/cavey.png">
</p>

# NOTE:
This package is only for Vogen version 1.0.16 and lower. Version 1.0.17 of Vogen generates its own serialisation attributes.
<hr/>


# Vogen.Serialization

## Overview

This package contains serializers for the Value Objects which are source-generated by [Vogen](https://www.nuget.org/packages/Vogen/).

Vogen is a _semi_-opinionated library which is a Source Generator and code analyzer that generates [Value Objects](https://wiki.c2.com/?ValueObject).

## Benchmarks

The following two tables (one for System.Text.Json and one for Newtonsoft.Json) show the relative performance of serializing an ints and strings using; native types, record structs, and Value Objects.

There is some overhead to serializing Value Objects, which is to be expected. However, the times shown here are extremely small measurements of time.

The `Strict` value represents whether the Value Objects had validation or not.

### Newtonsoft.Json

```
|                                Method | Strict |        Mean |      Error |    StdDev |   Gen 0 | Allocated |
|-------------------------------------- |------- |------------:|-----------:|----------:|--------:|----------:|
|                     NativeInt_JsonNet |  False |    17.80 μs |   2.558 μs |  0.140 μs |  0.5798 |     10 KB |
|          record_struct_containing_int |  False |    34.59 μs |   1.145 μs |  0.063 μs |  0.6714 |     12 KB |
|    value_object_struct_containing_int |  False |    46.69 μs |   5.881 μs |  0.322 μs |  0.7935 |     13 KB |
|     value_object_class_containing_int |  False |    44.56 μs |   1.495 μs |  0.082 μs |  0.7935 |     13 KB |
|                         native_string |  False |    16.94 μs |   1.161 μs |  0.064 μs |  0.5798 |     10 KB |
|  value_object_class_containing_string |  False |    44.12 μs |   3.269 μs |  0.179 μs |  0.7324 |     13 KB |
| value_object_struct_containing_string |  False |    46.06 μs |   2.626 μs |  0.144 μs |  0.7935 |     13 KB |
|                 various_value_objects |  False | 8,297.31 μs | 167.850 μs |  9.200 μs | 46.8750 |    871 KB |
|                     NativeInt_JsonNet |   True |    17.74 μs |   0.979 μs |  0.054 μs |  0.5798 |     10 KB |
|          record_struct_containing_int |   True |    34.77 μs |   0.342 μs |  0.019 μs |  0.6714 |     12 KB |
|    value_object_struct_containing_int |   True |    46.80 μs |   1.348 μs |  0.074 μs |  0.7935 |     13 KB |
|     value_object_class_containing_int |   True |    45.37 μs |   5.511 μs |  0.302 μs |  0.7324 |     13 KB |
|                         native_string |   True |    16.98 μs |   1.521 μs |  0.083 μs |  0.5798 |     10 KB |
|  value_object_class_containing_string |   True |    45.08 μs |  17.868 μs |  0.979 μs |  0.7324 |     13 KB |
| value_object_struct_containing_string |   True |    45.74 μs |   0.411 μs |  0.023 μs |  0.7935 |     13 KB |
|                 various_value_objects |   True | 8,224.62 μs | 538.024 μs | 29.491 μs | 46.8750 |    845 KB |
```

### System.Text.Json

```
|                                Method | Strict |         Mean |        Error |      StdDev |  Gen 0 |  Gen 1 | Allocated |
|-------------------------------------- |------- |-------------:|-------------:|------------:|-------:|-------:|----------:|
|              NativeInt_SystemTextJson |  False |     375.5 ns |     28.61 ns |     1.57 ns | 0.0219 |      - |     368 B |
|          record_struct_containing_int |  False |     669.3 ns |     25.44 ns |     1.39 ns | 0.0296 |      - |     510 B |
|    value_object_struct_containing_int |  False |   1,209.3 ns |    123.38 ns |     6.76 ns | 0.0362 |      - |     624 B |
|     value_object_class_containing_int |  False |   1,235.7 ns |    114.86 ns |     6.30 ns | 0.0362 |      - |     624 B |
|                         native_string |  False |     614.2 ns |     62.59 ns |     3.43 ns | 0.0410 |      - |     688 B |
|  value_object_class_containing_string |  False |   1,347.0 ns |     13.03 ns |     0.71 ns | 0.0496 |      - |     848 B |
| value_object_struct_containing_string |  False |   1,346.8 ns |    172.35 ns |     9.45 ns | 0.0496 |      - |     848 B |
|                 various_value_objects |  False | 216,397.8 ns |  9,087.26 ns |   498.10 ns | 6.1035 | 0.7324 | 105,080 B |
|              NativeInt_SystemTextJson |   True |     388.8 ns |     37.10 ns |     2.03 ns | 0.0219 |      - |     368 B |
|          record_struct_containing_int |   True |     653.3 ns |     23.28 ns |     1.28 ns | 0.0296 |      - |     510 B |
|    value_object_struct_containing_int |   True |   1,228.5 ns |     82.34 ns |     4.51 ns | 0.0362 |      - |     624 B |
|     value_object_class_containing_int |   True |   1,196.9 ns |     32.56 ns |     1.78 ns | 0.0362 |      - |     624 B |
|                         native_string |   True |     600.2 ns |     35.92 ns |     1.97 ns | 0.0410 |      - |     688 B |
|  value_object_class_containing_string |   True |   1,312.9 ns |    164.98 ns |     9.04 ns | 0.0496 |      - |     848 B |
| value_object_struct_containing_string |   True |   1,367.8 ns |    118.81 ns |     6.51 ns | 0.0496 |      - |     848 B |
|                 various_value_objects |   True | 216,738.1 ns | 20,647.94 ns | 1,131.78 ns | 6.1035 | 0.7324 | 105,080 B |
```

To run the benchmarks yourself, in the `tests\benchmarks` folder, run:

`dotnet run -c Release -- --job short --filter *`

