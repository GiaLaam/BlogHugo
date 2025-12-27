---
title: "Entity Framework Core - ORM cho .NET"
date: 2024-12-19
tags: [".NET Core", "Entity Framework", "Database"]
---

## Entity Framework Core là gì?

Entity Framework Core (EF Core) là một ORM (Object-Relational Mapper) hiện đại cho .NET, cho phép làm việc với database bằng các đối tượng .NET.

## Cài đặt EF Core

```bash
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

## Tạo Model

```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public string Email { get; set; } = string.Empty;
    public DateTime EnrollmentDate { get; set; }
    
    public ICollection<Course> Courses { get; set; } = new List<Course>();
}

public class Course
{
    public int Id { get; set; }
    public string Title { get; set; } = string.Empty;
    public int Credits { get; set; }
    
    public ICollection<Student> Students { get; set; } = new List<Student>();
}
```

## Tạo DbContext

```csharp
using Microsoft.EntityFrameworkCore;

public class SchoolContext : DbContext
{
    public SchoolContext(DbContextOptions<SchoolContext> options)
        : base(options)
    {
    }

    public DbSet<Student> Students { get; set; }
    public DbSet<Course> Courses { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Student>()
            .HasMany(s => s.Courses)
            .WithMany(c => c.Students);
    }
}
```

## Cấu hình trong Program.cs

```csharp
builder.Services.AddDbContext<SchoolContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

## Migration

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

## CRUD Operations

```csharp
public class StudentService
{
    private readonly SchoolContext _context;

    public StudentService(SchoolContext context)
    {
        _context = context;
    }

    // Create
    public async Task<Student> CreateAsync(Student student)
    {
        _context.Students.Add(student);
        await _context.SaveChangesAsync();
        return student;
    }

    // Read
    public async Task<List<Student>> GetAllAsync()
    {
        return await _context.Students
            .Include(s => s.Courses)
            .ToListAsync();
    }

    // Update
    public async Task UpdateAsync(Student student)
    {
        _context.Entry(student).State = EntityState.Modified;
        await _context.SaveChangesAsync();
    }

    // Delete
    public async Task DeleteAsync(int id)
    {
        var student = await _context.Students.FindAsync(id);
        if (student != null)
        {
            _context.Students.Remove(student);
            await _context.SaveChangesAsync();
        }
    }
}
```

## Kết luận

EF Core giúp đơn giản hóa việc làm việc với database, hỗ trợ LINQ queries, migrations, và nhiều database providers khác nhau.
