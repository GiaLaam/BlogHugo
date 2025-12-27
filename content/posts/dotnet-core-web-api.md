---
title: "Xây dựng REST API với ASP.NET Core"
date: 2024-12-22
tags: [".NET Core", "Web API", "REST"]
---

## Giới thiệu ASP.NET Core Web API

ASP.NET Core là framework mạnh mẽ của Microsoft để xây dựng các ứng dụng web và API hiện đại, cross-platform.

## Tạo project Web API

```bash
dotnet new webapi -n MyWebAPI
cd MyWebAPI
```

## Cấu trúc project

```
MyWebAPI/
├── Controllers/
│   └── WeatherForecastController.cs
├── Program.cs
├── appsettings.json
└── MyWebAPI.csproj
```

## Tạo Model

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public decimal Price { get; set; }
    public int Stock { get; set; }
}
```

## Tạo Controller

```csharp
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private static List<Product> _products = new()
    {
        new Product { Id = 1, Name = "Laptop", Price = 999.99m, Stock = 10 },
        new Product { Id = 2, Name = "Mouse", Price = 29.99m, Stock = 50 }
    };

    [HttpGet]
    public ActionResult<IEnumerable<Product>> GetAll()
    {
        return Ok(_products);
    }

    [HttpGet("{id}")]
    public ActionResult<Product> GetById(int id)
    {
        var product = _products.FirstOrDefault(p => p.Id == id);
        if (product == null)
            return NotFound();
        return Ok(product);
    }

    [HttpPost]
    public ActionResult<Product> Create(Product product)
    {
        product.Id = _products.Max(p => p.Id) + 1;
        _products.Add(product);
        return CreatedAtAction(nameof(GetById), new { id = product.Id }, product);
    }

    [HttpPut("{id}")]
    public IActionResult Update(int id, Product product)
    {
        var existing = _products.FirstOrDefault(p => p.Id == id);
        if (existing == null)
            return NotFound();
        
        existing.Name = product.Name;
        existing.Price = product.Price;
        existing.Stock = product.Stock;
        return NoContent();
    }

    [HttpDelete("{id}")]
    public IActionResult Delete(int id)
    {
        var product = _products.FirstOrDefault(p => p.Id == id);
        if (product == null)
            return NotFound();
        
        _products.Remove(product);
        return NoContent();
    }
}
```

## Chạy API

```bash
dotnet run
```

API sẽ chạy tại `https://localhost:5001/api/products`

## Kết luận

ASP.NET Core Web API cung cấp cách tiếp cận đơn giản và mạnh mẽ để xây dựng RESTful services với đầy đủ tính năng như routing, model binding, validation.
