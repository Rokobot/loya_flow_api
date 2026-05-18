# 🚀 Loya Mobil Tətbiqi — Müştəri Auth (Qeydiyyat, Giriş & Şifrə) API Spesifikasiyası

Bu sənəd **Loya** mobil tətbiqində **standart istifadəçinin (müştərinin)** qeydiyyat, giriş, PIN kod təyini, şifrə sıfırlama (unutdum) və tətbiq daxilindən cari şifrəni dəyişmə (change password) axışlarını və müvafiq **Sorğu (Request) JSON** modellərini tam və aydın şəkildə əhatə edir.

---

## 🔄 BÖLMƏ 1: ARDICILLIQ DİAQRAMLARI (SEQUENCE FLOWS)

### AXISH 1: Yeni İstifadəçi Qeydiyyatı (Registration)
```mermaid
sequenceDiagram
    autonumber
    actor App as Mobil Tətbiq (Müştəri)
    participant BE as Backend API (Server)
    participant SMS as SMS Gateway

    %% Step 1: Send OTP
    Note over App, BE: STEP 1: OTP SMS Sorğusu
    App->{BE}: POST /api/auth/register/send-otp (application/json)
    Note right of App: Request: { phone_number }
    BE-->>SMS: 6 rəqəmli OTP kod generasiya et və göndər
    SMS-->>App: SMS vasitəsilə 6 rəqəmli OTP kod çatır
    BE-->>App: Response: 200 OK { success, expires_in }

    %% Step 2: Verify OTP
    Note over App, BE: STEP 2: OTP Kodunun Təsdiqlənməsi
    App->{BE}: POST /api/auth/register/verify-otp (application/json)
    Note right of App: Request: { phone_number, otp_code }
    alt OTP Doğrudur
        BE-->>App: Response: 200 OK { success, verification_token }
    else OTP Yanlışdır
        BE-->>App: Response: 400 Bad Request { success, error_code: "INVALID_OTP" }
    end

    %% Step 3: Complete Register
    Note over App, BE: STEP 3: Şəxsi Məlumatlar və Şifrənin Yazılması
    App->{BE}: POST /api/auth/register/complete (application/json)
    Note right of App: Request: { verification_token, name, surname, gender, birthdate, password }
    BE-->>App: Response: 201 Created { success, access_token, user_profile }
```

### AXISH 2: Giriş (Login) & PIN Girişi
```mermaid
sequenceDiagram
    autonumber
    actor App as Mobil Tətbiq (Müştəri)
    participant BE as Backend API (Server)

    %% Flow 1: Password Login
    rect rgb(240, 248, 255)
        Note over App, BE: AXISH 2.1: Standart Şifrə ilə Giriş
        App->{BE}: POST /api/auth/login (application/json)
        Note right of App: Request: { phone_number, password }
        BE-->>App: Response: 200 OK { success, access_token, user_profile }
    end

    %% Flow 2: PIN Login
    rect rgb(255, 240, 245)
        Note over App, BE: AXISH 2.2: Sürətli PIN Kod ilə Giriş / Təyini
        App->{BE}: POST /api/auth/login/pin (application/json)
        Note right of App: Request: { phone_number, pin_code }
        BE-->>App: Response: 200 OK { success, access_token, user_profile }
    end
```

### AXISH 3: Şifrənin İdarə Edilməsi (Unutdum & Sıfırlama)
```mermaid
sequenceDiagram
    autonumber
    actor App as Mobil Tətbiq (Müştəri)
    participant BE as Backend API (Server)
    participant SMS as SMS Gateway

    %% Forgot Flow
    rect rgb(245, 255, 250)
        Note over App, BE: AXISH 3.1: Şifrəni Unutdum (Giriş Ekranında)
        App->{BE}: POST /api/auth/password/forgot (application/json)
        Note right of App: Request: { phone_number }
        BE-->>SMS: OTP SMS Kod göndər
        SMS-->>App: 6 rəqəmli OTP çatır (məs: 654321)
        BE-->>App: Response: 200 OK { success, expires_in }

        App->{BE}: POST /api/auth/password/verify (application/json)
        Note right of App: Request: { phone_number, otp_code }
        BE-->>App: Response: 200 OK { success, verification_token }

        App->{BE}: POST /api/auth/password/reset (application/json)
        Note right of App: Request: { verification_token, new_password }
        BE-->>App: Response: 200 OK { success, message }
    end

    %% Change Flow
    rect rgb(255, 250, 240)
        Note over App, BE: AXISH 3.2: Daxildən Şifrə Dəyişmə (Ayarlar - SetPasswordScreen)
        App->{BE}: POST /api/auth/password/change (application/json)
        Note right of App: Headers: Authorization Bearer access_token<br>Request: { current_password, new_password }
        BE-->>App: Response: 200 OK { success, message }
    end
```

