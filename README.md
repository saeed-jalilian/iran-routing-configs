<div dir="rtl">

# کانفیگ‌های مسیر‌دهی برای ایران

مجموعه‌ای از کانفیگ‌ها برای کلاینت‌های پروکسی که سایت‌ها و سرویس‌های ایرانی را **مستقیم** و بقیه‌ی ترافیک را از **پروکسی** عبور می‌دهند، و تبلیغات و بدافزار را مسدود می‌کنند.

[English version below](#english)

## کلاینت‌ها

| کلاینت | پلتفرم | پوشه | وضعیت |
|---|---|---|---|
| Shadowrocket | iOS، iPadOS، macOS (Apple Silicon) | [`shadowrocket/`](shadowrocket/) | ✅ آماده |
| Streisand | iOS، macOS | — | به‌زودی |

راهنمای نصب، تنظیمات پیشنهادی اپ و عیب‌یابی هر کلاینت در README پوشه‌ی همان کلاینت است.

## اصول مشترک همه‌ی کانفیگ‌ها

- دامنه‌های `.ir`، دامنه‌های میزبانی‌شده در ایران و رنج IPهای ایران مستقیم (DIRECT) می‌روند.
- سایت‌هایی که IP ایران را تحریم کرده‌اند همیشه از پروکسی می‌روند.
- بدافزار، ماینرهای مرورگری و تبلیغات (با یک لیست سبک) مسدود می‌شوند.
- قوانین IP بدون DNS lookup اجرا می‌شوند، IPv6 خاموش است و DNS بدون فیلتر و رمزگذاری‌شده است.
- لیست‌ها از منابعی گرفته می‌شوند که هر روز بروز می‌شوند. جزئیات منابع در README هر کلاینت آمده است.

> ⚠️ این ریپو فقط قوانین مسیر‌دهی دارد. هیچ سرور، رمز یا لینک اشتراکی را به آن اضافه نکنید.

</div>

---

<a id="english"></a>

# Iran routing configs

Configs for proxy clients that route Iranian sites and services **DIRECT**, everything else through your **PROXY**, and block ads and malware.

## Clients

| Client | Platform | Folder | Status |
|---|---|---|---|
| Shadowrocket | iOS, iPadOS, macOS (Apple Silicon) | [`shadowrocket/`](shadowrocket/) | ✅ Ready |
| Streisand | iOS, macOS | — | Planned |

Each client folder has its own README with install steps, recommended app settings and troubleshooting.

## Shared principles

- `.ir` domains, Iran-hosted domains and Iranian IP ranges go DIRECT.
- Sites that refuse Iranian IPs always go through the proxy.
- Malware, browser cryptominers and ads are blocked, using a lightweight ad list.
- IP rules never trigger DNS lookups. IPv6 is off, and DNS is encrypted and non-filtering.
- Lists come from sources that are rebuilt daily. Each client's README lists the sources.

> ⚠️ This repo holds routing rules only. Never commit servers, passwords or subscription links.
