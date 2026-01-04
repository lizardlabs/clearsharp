# Appendix D – ClearSharp → C# Translation Examples

This appendix provides representative ClearSharp examples and their lowered C# output.

Goals:
- show the Args model and classArgs model
- demonstrate validation, services, resources
- keep generated C# readable and idiomatic

The examples are illustrative and may omit trivial boilerplate.

---

## 1. Simple Action with Args

### ClearSharp

```text
action Add(a as Integer required, b as Integer required)
    return args.a + args.b
end action
```

### Generated C#

```csharp
public sealed class AddArgs
{
    public int A { get; init; }
    public int B { get; init; }
}

public static class Actions
{
    public static int Add(AddArgs args)
    {
        return args.A + args.B;
    }
}
```

---

## 2. Action Call Normalization

### ClearSharp

```text
let x = Add(1, 2)
let y = Add(a=1, b=2)
let z = Add({ a=1, b=2 })
```

### Generated C# (conceptual)

```csharp
var x = Actions.Add(new AddArgs { A = 1, B = 2 });
var y = Actions.Add(new AddArgs { A = 1, B = 2 });
var z = Actions.Add(new AddArgs { A = 1, B = 2 });
```

---

## 3. Service with Dependencies and classArgs

### ClearSharp

```text
service PaymentService
    needs repo as IPaymentRepository
    needs log as ILogger

    action New(Environment as String = "PROD")
        log.Info("Env: " + classArgs.Environment)
    end action

    action Process(PaymentId as Guid required)
        requires PaymentId isNot Nothing else "PaymentId required"
        repo.Save(PaymentId)
    end action
end service
```

### Generated C#

```csharp
public sealed class PaymentServiceArgs
{
    public IPaymentRepository Repo { get; init; } = default!;
    public ILogger Log { get; init; } = default!;
    public string Environment { get; init; } = "PROD";
}

public sealed class PaymentService
{
    private readonly PaymentServiceArgs _classArgs;

    public PaymentService(PaymentServiceArgs classArgs)
    {
        _classArgs = classArgs;
        _classArgs.Log.Info("Env: " + _classArgs.Environment);
    }

    public void Process(ProcessArgs args)
    {
        if (args.PaymentId == Guid.Empty)
            throw new ArgumentException("PaymentId required");

        _classArgs.Repo.Save(args.PaymentId);
    }

    public sealed class ProcessArgs
    {
        public Guid PaymentId { get; init; }
    }
}
```

---

## 4. Rules and Requires

### ClearSharp

```text
rules Transfer for TransferArgs
    require From isNotEmpty else "From required"
    require To isNotEmpty else "To required"
    require Amount > 0 else "Amount must be > 0"
end rules

action Transfer(From as String required, To as String required, Amount as Decimal required)
    requires rules Transfer
    // ... body
end action
```

### Generated C# (conceptual)

```csharp
public static class TransferRules
{
    public static void Validate(TransferArgs args)
    {
        if (string.IsNullOrEmpty(args.From)) throw new ArgumentException("From required");
        if (string.IsNullOrEmpty(args.To)) throw new ArgumentException("To required");
        if (args.Amount <= 0m) throw new ArgumentException("Amount must be > 0");
    }
}

public static class Actions
{
    public static void Transfer(TransferArgs args)
    {
        TransferRules.Validate(args);
        // ... body
    }
}
```

---

## 5. Query (Datasource) Lowering

### ClearSharp

```text
datasource MainDb
    provider = "SqlServer"
    connection = config("MainDb.ConnectionString")
end datasource

query GetCustomer uses MainDb
    input Id as Guid required
    returns Customer
    sql "select * from Customers where Id = @Id"
end query
```

### Generated C# (conceptual)

```csharp
public sealed class GetCustomerArgs
{
    public Guid Id { get; init; }
}

public sealed class GetCustomerQuery
{
    private readonly IDbConnection _db;

    public GetCustomerQuery(IDbConnection db)
    {
        _db = db;
    }

    public Customer? Execute(GetCustomerArgs args)
    {
        return _db.QuerySingleOrDefault<Customer>(
            "select * from Customers where Id = @Id",
            new { Id = args.Id });
    }
}
```

---

## 6. HTTP Request (Refit-style) Lowering

### ClearSharp

```text
api PaymentsApi
    base url = config("PaymentsApi.BaseUrl")
end api

request GetPayment uses PaymentsApi
    method = GET
    path = "/payments/{Id}"
    input Id as Guid required
    returns PaymentDto
end request
```

### Generated C# (conceptual)

```csharp
public sealed class GetPaymentArgs
{
    public Guid Id { get; init; }
}

public interface IPaymentsApi
{
    [Get("/payments/{id}")]
    Task<PaymentDto> GetPayment(Guid id);
}

public sealed class GetPaymentRequest
{
    private readonly IPaymentsApi _api;

    public GetPaymentRequest(IPaymentsApi api)
    {
        _api = api;
    }

    public async Task<PaymentDto?> Execute(GetPaymentArgs args)
    {
        try { return await _api.GetPayment(args.Id); }
        catch { return null; }
    }
}
```

(Note: ClearSharp surface language does not expose Task; this is an implementation detail.)

---

## 7. Message and Handler Lowering

### ClearSharp

```text
message PaymentRequested
    PaymentId as Guid required
    Amount as Decimal required
end message

handler PaymentRequested uses Events
    action Handle()
        requires args.Amount > 0 else "Amount must be > 0"
        // ... process
    end action
end handler
```

### Generated C# (conceptual)

```csharp
public sealed class PaymentRequested
{
    public Guid PaymentId { get; init; }
    public decimal Amount { get; init; }
}

public sealed class PaymentRequestedHandler
{
    public void Handle(PaymentRequested args)
    {
        if (args.Amount <= 0m) throw new ArgumentException("Amount must be > 0");
        // ... process
    }
}
```

---

### End of Appendix D
