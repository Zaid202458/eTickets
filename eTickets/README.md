# eTickets (ASP.NET Core MVC)

مشروع ويب لإدارة تذاكر الأفلام (أفلام، ممثلون، منتجون، دور سينما) مع سلة مشتريات ونظام طلبات ومصادقة وأدوار.

## المزايا الرئيسية
- **إدارة الكيانات**: أفلام، ممثلون، منتجون، دور سينما.
- **علاقات**: ربط متعدد ممثلين–أفلام عبر `Actor_Movie`.
- **سلة مشتريات جلسية** (Session) مع حساب الإجمالي.
- **إنشاء الطلبات** وربط عناصر الطلب بالمستخدم.
- **مصادقة وهوية** باستخدام ASP.NET Core Identity مع أدوار: Admin, User.
- **تهيئة بيانات تلقائية** (Seeding) للكيانات والأدوار/المستخدمين.

## التقنيات المستخدمة
- ASP.NET Core MVC (.NET 5)
- Entity Framework Core + SQL Server
- ASP.NET Core Identity
- Razor Views, Bootstrap

## المتطلبات
- .NET 5 SDK
- SQL Server (محلي أو بعيد)

## الإعداد السريع
1) حدّث سلسلة الاتصال في `appsettings.json` ضمن المفتاح: `DefaultConnectionString`.
2) استرجاع الحزم:
```bash
dotnet restore
```
3) إنشاء/تحديث قاعدة البيانات (باستخدام الهجرات الموجودة):
```bash
dotnet ef database update
```
إذا لم تكن أداة EF CLI مثبتة:
```bash
dotnet tool install --global dotnet-ef
```
4) التشغيل:
```bash
dotnet run
```
سيفتح التطبيق على العنوان الافتراضي في الإخراج (عادة `https://localhost:5001`).

## الحسابات الافتراضية (Seed)
- Admin: `admin@etickets.com` / كلمة المرور: `Coding@1234?`
- User: `user@etickets.com` / كلمة المرور: `Coding@1234?`
يتم إنشاء الأدوار والمستخدمين عبر `AppDbInitializer.SeedUsersAndRolesAsync` عند بدء التشغيل.

## ملاحظات مهمة
- المسار الافتراضي يشير إلى `Home/Index`. إذا لم يكن هناك `HomeController`، عدّل المسار الافتراضي إلى `Movies/Index` في `Startup.Configure` أو أضف متحكم Home.
- يوجد تكرار لاستدعاء `app.UseAuthorization()` في `Startup.Configure`؛ يمكن حذف التكرار.
- فلترة الأفلام تستخدم مطابقة تامة؛ لتفعيل البحث الجزئي استخدم `Contains` بدل `Equals`.

## بنية المشروع
- `Program.cs`, `Startup.cs`: التمهيد والإعداد.
- `Data/AppDbContext.cs`: السياق وربط الجداول والعلاقات.
- `Data/Services/*`: منطق الأعمال (خدمات الأفلام، الممثلين، …).
- `Data/Cart/ShoppingCart.cs`: منطق السلة المعتمد على الجلسة.
- `Controllers/*`: متحكمات MVC.
- `Views/*`: واجهات Razor.
- `Models/*`: الكيانات.
- `Migrations/*`: هجرات قاعدة البيانات.

## تشغيل بدون EF CLI (بديل)
يمكن تشغيل التطبيق مباشرة بـ `dotnet run` وسيقوم المُهيّئ بإنشاء قاعدة البيانات والجداول مبدئيًا عبر `EnsureCreated()` إذا لم تكن موجودة؛ الهجرات تبقى الموصى بها للإنتاج.

---
English (brief)
- Web app to manage movies, actors, producers, cinemas with cart, orders, and Identity roles.
- Stack: ASP.NET Core MVC (.NET 5), EF Core + SQL Server, Identity, Razor/Bootstrap.
- Setup: set `DefaultConnectionString`, `dotnet restore`, `dotnet ef database update`, `dotnet run`.
- Seeded accounts: `admin@etickets.com` / `Coding@1234?`, `user@etickets.com` / `Coding@1234?`.
