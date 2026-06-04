Backend API Issues


🕷️
Backend API Issue Log
Auth Service · Rafet → Backend Team
🕸️ Auth Service
📍 62.171.172.254:8083
📅 2025
4
Kritik Bug
4
Yeni Endpoint
2
Auth Fix
2
PIN API
BUG-LAR — DÜZƏLDILMƏLI
1
Login — yanlış credentials-də 500 qaytarır, 200 olmalıdır
Critical
POST /api/auth/login
⌄

⚡ MÖVCUD DAVRANIŞ
Telefon nömrəsi və ya şifrə yanlış olduqda server HTTP 500 Internal Server Error qaytarır. Flutter tərəf DioExceptionType.badResponse ilə crash edir.
✅ GÖZLƏNILƏN DAVRANIŞ
Sorğu uğurla işlənməli, HTTP 200 OK + JSON body qaytarılmalıdır. success: false flag-i ilə istifadəçi dostu mesaj daxil olmalıdır.
📱 FLUTTER LOG (MÖVCUD)
╔╣ DioError ║ Status: 500 Internal Server Error ║ Time: 557 ms
║ http://62.171.172.254:8083/api/auth/login
╔ DioExceptionType.badResponse
║ BadHttpRequestException: Telefon nömrəsi və ya şifrə yanlışdır. 
// ❌ Mövcud response
HTTP 500 Internal Server Error
AuthService.Application.Exceptions.BadHttpRequestException:
  Telefon nömrəsi və ya şifrə yanlışdır.

// ✅ Olmalıdır
HTTP 200 OK
{
  "success": false,
  "message": "Telefon nömrəsi və ya şifrə yanlışdır."
}
🔧 Fix: BadHttpRequestException tipini global exception handler-də tutun. Bu exception HTTP 500-ə çevrilməməli, 200 OK + success: false + mesaj ilə qaytarılmalıdır. ExceptionMiddleware və ya GlobalExceptionHandler-də bu tipi ayrıca handle edin. 
2
Login — ilk sorğuda 500, ikinci sorğuda success olur
High
POST /api/auth/login · Race condition / cold start
⌄

📋 TƏSVIR
Credentials tamamilə doğru olsa belə, ilk login sorğusu 500 qaytarır. Flutter tərəfdə catch blokunda ikinci try işə düşür və success olur. Bu davranış server tərəfdəki bir initialization probleminə işarə edir.
SƏBƏB 1
DB connection pool ilk sorğuda hazır olmaya bilər (cold start).
SƏBƏB 2
Middleware sırası: token/session init middleware ilk sorğuda fail edə bilər.
SƏBƏB 3
Redis / cache / token store server start-da tam init olmadan sorğu gəlir.
🔧 Fix tövsiyələri: Server başladıqda bütün servislərin (DB, Redis, cache, token store) hazır olduğunu yoxlamaq üçün health check endpoint əlavə edin. Middleware sırasını nəzərdən keçirin. Connection pool warmup tətbiq edin. 
3
Eyni parol yenidən reset edilə bilir
Medium
POST /api/auth/reset-password · Güvənlik problemi
⌄

🚨 PROBLEM
Password reset endpoint-ində mövcud parol ilə eyni parolu yenidən set etmək mümkündür. Bu güvənlik riskidir — istifadəçi real mənada parolunu dəyişməmiş olur.
✅ GÖZLƏNILƏN
Yeni parol köhnə parol ilə eyni olduqda 400 Bad Request + istifadəçi dostu xəta mesajı qaytarılmalıdır.
// ❌ Mövcud — eyni parol qəbul edilir
POST /api/auth/reset-password
{ "new_password": "mövcud_parol" }
→ HTTP 200 OK  (yanlışdır)

// ✅ Olmalıdır
HTTP 200 OK
{
  "success": false,
  "message": "Yeni parol köhnə parolla eyni ola bilməz."
}
🔧 Fix: Reset əməliyyatında yeni parolu hash-ləyib mövcud hash ilə password_verify() / BCrypt.Verify() ilə müqayisə edin. Eyni olduqda success: false + mesaj qaytarın. Bu yoxlama həm #7 PIN reset-ə də tətbiq edilməlidir. 
4
Register — mövcud nömrə üçün 500 qaytarır
Critical
POST /api/auth/register
⌄

📱 FLUTTER LOG (MÖVCUD)
║ AuthService.Application.Exceptions.BadHttpRequestException:
║ Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib 
// ❌ Mövcud
HTTP 500 Internal Server Error
BadHttpRequestException: Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib.

// ✅ Olmalıdır
HTTP 200 OK
{
  "success": false,
  "message": "Bu nömrə ilə istifadəçi artıq qeydiyyatdan keçib."
}
🔧 Fix: #1 ilə eyni kök problem — BadHttpRequestException global handler-də düzgün handle edilmir. Register controller-də nömrə mövcudluğunu əvvəlcədən DB-dən yoxlayın, exception-a düşmədən 200 + success: false qaytarın. 
YENI ENDPOINTLƏR — ƏLAVƏ EDILMƏLI
5
PUT
Müştəri nömrə dəyişmə API
New Endpoint
/api/customer/change-phone · Auth required
⌄