---

## 📝 BÖLMƏ 2: API REQUEST JSON MODELLƏRİ & ENDPOINTS

Bütün sorğuların (Requests) göndərilmə formatı `Content-Type: application/json` olmalıdır.

### 1. 📝 QEYDİYYAT (REGISTER) SORĞULARI

#### A. OTP SMS Sorğusu (Mərhələ 1)
* **Endpoint:** `POST /api/auth/register/send-otp`
```json
{
  "phone_number": "+994519876543"
}
```

#### B. OTP Doğrulanması (Mərhələ 2)
* **Endpoint:** `POST /api/auth/register/verify-otp`
```json
{
  "phone_number": "+994519876543",
  "otp_code": "654321"
}
```

#### C. Qeydiyyatın Tamamlanması (Mərhələ 3)
* **Endpoint:** `POST /api/auth/register/complete`
```json
{
  "verification_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwaG9uZV9udW1iZXIiOiIrOTk0NTE5ODc2NTQzIn0...",
  "name": "Ali",
  "surname": "Aliyev",
  "gender": "male",
  "birthdate": "1998-05-18",
  "password": "Password123!"
}
```

---

### 2. 🔑 GİRİŞ (LOGIN) SORĞULARI

#### A. Standart Şifrə ilə Giriş (Password Login)
* **Endpoint:** `POST /api/auth/login`
```json
{
  "phone_number": "+994519876543",
  "password": "Password123!"
}
```

#### B. Sürətli PIN Kod ilə Giriş (PIN Login / PIN Setup)
* **Endpoint:** `POST /api/auth/login/pin`
```json
{
  "phone_number": "+994519876543",
  "pin_code": "0000"
}
```

---

### 🔄 3. ŞİFRƏNİ UNUTDUM (FORGOT PASSWORD) SORĞULARI

#### A. Şifrə Sıfırlama OTP Sorğusu
* **Endpoint:** `POST /api/auth/password/forgot`
```json
{
  "phone_number": "+994519876543"
}
```

#### B. Şifrə Sıfırlama OTP-sinin Təsdiqlənməsi
* **Endpoint:** `POST /api/auth/password/verify`
```json
{
  "phone_number": "+994519876543",
  "otp_code": "654321"
}
```

#### C. Yeni Şifrənin Təyin Edilməsi (Reset Complete)
* **Endpoint:** `POST /api/auth/password/reset`
```json
{
  "verification_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwaG9uZV9udW1iZXIiOiIrOTk0NTE5ODc2NTQzIn0...",
  "new_password": "NewSecurePassword123!"
}
```

---

### 🛠️ 4. DAXİLDƏN ŞİFRƏ DƏYİŞMƏ (CHANGE PASSWORD) SORĞUSU

#### A. Cari və Yeni Şifrə ilə Şifrə Dəyişdirilməsi (Ayarlar Ekranı)
* **Endpoint:** `POST /api/auth/password/change`
* **Headers:**
  ```http
  Authorization: Bearer <access_token>
  ```
* **Sorğu JSON Model:**
```json
{
  "current_password": "OldPassword123!",
  "new_password": "NewSecurePassword123!"
}
```
* **Request Parametrləri:**
  * `current_password` *(String / Mütləqdir)*: Müştərinin hazırkı cari şifrəsi.
  * `new_password` *(String / Mütləqdir)*: Müştərinin təyin etmək istədiyi yeni təhlükəsiz şifrə.
