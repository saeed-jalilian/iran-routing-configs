<div dir="rtl">

# کانفیگ Shadowrocket برای ایران

یک کانفیگ سبک و تمیز برای [Shadowrocket](https://apps.apple.com/app/id932747118) که سایت‌ها و سرویس‌های ایرانی را **مستقیم** و بقیه‌ی ترافیک را از **پروکسی** عبور می‌دهد، و تبلیغات، بدافزار و ماینرهای مرورگری را مسدود می‌کند.

[English version below](#english)

## چرا این کانفیگ؟

کانفیگ‌های آماده‌ی رایج معمولاً یکی از این مشکل‌ها را دارند که باعث می‌شود «بعضی وقت‌ها یک سایت کاملاً رندوم لود نشود»:

| مشکل رایج | این کانفیگ |
|---|---|
| IPv6 روشن است و روی بعضی شبکه‌ها و سرورها اتصال‌ها گیر می‌کنند | `ipv6 = false` |
| DNSهای فیلترشده و بدون فیلتر (مثل `1.1.1.1` و `1.1.1.3`) قاطی هستند و یک سایت گاهی باز و گاهی بسته است | فقط DoH بدون فیلتر (`1.1.1.1` و `dns.google`) |
| قوانین IP و `GEOIP` بدون `no-resolve` و قبل از قوانین دامنه هستند، پس برای هر اتصال DNS زده می‌شود و DNS مسموم‌شده مسیر را خراب می‌کند | همه‌ی قوانین IP با `no-resolve` هستند و `GEOIP` خاموش است |
| لیست تبلیغات صدها هزار خطی که حافظه را پر می‌کند و تونل قطع و وصل می‌شود | لیست سبک تبلیغات (حدود ۵ هزار دامنه) |
| `update-url` به کانفیگ شخص دیگری اشاره می‌کند و تغییرات شما بازنویسی می‌شود | `update-url` به همین ریپو اشاره می‌کند |

## نصب

۱. این لینک را کپی کنید:

</div>

```
https://raw.githubusercontent.com/saeed-jalilian/shadowrocket-iran/main/shadowrocket-iran.conf
```

<div dir="rtl">

۲. در Shadowrocket به تب **Config** بروید، روی **+** بزنید، لینک را paste کنید و **Download** را بزنید.

۳. روی کانفیگ دانلودشده بزنید و **Use Config** را انتخاب کنید.

۴. در صفحه‌ی اصلی، **Global Routing** را روی **Config** بگذارید.

۵. در تب Config بخش Rules را باز کنید و مطمئن شوید همه‌ی لیست‌های ریموت دانلود شده‌اند.

> نکته: `raw.githubusercontent.com` و دانلودهای `github.com` در بعضی شبکه‌های ایران فیلتر هستند. دانلود و بروزرسانی را وقتی انجام دهید که به پروکسی وصل هستید.

## تنظیمات پیشنهادی خود اپلیکیشن

| مسیر | مقدار | توضیح |
|---|---|---|
| Home → Global Routing | **Config** | با Proxy یا Direct قوانین این کانفیگ اجرا نمی‌شوند |
| Settings → TCP → TLS | **OpenSSL** | TLS 1.3، مصرف حافظه‌ی کمتر |
| Settings → TCP → Fingerprint | **Chrome140** | جدیدترین fingerprint کروم، با پشتیبانی از X25519MLKEM768 |
| Settings → TCP → Max Read Length | **16384** | پیش‌فرض |
| Settings → Test Method | **CONNECT** | واقعی‌ترین تست سالم بودن سرور |
| Settings → UDP → Disable STUN | **روشن** | جلوگیری از نشت IP از طریق WebRTC |
| Settings → UDP Relay | **فقط اگر سرور UDP پشتیبانی کند** | در غیر این صورت QUIC/HTTP3 با تأخیر به TCP برمی‌گردد |
| Settings → Include All Networks | **خاموش** | شبکه‌ی محلی و AirDrop را خراب نمی‌کند |
| Settings → Proxy Share | **خاموش** | مگر اینکه لازمش داشته باشید |

## ساختار قوانین

قوانین به ترتیب از بالا به پایین بررسی می‌شوند و اولین قانونی که مچ شود اعمال می‌شود.

| # | قانون | سیاست |
|---|---|---|
| ۱ | IPهای صفحه‌ی فیلترینگ (`ir-injected`) | REJECT |
| ۲ | شبکه‌های محلی و خصوصی | DIRECT |
| ۳ | بدافزار، ماینرهای مرورگری، تبلیغات (نسخه‌ی سبک) | REJECT |
| ۴ | سایت‌هایی که IP ایران را تحریم کرده‌اند (`sanctioned`) | PROXY |
| ۵ | دامنه‌های `.ir`، دامنه‌های میزبانی‌شده در ایران، رنج IPهای ایران | DIRECT |
| ۶ | هر چیز دیگر (`FINAL`) | PROXY |

### تنظیمات DNS

| کلید | مقدار | دلیل |
|---|---|---|
| `dns-server` | `https://1.1.1.1/dns-query, https://dns.google/dns-query` | DNS رمزگذاری‌شده و بدون فیلتر |
| `fallback-dns-server` | `system` | اگر DNS اصلی بیش از ۲ ثانیه طول بکشد |
| `dns-direct-system` | `true` | دامنه‌های ایرانی با DNS خود ISP جواب بگیرند |
| `private-ip-answer` | `false` | جواب‌های IP خصوصی (مثل `10.10.34.x`) دستکاری‌شده حساب می‌شوند و اتصال از پروکسی می‌رود |
| `ipv6` | `false` | جلوگیری از اتصال‌های نیمه‌کاره |

## قوانین اختیاری

این دو قانون در فایل کامنت شده‌اند. برای فعال کردنشان `#` اول خط را بردارید.

| قانون | چه زمانی فعالش کنید |
|---|---|
| `phishing.list` (حدود ۱۷ هزار مورد) | اگر مسدود کردن سایت‌های فیشینگ را می‌خواهید و تونل با آن پایدار می‌ماند |
| `GEOIP,IR,DIRECT` | اگر یک سایت ایرانی که در لیست‌ها نیست از پروکسی باز نمی‌شود. هزینه‌اش یک DNS lookup برای هر اتصال مچ‌نشده است |

## عیب‌یابی

اگر سایتی لود نشد:

۱. در تب **Data**، Proxy Log را روشن کنید و سایت را دوباره باز کنید.

۲. ببینید دامنه با کدام قانون مچ شده است:

| نتیجه‌ی لاگ | علت محتمل | راه‌حل |
|---|---|---|
| `REJECT` | دامنه به‌اشتباه در لیست بلاک است | خط لیست تبلیغات یا بدافزار را موقتاً کامنت کنید |
| `PROXY` ولی سایت ایرانی است | دامنه در لیست‌ها نیست یا لیست دانلود نشده | لیست‌ها را بروز کنید، یا یک قانون `DOMAIN-SUFFIX,example.com,DIRECT` بالای `FINAL` اضافه کنید |
| `DIRECT` ولی سایت خارجی است | IP به‌اشتباه ایرانی ثبت شده | یک قانون `DOMAIN-SUFFIX,example.com,PROXY` بالای قوانین ایران اضافه کنید |
| `PROXY` و سایت خارجی است ولی باز نمی‌شود | مشکل از سرور پروکسی یا UDP است | سرور دیگری را امتحان کنید، یا UDP Relay را بررسی کنید |

۳. بعد از پیدا کردن مشکل، Proxy Log را خاموش کنید.

## بروزرسانی

- **کانفیگ:** در تب Config روی کانفیگ نگه دارید و **Update** را بزنید. این کار کانفیگ را با نسخه‌ی همین ریپو جایگزین می‌کند و **تغییرات محلی پاک می‌شوند**. پس تغییرات را در گیت‌هاب بدهید، نه داخل اپ.
- **لیست‌ها:** هر روز توسط منبعشان دوباره ساخته می‌شوند و Shadowrocket خودش بروزشان می‌کند.

## منابع

لیست‌ها از پروژه‌ی [aliraghebi/routing-rules](https://github.com/aliraghebi/routing-rules) گرفته شده‌اند، که هر روز دوباره ساخته می‌شود و این منابع را ترکیب می‌کند:

- دامنه‌های ایرانی: [bootmortis/iran-hosted-domains](https://github.com/bootmortis/iran-hosted-domains)، [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)
- رنج IPهای ایران: [RIPEstat](https://stat.ripe.net/)، [IP Geolocation by DB-IP](https://db-ip.com)
- بدافزار و فیشینگ: [URLhaus](https://urlhaus.abuse.ch/) از طریق [malware-filter](https://gitlab.com/malware-filter)
- سایت‌های تحریمی: لیست‌های Dynx anti-ban و Filteryab
- IPهای صفحه‌ی فیلترینگ: [Chocolate4U/Iran-v2ray-rules](https://github.com/Chocolate4U/Iran-v2ray-rules)

این ریپو فقط به لیست‌ها لینک می‌دهد و محتوای آن‌ها را کپی نمی‌کند. مجوز هر لیست متعلق به منبع خودش است.

> ⚠️ این فایل فقط قوانین مسیر‌دهی است و هیچ سرور یا اطلاعات اتصالی داخلش نیست. سرور را جداگانه در Shadowrocket اضافه کنید و **هرگز** بخش `[Proxy]` یا لینک سرور را به این ریپو اضافه نکنید.

</div>

---

<a id="english"></a>

# Shadowrocket config for Iran

A lean [Shadowrocket](https://apps.apple.com/app/id932747118) config that routes Iranian sites and services **DIRECT**, everything else through your **PROXY**, and blocks ads, malware and browser cryptominers.

## Why

| Common problem in ready-made configs | This config |
|---|---|
| IPv6 on, causing stalled connections on some networks/servers | `ipv6 = false` |
| Filtering and non-filtering resolvers mixed (`1.1.1.1` + `1.1.1.3`), so a site loads only sometimes | Non-filtering DoH only |
| IP / `GEOIP` rules without `no-resolve`, placed before domain rules, so every connection triggers a (possibly poisoned) DNS lookup | Every IP rule uses `no-resolve`; `GEOIP` is off |
| Huge ad lists that exhaust tunnel memory | Lightweight ad list (~5k domains) |
| `update-url` pointing to someone else's file, overwriting your edits | `update-url` points to this repo |

## Install

1. Copy this URL:
   ```
   https://raw.githubusercontent.com/saeed-jalilian/shadowrocket-iran/main/shadowrocket-iran.conf
   ```
2. Shadowrocket → **Config** → **+** → paste → **Download**.
3. Tap the config → **Use Config**.
4. Home → **Global Routing** → **Config**.
5. Config → Rules: make sure every remote list downloaded.

GitHub may be filtered on some Iranian networks, so download and update while connected to your proxy.

## Recommended app settings

| Path | Value |
|---|---|
| Home → Global Routing | **Config** |
| Settings → TCP → TLS | **OpenSSL** |
| Settings → TCP → Fingerprint | **Chrome140** |
| Settings → TCP → Max Read Length | **16384** |
| Settings → Test Method | **CONNECT** |
| Settings → UDP → Disable STUN | **On** |
| Settings → UDP Relay | **On only if your server supports UDP** |
| Settings → Include All Networks | **Off** |
| Settings → Proxy Share | **Off** |

## Rule order

| # | Rule | Policy |
|---|---|---|
| 1 | Iran DNS block-page IPs (`ir-injected`) | REJECT |
| 2 | Local / private networks | DIRECT |
| 3 | Malware, cryptominers, ads (lite) | REJECT |
| 4 | Sites that refuse Iranian IPs (`sanctioned`) | PROXY |
| 5 | `.ir`, Iran-hosted domains, Iranian IP ranges | DIRECT |
| 6 | Everything else (`FINAL`) | PROXY |

## Optional rules

These are commented out in the file. Remove the leading `#` to enable them.

- `phishing.list` (~17k entries): enable it if you want phishing blocking and the tunnel stays stable with it.
- `GEOIP,IR,DIRECT`: enable it if an Iranian site that isn't on the lists fails through the proxy. It costs one DNS lookup per unmatched connection.

## Troubleshooting

1. Turn on the proxy log under **Data** and reload the failing site.
2. Check which rule the domain matched, then fix it based on the table below.
3. Turn the log off when you're done.

| Matched | Likely cause | Fix |
|---|---|---|
| `REJECT` | False positive in a block list | Comment out that list temporarily |
| `PROXY`, Iranian site | Domain not listed, or the list failed to download | Update the lists, or add `DOMAIN-SUFFIX,example.com,DIRECT` above `FINAL` |
| `DIRECT`, foreign site | IP misclassified as Iranian | Add `DOMAIN-SUFFIX,example.com,PROXY` above the Iran rules |
| `PROXY`, foreign site still fails | Server or UDP issue | Try another server, check UDP Relay |

## Updating

**Update** on the config replaces it with this repo's version, and any local edits are lost, so edit the file here on GitHub. The rule lists are rebuilt daily upstream, and Shadowrocket refreshes them on its own.

## Credits

The rule lists come from [aliraghebi/routing-rules](https://github.com/aliraghebi/routing-rules), which merges:

- [bootmortis/iran-hosted-domains](https://github.com/bootmortis/iran-hosted-domains)
- [v2fly/domain-list-community](https://github.com/v2fly/domain-list-community)
- [RIPEstat](https://stat.ripe.net/)
- [IP Geolocation by DB-IP](https://db-ip.com)
- [URLhaus](https://urlhaus.abuse.ch/) / [malware-filter](https://gitlab.com/malware-filter)
- Dynx anti-ban
- [Chocolate4U/Iran-v2ray-rules](https://github.com/Chocolate4U/Iran-v2ray-rules)

This repo only links to those lists and does not redistribute them. Each list is under its own source's license.

> ⚠️ This file holds routing rules only, with no server or credentials. Add your server in Shadowrocket separately, and never commit a `[Proxy]` section or server links here.
