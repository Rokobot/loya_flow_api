# 🤝 Loya Partner Login, Password Reset & Change — Backend API Spec

Bu sənəd **Loya** tətbiqində **Partnyorların (Biznes/Filial sahiblərinin)** Giriş (Login), Şifrə Sıfırlama (Forgot Password) və Daxildən Şifrə Dəyişmə (Change Password) axışlarını və bu zaman backend-ə göndəriləcək **Request (Sorğu) JSON** modellərini əhatə edir.

> ⚠️ **Qeyd:** Partnyorların qeydiyyatı (register) tətbiqdə yox, **Admin Paneldə** həyata keçirilir. Partnyor ilk dəfə tətbiqə Admin Panel tərəfindən verilən keçici şifrə ilə daxil olur.

---

## 1. 🔄 Partnyor Giriş & Şifrə Axışları (Sequence Flows)

### AXISH 1: Giriş və Sürətli PIN Girişi (Login & PIN Login)
```mermaid
sequenceDiagram
    autonumber
    actor Partner as Partnyor Tətbiqi (Frontend)
    participant BE as Backend API (Server)

    %% Flow 1: Simple Password Login
    rect rgb(240, 248, 255)
        Note over Partner, BE: AXISH 1.1: Standart Şifrə ilə Giriş
        Partner->{BE}: POST /api/partner/auth/login (application/json)
        Note right of Partner: Request: { phone_number, password }
        
        alt İlk Girişdir (Keçici şifrə ilə)
            BE-->>Partner: Response: 200 OK { success, is_first_login: true, temp_token }
            Note over Partner: Tətbiq partnyoru yeni şifrə təyini ekranına yönləndirir
        else Normal Girişdir
            BE-->>Partner: Response: 200 OK { success, is_first_login: false, access_token, partner_profile }
        end
    end

    %% Flow 2: Quick PIN Login
    rect rgb(255, 240, 245)
        Note over Partner, BE: AXISH 1.2: Sürətli PIN Kod ilə Giriş
        Partner->{BE}: POST /api/partner/auth/login/pin (application/json)
        Note right of Partner: Request: { phone_number, pin_code }
        BE-->>Partner: Response: 200 OK { success, access_token, partner_profile }
    end
```

### AXISH 2: Şifrəni Unutdum (Forgot Password)
```mermaid
sequenceDiagram
    autonumber
    actor Partner as Partnyor Tətbiqi (Frontend)
    participant BE as Backend API (Server)
    participant SMS as SMS Gateway

    Note over Partner, BE: Şifrəmi Unutdum Axışı (Giriş Ekranında)
    Partner->{BE}: POST /api/partner/auth/password/forgot (application/json)
    Note right of Partner: Request: { phone_number }
    BE-->>SMS: OTP SMS Kod göndər
    SMS-->>Partner: 6 rəqəmli OTP çatır (məs: 654321)
    BE-->>Partner: Response: 200 OK { success, expires_in }

    Partner->{BE}: POST /api/partner/auth/password/verify (application/json)
    Note right of Partner: Request: { phone_number, otp_code }
    BE-->>Partner: Response: 200 OK { success, verification_token }

    Partner->{BE}: POST /api/partner/auth/password/reset (application/json)
    Note right of Partner: Request: { verification_token, new_password }
    BE-->>Partner: Response: 200 OK { success, message }
```

### AXISH 3: Daxildən Şifrə Dəyişmə (Set / Change Password)
```mermaid
sequenceDiagram
    autonumber
    actor Partner as Partnyor Tətbiqi (Ayarlar)
    participant BE as Backend API (Server)

    Note over Partner, BE: Daxildə Şifrə Dəyişmə (SetPasswordScreen)
    Partner->{BE}: POST /api/partner/auth/password/change (application/json)
    Note right of Partner: Headers: Authorization Bearer access_token<br>Request: { current_password, new_password }
    BE-->>Partner: Response: 200 OK { success, message }
```

---

## 2. 📝 API Request JSON Modelləri & Uç Nöqtələri (Endpoints)

Bütün sorğuların (Requests) göndərilmə formatı `Content-Type: application/json` olmalıdır.

### 🔑 BÖLMƏ 1: GİRİŞ (LOGIN) SORĞULARI

#### A. Şifrə ilə Giriş (İlk və ya Normal Giriş)
* **Endpoint:** `POST /api/partner/auth/login`
* **Sorğu JSON Model:**
```json
{
  "phone_number": "+994519876543",
  "password": "Password123!"
}
```
* **Request Parametrləri:**
  * `phone_number` *(String / Mütləqdir)*: Partnyorun beynəlxalq formatda telefon nömrəsi.
  * `password` *(String / Mütləqdir)*: Giriş şifrəsi.

---

#### B. Sürətli PIN Kod ilə Giriş (PIN Login)
* **Endpoint:** `POST /api/partner/auth/login/pin`
* **Sorğu JSON Model:**
```json
{
  "phone_number": "+994519876543",
  "pin_code": "1234"
}
```
* **Request Parametrləri:**
  * `phone_number` *(String / Mütləqdir)*: Partnyorun telefon nömrəsi.
  * `pin_code` *(String / Mütləqdir)*: 4 rəqəmli sürətli giriş kodu (məs: `"1234"`).

---

### 🔄 BÖLMƏ 2: ŞİFRƏNİ UNUTDUM (FORGOT PASSWORD) SORĞULARI

#### A. Şifrə Sıfırlama OTP Sorğusu
* **Endpoint:** `POST /api/partner/auth/password/forgot`
* **Sorğu JSON Model:**
```json
{
  "phone_number": "+994519876543"
}
```

---

#### B. Şifrə Sıfırlama OTP-sinin Təsdiqlənməsi
* **Endpoint:** `POST /api/partner/auth/password/verify`
* **Sorğu JSON Model:**
```json
{
  "phone_number": "+994519876543",
  "otp_code": "654321"
}
```

---

#### C. Yeni Şifrənin Təyin Edilməsi (Reset Complete)
* **Endpoint:** `POST /api/partner/auth/password/reset`
* **Sorğu JSON Model:**
```json
{
  "verification_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwaG9uZV9udW1iZXIiOiIrOTk0NTE5ODc2NTQzIn0...",
  "new_password": "NewSecurePassword123!"
}
```

---

### 🛠️ BÖLMƏ 3: DAXİLDƏN ŞİFRƏ DƏYİŞMƏ (CHANGE PASSWORD) SORĞUSU

#### A. Cari və Yeni Şifrə ilə Şifrə Dəyişdirilməsi
* **Endpoint:** `POST /api/partner/auth/password/change`
* **Məqsəd:** Sistemə daxil olmuş partnyorun profil tənzimləmələrindən cari şifrəsini doğrulayıb yeni şifrə təyin etməsi.
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
  * `current_password` *(String / Mütləqdir)*: Partnyorun hazırkı cari şifrəsi.
  * `new_password` *(String / Mütləqdir)*: Təyin etmək istədiyi yeni təhlükəsiz şifrə.