PUT /api/customer/change-phone
Authorization: Bearer {token}
SAHƏ	TIP	TƏLƏB	AÇIQLAMA
current_phone	string	✱ Məcburi	İstifadəçinin mövcud telefon nömrəsi
new_phone	string	✱ Məcburi	Yeni telefon nömrəsi (format: +994XXXXXXXXX)
otp_code	string	✱ Məcburi	Yeni nömrəyə göndərilən 6 rəqəmli OTP
// ✅ Success
HTTP 200 OK
{ "success": true, "message": "Nömrə uğurla dəyişdirildi." }

// ❌ OTP xəta
{ "success": false, "message": "OTP yanlışdır və ya müddəti bitib." }

// ❌ Nömrə artıq mövcuddur
{ "success": false, "message": "Bu nömrə artıq istifadədədir." }
📝 Qeyd: OTP göndərmə üçün əvvəlcə POST /api/auth/send-otp sorğusu atılmalıdır. Nömrə dəyişdikdən sonra mövcud token yenilənməlidir (yeni nömrə ilə yeni token qaytarın). 
6
GET
İstifadəçi məlumatlarını alma API
New Endpoint
/api/customer/me · Auth required
⌄

GET /api/customer/me
Authorization: Bearer {token}
Content-Type: application/json
// ✅ Success response
HTTP 200 OK
{
  "success": true,
  "data": {
    "id": 1,
    "full_name": "Əli Həsənov",
    "phone": "+994501234567",
    "email": "ali@example.com",
    "profile_image": "https://...",
    "created_at": "2024-01-01T00:00:00Z",
    "is_verified": true,
    "has_pin": true
  }
}

// ❌ Token invalid / expired
{ "success": false, "message": "Unauthorized." }
📝 Qeyd: has_pin sahəsi Flutter tərəfin PIN ekranına yönləndirmə qərarı üçün lazımdır. Token refresh lazım olduqda 401 qaytarın, 500 yox. 
7
POST
PIN doğrulama API
New Endpoint
/api/auth/pin/verify · Brute-force qoruma daxil
⌄

POST /api/auth/pin/verify
Content-Type: application/json
SAHƏ	TIP	TƏLƏB	AÇIQLAMA
phone	string	✱ Məcburi	İstifadəçi telefon nömrəsi
pin	string	✱ Məcburi	4–6 rəqəmli PIN kod
// ✅ Uğurlu doğrulama
HTTP 200 OK
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5...",
  "user": { "id": 1, "full_name": "Əli Həsənov" }
}

// ❌ Yanlış PIN
{ "success": false, "message": "PIN yanlışdır.", "attempts_left": 4 }

// ❌ Çox sayda yanlış cəhd
{
  "success": false,
  "message": "Çox sayda yanlış cəhd. 5 dəqiqə sonra yenidən cəhd edin.",
  "locked_until": "2024-01-01T12:05:00Z"
}
📝 Brute-force qoruma: Ardıcıl 5 yanlış cəhddən sonra hesab müvəqqəti kilidlənməlidir (locked_until qaytarın). attempts_left sahəsi Flutter UI üçün faydalıdır. 
8
POST
PIN sıfırlama — Forget PIN
New Endpoint
/api/auth/pin/forgot + /api/auth/pin/reset · 2 addımlı flow
⌄

Addım 1 — OTP göndər
→
Addım 2 — Yeni PIN set et
// ── Addım 1: OTP göndər ──────────────────────────
POST /api/auth/pin/forgot
{ "phone": "+994501234567" }

→ HTTP 200 OK
{ "success": true, "message": "OTP nömrənizə göndərildi.", "expires_in": 300 }

// ── Addım 2: Yeni PIN set et ─────────────────────
POST /api/auth/pin/reset
{
  "phone": "+994501234567",
  "otp_code": "123456",
  "new_pin": "4321",
  "new_pin_confirmation": "4321"
}

→ HTTP 200 OK
{ "success": true, "message": "PIN uğurla yeniləndi." }

→ OTP yanlış / müddəti bitib:
{ "success": false, "message": "OTP yanlışdır və ya müddəti bitib." }

→ PIN uyğun deyil:
{ "success": false, "message": "PIN-lər uyğun gəlmir." }

→ Köhnə PIN ilə eyni:
{ "success": false, "message": "Yeni PIN köhnə ilə eyni ola bilməz." }
📝 Qeydlər: OTP müddəti 5 dəqiqə (300 saniyə). expires_in sahəsi Flutter countdown timer üçün lazımdır. Yeni PIN köhnə ilə eyni ola bilməz (#3 fix ilə uyğun). PIN minimum 4, maksimum 6 rəqəm olmalıdır. 
🕸️ Auth Service · 4 Bug · 4 New Endpoint · Backend Team 
