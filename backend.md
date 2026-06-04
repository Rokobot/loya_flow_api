# 🕷️ Backend API — Issue Log

> **Auth Service** · `62.171.172.254:8083` · Hazırlayan: **Rafet** → Backend Team

---

## 📊 Xülasə

| | Sayı | Status |
|---|---|---|
| 🔴 Kritik Bug | 2 | Düzəldilməli |
| 🟠 High Bug | 1 | Düzəldilməli |
| 🟡 Medium Bug | 1 | Düzəldilməli |
| 🔵 Yeni Endpoint | 4 | Əlavə edilməli |

---

## 🔴 BUG-LAR

---

### #1 — Login · Yanlış credentials-də `500` qaytarır

> **Severity:** 🔴 Critical  
> **Endpoint:** `POST /api/auth/login`

**Flutter log (mövcud):**
```
╔╣ DioError ║ Status: 500 Internal Server Error ║ Time: 557 ms
║ http://62.171.172.254:8083/api/auth/login
╔ DioExceptionType.badResponse
║ AuthService.Application.Exceptions.BadHttpRequestException:
║   Telefon nömrəsi və ya şifrə yanlışdır.
```

**Problem:**  
Telefon nömrəsi və ya şifrə yanlış olduqda server `HTTP 500` qaytarır. Bu Flutter tərəfdə `DioExceptionType.badResponse` crash-ına səbəb olur. Validation xətası heç vaxt `500` olmamalıdır.

**Mövcud response:**
```http
HTTP/1.1 500 Internal Server Error

AuthService.Application.Exceptions.BadHttpRequestException:
  Telefon nömrəsi və ya şifrə yanlışdır.
```

**Olmalıdır:**
```json
HTTP/1.1 200 OK

{
  "success": false,
  "message": "Telefon nömrəsi və ya şifrə yanlışdır."
}
```

**Fix:**  
`BadHttpRequestException` tipini global exception handler-də (`ExceptionMiddleware` / `GlobalExceptionHandler`) ayrıca tutun. Bu exception `HTTP 500`-ə çevrilməməli, `200 OK` + `success: false` + mesaj ilə qaytarılmalıdır.

---

### #2 — Login · İlk sorğuda `500`, ikincidə success olur

> **Severity:** 🟠 High  
> **Endpoint:** `POST /api/auth/login` · Race condition / Cold start

**Problem:**  
Credentials tamamilə doğru olsa belə ilk login sorğusu `500` qaytarır. Flutter tərəfdə `catch` blokundakı ikinci `try` isə uğurla tamamlanır. Bu server tərəfdəki initialization probleminə işarə edir.

**Ehtimal olunan səbəblər:**

| Səbəb | İzah |
|---|---|
| DB connection pool | İlk sorğuda pool hazır olmaya bilər (cold start) |
| Middleware sırası | Token/session init middleware ilk sorğuda fail edə bilər |
| Servis init | Redis / cache / token store server start-da tam hazır olmur |

**Fix:**  
- Server başladıqda `DB`, `Redis`, `cache`, `token store` servislerinin hazır olduğunu yoxlamaq üçün `/health` endpoint əlavə edin  
- Middleware sırasını nəzərdən keçirin  
- Connection pool warmup tətbiq edin  

---

### #3 — Password Reset · Eyni parol yenidən set edilə bilir

> **Severity:** 🟡 Medium  
> **Endpoint:** `POST /api/auth/reset-password` · Güvənlik problemi

**Problem:**  
Password reset endpoint-ində mövcud parol ilə eyni parolu yenidən set etmək mümkündür. İstifadəçi real mənada parolunu dəyişməmiş olur — bu güvənlik riskidir.

**Mövcud davranış:**
```http
POST /api/auth/reset-password
{ "new_password": "mövcud_parol" }

→ HTTP 200 OK   ← yanlışdır
```

**Olmalıdır:**
```json
{
  "success": false,
  "message": "Yeni parol köhnə parolla eyni ola bilməz."
}
```

**Fix:**  
Reset əməliyyatında yeni parolu hash-ləyib mövcud hash ilə `password_verify()` / `BCrypt.Verify()` ilə müqayisə edin. Eyni olduqda `success: false` + mesaj qaytarın.  
> ⚠️ Bu yoxlama `#8` PIN reset-ə də tətbiq edilməlidir.

---

### #4 — Register · Mövcud nömrə üçün `500` qaytarır

> **Severity:** 🔴 Critical  
> **Endpoint:** `POST /api/auth/register`

**Flutter log (mövcud):**
```
║ AuthService.Application.Exceptions.BadHttpRequestException:
║   Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib
```

**Mövcud response:**
```http
HTTP/1.1 500 Internal Server Error

BadHttpRequestException: Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib.
```

**Olmalıdır:**
```json
HTTP/1.1 200 OK

{
  "success": false,
  "message": "Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib."
}
```

**Fix:**  
`#1` ilə eyni kök problem. Register controller-də nömrə mövcudluğunu DB-dən əvvəlcədən yoxlayın, exception-a düşmədən `200 + success: false` qaytarın.

---

## 🔵 YENİ ENDPOİNTLƏR

---

### #5 — Müştəri nömrə dəyişmə API

> **Method:** `PUT`  
> **Endpoint:** `/api/customer/change-phone`  
> **Auth:** `Bearer {token}` — məcburi

**Request:**
```http
PUT /api/customer/change-phone
Authorization: Bearer {token}
Content-Type: application/json
```

```json
{
  "current_phone": "+994501234567",
  "new_phone":     "+994551234567",
  "otp_code":      "123456"
}
```

**Parametrlər:**

