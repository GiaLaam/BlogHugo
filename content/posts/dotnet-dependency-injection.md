---
title: "Dependency Injection trong ASP.NET Core"
date: 2024-12-16
tags: [".NET Core", "DI", "Design Pattern"]
---

## Dependency Injection là gì?

Dependency Injection (DI) là một design pattern giúp đạt được Inversion of Control (IoC) giữa các class và dependencies của chúng. ASP.NET Core có built-in DI container.

## Tại sao cần DI?

- **Loose coupling**: Giảm sự phụ thuộc giữa các class
- **Testability**: Dễ dàng unit test với mock objects
- **Maintainability**: Code dễ bảo trì và mở rộng

## Service Lifetimes

### Transient
Tạo instance mới mỗi lần request.

```csharp
builder.Services.AddTransient<IMyService, MyService>();
```

### Scoped
Tạo instance mới cho mỗi HTTP request.

```csharp
builder.Services.AddScoped<IMyService, MyService>();
```

### Singleton
Chỉ tạo một instance duy nhất cho toàn bộ ứng dụng.

```csharp
builder.Services.AddSingleton<IMyService, MyService>();
```

## Ví dụ thực tế

### Định nghĩa Interface

```csharp
public interface IEmailService
{
    Task SendEmailAsync(string to, string subject, string body);
}

public interface IUserRepository
{
    Task<User?> GetByIdAsync(int id);
    Task<List<User>> GetAllAsync();
    Task CreateAsync(User user);
}
```

### Implement Services

```csharp
public class EmailService : IEmailService
{
    private readonly ILogger<EmailService> _logger;

    public EmailService(ILogger<EmailService> logger)
    {
        _logger = logger;
    }

    public async Task SendEmailAsync(string to, string subject, string body)
    {
        _logger.LogInformation("Sending email to {To}", to);
        // Logic gửi email
        await Task.CompletedTask;
    }
}

public class UserRepository : IUserRepository
{
    private readonly AppDbContext _context;

    public UserRepository(AppDbContext context)
    {
        _context = context;
    }

    public async Task<User?> GetByIdAsync(int id)
    {
        return await _context.Users.FindAsync(id);
    }

    public async Task<List<User>> GetAllAsync()
    {
        return await _context.Users.ToListAsync();
    }

    public async Task CreateAsync(User user)
    {
        _context.Users.Add(user);
        await _context.SaveChangesAsync();
    }
}
```

### Đăng ký Services

```csharp
// Program.cs
builder.Services.AddScoped<IUserRepository, UserRepository>();
builder.Services.AddTransient<IEmailService, EmailService>();
```

### Sử dụng trong Controller

```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    private readonly IUserRepository _userRepository;
    private readonly IEmailService _emailService;

    public UsersController(
        IUserRepository userRepository,
        IEmailService emailService)
    {
        _userRepository = userRepository;
        _emailService = emailService;
    }

    [HttpGet]
    public async Task<ActionResult<List<User>>> GetAll()
    {
        var users = await _userRepository.GetAllAsync();
        return Ok(users);
    }

    [HttpPost]
    public async Task<IActionResult> Create(User user)
    {
        await _userRepository.CreateAsync(user);
        await _emailService.SendEmailAsync(
            user.Email,
            "Chào mừng!",
            "Tài khoản của bạn đã được tạo."
        );
        return CreatedAtAction(nameof(GetAll), user);
    }
}
```

## Kết luận

Dependency Injection là một phần quan trọng trong ASP.NET Core, giúp code clean, dễ test và bảo trì. Hiểu rõ service lifetimes giúp tránh các lỗi về memory và performance.
