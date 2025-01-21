
# Dapper.ColumnMapper

[![NuGet Version](https://img.shields.io/nuget/v/Dapper.ColumnMapper)](https://www.nuget.org/packages/Dapper.ColumnMapper/)
[![Build Status](https://img.shields.io/github/actions/workflow/status/yourusername/Dapper.ColumnMapper/build.yml?branch=main)](https://github.com/yourusername/Dapper.ColumnMapper/actions)
[![License](https://img.shields.io/github/license/yourusername/Dapper.ColumnMapper)](LICENSE)

An extension library for [Dapper](https://github.com/DapperLib/Dapper) that enables attribute-based mapping of database column names to class properties using custom attributes. This simplifies data access code by eliminating the need for manual property mappings.

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Installation](#installation)
- [Usage](#usage)
    - [Defining Models](#defining-models)
    - [Registering Type Maps](#registering-type-maps)
    - [Executing Queries](#executing-queries)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Features

- Attribute-based mapping using `[ColumnName("db_column_name")]`
- Automatic type map registration for Dapper
- Eliminates the need for manual property mappings
- Compatible with standard Dapper methods (`Query`, `Execute`, etc.)
- Supports .NET Framework, .NET Core, and .NET 5/6+

## Getting Started

### Prerequisites

- [.NET SDK](https://dotnet.microsoft.com/download) (.NET Core 3.1 or later recommended)
- [Dapper](https://www.nuget.org/packages/Dapper/) installed
- SQL Server or any other database supported by Dapper

### Installation

Install the `Dapper.ColumnMapper` package via NuGet Package Manager:

```shell
Install-Package Dapper.ColumnMapper
```

### Usage

Defining Models
Decorate your model properties with the [ColumnName] attribute to specify the corresponding database column names.

```csharp
using Dapper.ColumnMapper;

public class User
{
    [ColumnName("user_id")]
    public Guid Id { get; set; }

    [ColumnName("user_name")]
    public string Name { get; set; }

    // Other properties...
}
```
Registering Type Maps
At application startup, register the type maps to enable Dapper to use your custom mappings. This can be done in your Main method or startup configuration.

```csharp
Copy code
using Dapper.ColumnMapper;

public class Program
{
    public static void Main(string[] args)
    {
        // Register custom type maps
        DapperTypeMappingConfig.RegisterTypeMaps();

        // Continue with application startup...
    }
}
```
Executing Queries
Use Dapper's standard methods to execute queries. The custom mappings will be applied automatically.

```csharp
Copy code
using Dapper;
using Microsoft.Data.SqlClient;

public class UserRepository
{
    private readonly string _connectionString = "YourConnectionString";

    public IEnumerable<User> GetAllUsers()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            return connection.Query<User>("SELECT user_id, user_name FROM Users");
        }
    }
}
```

### Examples

Example Model
```csharp
using Dapper.ColumnMapper;

public class Product
{
    [ColumnName("product_id")]
    public int Id { get; set; }

    [ColumnName("product_name")]
    public string Name { get; set; }

    [ColumnName("price")]
    public decimal Price { get; set; }
}
```
Example Repository
```csharp
using Dapper;
using Microsoft.Data.SqlClient;

public class ProductRepository
{
    private readonly string _connectionString = "YourConnectionString";

    public ProductRepository()
    {
        // Ensure type maps are registered
        DapperTypeMappingConfig.RegisterTypeMaps();
    }

    public IEnumerable<Product> GetAllProducts()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            return connection.Query<Product>("SELECT product_id, product_name, price FROM Products");
        }
    }
}
```
Full Example Usage
```csharp
using System;
using System.Collections.Generic;
using Dapper;
using Dapper.ColumnMapper;
using Microsoft.Data.SqlClient;

namespace SampleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // Register custom type maps
            DapperTypeMappingConfig.RegisterTypeMaps();

            // Connection string to your database
            var connectionString = "YourConnectionString";

            using var connection = new SqlConnection(connectionString);

            // Example query
            var products = connection.Query<Product>("SELECT product_id, product_name, price FROM Products");

            foreach (var product in products)
            {
                Console.WriteLine($"{product.Id}: {product.Name} - ${product.Price}");
            }
        }
    }
}
```

### Contributing

Contributions are welcome! Please follow these steps to contribute:

Fork the repository on GitHub.

Clone your fork:

```shell
git clone https://github.com/yourusername/Dapper.ColumnMapper.git
```

Create a feature branch:

```shell
git checkout -b feature/your-feature-name
```

Commit your changes:

```shell
git commit -am "Add your feature"
```

Push to the branch:

```shell
git push origin feature/your-feature-name
```

Open a Pull Request on GitHub.

Building the Project
Open the solution in JetBrains Rider or Visual Studio and build the solution to restore dependencies and compile the projects.

Running Tests
The solution includes a Dapper.ColumnMapper.Tests project with unit tests.

Using the IDE: Run the tests using the built-in test runner.

Using the .NET CLI:

```shell
dotnet test
```

### License

This project is licensed under the MIT License. See the LICENSE file for details.

### Acknowledgments

Dapper - Simple object mapper for .NET

