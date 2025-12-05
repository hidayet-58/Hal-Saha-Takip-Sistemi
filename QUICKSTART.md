# HaliSaha - Quick Start Guide

## 🚀 Getting Started in 2 Minutes

### Prerequisites
- .NET 8.0 SDK installed
- Visual Studio or VS Code
- SQL Server LocalDB

### Step 1: Start the API
```powershell
cd "C:\Users\hdyto\source\repos\HaliSaha\HaliSahaAPI"
dotnet run --configuration Debug
```
**Expected Output:** `Now listening on: http://localhost:5070`

### Step 2: Start the MVC Application (New Terminal)
```powershell
cd "C:\Users\hdyto\source\repos\HaliSaha\HaliSaha"
dotnet run --configuration Debug
```
**Expected Output:** `Now listening on: http://localhost:5015`

### Step 3: Access the Application
- Open browser to: **http://localhost:5015**
- Navigate to Müşteri, Saha, or Rezervasyon sections

---

## 📝 Testing the API

### Test with PowerShell

**Get all Müşteri:**
```powershell
Invoke-RestMethod -Uri "http://localhost:5070/api/MusteriApi" -Method GET
```

**Create new Müşteri:**
```powershell
$data = @{
    adSoyad = "John Doe"
    telefon = "0555-1234567"
    mail = "john@example.com"
} | ConvertTo-Json

Invoke-RestMethod -Uri "http://localhost:5070/api/MusteriApi" `
    -Method POST `
    -ContentType "application/json" `
    -Body $data
```

**Create new Saha:**
```powershell
$data = @{
    ad = "Futbol Sahası"
    kapasite = 10
    saatlikUcret = 150
} | ConvertTo-Json

Invoke-RestMethod -Uri "http://localhost:5070/api/SahaAPI" `
    -Method POST `
    -ContentType "application/json" `
    -Body $data
```

**Create Rezervasyon:**
```powershell
$data = @{
    musteriId = 3
    sahaId = 1
    baslangicSaati = "2025-12-15T18:00:00"
    bitisSaati = "2025-12-15T20:00:00"
} | ConvertTo-Json

Invoke-RestMethod -Uri "http://localhost:5070/api/RezervasyonAPI" `
    -Method POST `
    -ContentType "application/json" `
    -Body $data
```

---

## 🎯 Key Features

### Automatic Calculation
When creating a reservation, `ToplamUcret` is automatically calculated:
- Formula: `(End Time - Start Time in hours) × Hourly Rate`
- Example: 2 hours × 100 TL/hour = 200 TL

### Error Handling
- Invalid data returns **400 Bad Request** with details
- Non-existent resources return **404 Not Found**
- Successful deletes return **204 No Content**

### Navigation
All pages display data from the API:
- Müşteri list shows all customers
- Saha list shows all fields
- Rezervasyon list shows all reservations with details

---

## 📊 API Endpoints Reference

### Müşteri (Customer)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/MusteriApi` | List all customers |
| GET | `/api/MusteriApi/{id}` | Get specific customer |
| POST | `/api/MusteriApi` | Create new customer |
| PUT | `/api/MusteriApi/{id}` | Update customer |
| DELETE | `/api/MusteriApi/{id}` | Delete customer |

### Saha (Field)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/SahaAPI` | List all fields |
| GET | `/api/SahaAPI/{id}` | Get specific field |
| POST | `/api/SahaAPI` | Create new field |
| PUT | `/api/SahaAPI/{id}` | Update field |
| DELETE | `/api/SahaAPI/{id}` | Delete field |

### Rezervasyon (Reservation)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/RezervasyonAPI` | List all reservations |
| GET | `/api/RezervasyonAPI/{id}` | Get specific reservation |
| POST | `/api/RezervasyonAPI` | Create new reservation |
| PUT | `/api/RezervasyonAPI/{id}` | Update reservation |
| DELETE | `/api/RezervasyonAPI/{id}` | Delete reservation |

---

## 🔧 Troubleshooting

### Port Already in Use
If you get "port 5070 already in use":
```powershell
# Find process using port
Get-NetTCPConnection -LocalPort 5070

# Kill the process (replace PID)
Stop-Process -Id <PID> -Force
```

### Database Error
If database operations fail:
```powershell
# Navigate to API project
cd HaliSahaAPI

# Update database
dotnet ef database update

# Run migrations
dotnet ef migrations add InitialCreate
dotnet ef database update
```

### Build Fails
```powershell
# Clean and rebuild
dotnet clean
dotnet build
```

---

## 📚 Additional Resources

- Full documentation: See `PROJECT_STATUS.md`
- Architecture details: Check controller files for implementation
- Data models: See `Models/` directory in both projects

---

## ✨ Example Workflow

1. **Add Customer** → http://localhost:5015/Musteri/Ekle
2. **Add Field** → http://localhost:5015/Saha/Ekle
3. **Create Reservation** → http://localhost:5015/Rezervasyon/Ekle
   - System auto-calculates total fee
4. **View Reservations** → http://localhost:5015/Rezervasyon
   - Shows customer name, field name, and calculated fee

---

**Status:** ✅ Ready to use  
**Last Updated:** Current session