| Sahə | Tip | Tələb | Açıqlama |
|---|---|---|---|
| `current_phone` | string | ✱ Məcburi | İstifadəçinin mövcud nömrəsi |
| `new_phone` | string | ✱ Məcburi | Yeni nömrə (format: `+994XXXXXXXXX`) |
| `otp_code` | string | ✱ Məcburi | Yeni nömrəyə göndərilən 6 rəqəmli OTP |

**Responses:**
```json
// ✅ Uğurlu
{ "success": true, "message": "Nömrə uğurla dəyişdirildi." }

// ❌ OTP yanlış / müddəti bitib
{ "success": false, "message": "OTP yanlışdır və ya müddəti bitib." }

// ❌ Nömrə artıq mövcuddur
{ "success": false, "message": "Bu nömrə artıq istifadədədir." }
```

> 📝 OTP göndərmə üçün əvvəlcə `POST /api/auth/send-otp` atılmalıdır. Nömrə dəyişdikdən sonra yeni token qaytarın.

---

### #6 — İstifadəçi məlumatları (Get User Info)

> **Method:** `GET`  
> **Endpoint:** `/api/customer/me`  
> **Auth:** `Bearer {token}` — məcburi

**Request:**
```http
GET /api/customer/me
Authorization: Bearer {token}
```

**Response:**
```json
// ✅ Uğurlu
HTTP/1.1 200 OK

{
  "success": true,
  "data": {
    "id":            1,
    "full_name":     "Əli Həsənov",
    "phone":         "+994501234567",
    "email":         "ali@example.com",
    "profile_image": "https://...",
    "created_at":    "2024-01-01T00:00:00Z",
    "is_verified":   true,
    "has_pin":       true
  }
}

// ❌ Token invalid / expired
{ "success": false, "message": "Unauthorized." }
```

> 📝 `has_pin` sahəsi Flutter tərəfin PIN ekranına yönləndirmə qərarı üçün lazımdır. Token refresh lazım olduqda `401` qaytarın, `500` yox.

---

### #7 — PIN Doğrulama (Pin Verify)

> **Method:** `POST`  
> **Endpoint:** `/api/auth/pin/verify`  
> **Brute-force qoruma daxildir**

**Request:**
```http
POST /api/auth/pin/verify
Content-Type: application/json
```

```json
{
  "phone": "+994501234567",
  "pin":   "4321"
}
```

**Parametrlər:**

| Sahə | Tip | Tələb | Açıqlama |
|---|---|---|---|
| `phone` | string | ✱ Məcburi | İstifadəçi telefon nömrəsi |
| `pin` | string | ✱ Məcburi | 4–6 rəqəmli PIN kod |

**Responses:**
```json
// ✅ Uğurlu doğrulama
{
  "success": true,
  "token":   "eyJhbGciOiJIUzI1NiIsInR5...",
  "user": {
    "id":        1,
    "full_name": "Əli Həsənov"
  }
}

// ❌ Yanlış PIN
{
  "success":      false,
  "message":      "PIN yanlışdır.",
  "attempts_left": 4
}

// ❌ Çox sayda yanlış cəhd (5 ardıcıl)
{
  "success":      false,
  "message":      "Çox sayda yanlış cəhd. 5 dəqiqə sonra yenidən cəhd edin.",
  "locked_until": "2024-01-01T12:05:00Z"
}
```

> 📝 5 ardıcıl yanlış cəhddən sonra hesab müvəqqəti kilidlənməlidir. `attempts_left` və `locked_until` sahələri Flutter UI üçün lazımdır.

---

### #8 — PIN Sıfırlama (Forget PIN)

> **Flow:** 2 addımlı — OTP göndər → Yeni PIN set et

#### Addım 1 — OTP Göndər

```http
POST /api/auth/pin/forgot
Content-Type: application/json
```

```json
{ "phone": "+994501234567" }
```

```json
// ✅ Response
{
  "success":    true,
  "message":    "OTP nömrənizə göndərildi.",
  "expires_in": 300
}
```

#### Addım 2 — Yeni PIN Set Et

```http
POST /api/auth/pin/reset
Content-Type: application/json
```

```json
{
  "phone":                "+994501234567",
  "otp_code":             "123456",
  "new_pin":              "4321",
  "new_pin_confirmation": "4321"
}
```

```json
// ✅ Uğurlu
{ "success": true, "message": "PIN uğurla yeniləndi." }

// ❌ OTP yanlış / müddəti bitib
{ "success": false, "message": "OTP yanlışdır və ya müddəti bitib." }

// ❌ PIN-lər uyğun deyil
{ "success": false, "message": "PIN-lər uyğun gəlmir." }

// ❌ Köhnə PIN ilə eyni
{ "success": false, "message": "Yeni PIN köhnə ilə eyni ola bilməz." }
```

> 📝 OTP müddəti `5 dəqiqə (300 saniyə)`. `expires_in` Flutter countdown timer üçün lazımdır. Yeni PIN köhnə ilə eyni ola bilməz (`#3` fix ilə uyğun). PIN: minimum 4, maksimum 6 rəqəm.

---

## ✅ Qlobal Fix Tövsiyəsi

`#1` və `#4` eyni kök problemi paylaşır. Bütün `BadHttpRequestException` tipləri üçün global handler:

```csharp
// ExceptionMiddleware.cs (nümunə — .NET)
if (exception is BadHttpRequestException badReq)
{
    context.Response.StatusCode = 200;
    await context.Response.WriteAsJsonAsync(new {
        success = false,
        message = badReq.Message
    });
    return;
}
```

> ⚠️ Yuxarıdakı pattern PHP/Laravel, Node.js, Go və s. üçün ekvivalent şəkildə tətbiq edilməlidir.

---

*🕸️ Auth Service · 4 Bug · 4 New Endpoint · Backend Team*
