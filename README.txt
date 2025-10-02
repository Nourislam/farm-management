# ⚡ البداية السريعة - مستخدم واحد

## 🎯 الهدف
إنشاء نظام تسجيل دخول بمستخدم واحد فقط (المدير).

---

## 📝 الخطوات (5 دقائق)

### 1️⃣ تفعيل Email في Supabase
```
Supabase → Authentication → Providers → Email ✅
```

### 2️⃣ إنشاء المستخدم الوحيد
افتح **SQL Editor** في Supabase:

```sql
-- تفعيل التشفير (شغل أولاً)
CREATE EXTENSION IF NOT EXISTS pgcrypto;

-- إنشاء المستخدم
INSERT INTO auth.users (
    instance_id,
    id,
    aud,
    role,
    email,
    encrypted_password,
    email_confirmed_at,
    raw_app_meta_data,
    raw_user_meta_data,
    created_at,
    updated_at
) VALUES (
    '00000000-0000-0000-0000-000000000000',
    gen_random_uuid(),
    'authenticated',
    'authenticated',
    'admin@farm.com',
    crypt('admin123', gen_salt('bf')),
    NOW(),
    '{"provider":"email","providers":["email"]}',
    '{"role":"admin","name":"المدير"}',
    NOW(),
    NOW()
);
```

### 3️⃣ التحقق
```sql
SELECT email, created_at FROM auth.users WHERE email = 'admin@farm.com';
```
يجب أن ترى: **admin@farm.com** ✅

### 4️⃣ الملفات المطلوبة
ضع هذه الملفات في مجلد واحد:
- `index.html` (الصفحة الرئيسية - من artifact "index_protected")
- `login.html` (صفحة تسجيل الدخول - من artifact "login_page")

### 5️⃣ رفع على Netlify
```bash
# الطريقة 1: Drag & Drop
# اسحب المجلد إلى: netlify.com/drop

# الطريقة 2: GitHub
git init
git add .
git commit -m "Initial commit"
git push origin main
# ثم اربط مع Netlify
```

---

## 🧪 اختبار سريع

افتح: `https://YOUR-SITE.netlify.app/login.html`

**بيانات الدخول:**
- Email: `admin@farm.com`
- Password: `admin123`

✅ يجب أن تدخل للصفحة الرئيسية
✅ يجب أن ترى "المدير" في الأعلى
✅ زر "تسجيل خروج" يعمل

---

## 🔧 تخصيص بياناتك

### تغيير البريد الإلكتروني:
```sql
UPDATE auth.users 
SET email = 'youremail@example.com'
WHERE email = 'admin@farm.com';
```

### تغيير كلمة المرور:
```sql
UPDATE auth.users 
SET encrypted_password = crypt('your-new-password', gen_salt('bf'))
WHERE email = 'admin@farm.com';
```

### تغيير الاسم:
```sql
UPDATE auth.users 
SET raw_user_meta_data = '{"role":"admin","name":"اسمك هنا"}'
WHERE email = 'admin@farm.com';
```

---

## ⚠️ استكشاف الأخطاء

### مشكلة: لا يعمل تسجيل الدخول
**الحل:**
1. تأكد من تفعيل Email في Supabase
2. تحقق من المفاتيح في `login.html` و `index.html`:
```javascript
const SUPABASE_URL = 'https://sstvsknmmhbwubzajeix.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOi...';
```

### مشكلة: "Invalid login credentials"
**الحل:**
```sql
-- تحقق من وجود المستخدم
SELECT email, email_confirmed_at FROM auth.users 
WHERE email = 'admin@farm.com';

-- يجب أن يكون email_confirmed_at ليس NULL
```

### مشكلة: يحولني لـ login.html دائماً
**الحل:**
1. امسح الـ Cookies (Ctrl+Shift+Delete)
2. أعد تسجيل الدخول

---

## 📞 دعم إضافي

- [توثيق Supabase Auth](https://supabase.com/docs/guides/auth)
- [دعم Netlify](https://docs.netlify.com/)
- افتح Console في المتصفح (F12) لرؤية الأخطاء

---

## ✅ تم!

الآن لديك:
- ✅ مستخدم واحد (المدير)
- ✅ تسجيل دخول وخروج
- ✅ حماية تلقائية للصفحات
- ✅ واجهة جميلة بالعربية

**استمتع بإدارة مزرعتك! 🌾**
