<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>آموزش ساخت کانفیگ TB-Cdn در پاسارگارد</title>
<style>
    body {
        font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
        line-height: 1.6;
        background-color: #f4f6f8;
        color: #333;
        padding: 20px;
    }
    h1, h2, h3 {
        color: #2c3e50;
    }
    h1 {
        text-align: center;
        margin-bottom: 30px;
    }
    ol {
        margin-left: 20px;
    }
    pre {
        background-color: #2d2d2d;
        color: #f8f8f2;
        padding: 15px;
        border-radius: 8px;
        overflow-x: auto;
    }
    code {
        font-family: Consolas, monospace;
    }
    .section {
        background-color: #fff;
        padding: 20px;
        margin-bottom: 20px;
        border-radius: 10px;
        box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    a {
        color: #2980b9;
        text-decoration: none;
    }
    a:hover {
        text-decoration: underline;
    }
</style>
</head>
<body>

<h1>آموزش ساخت کانفیگ TB-Cdn در پاسارگارد</h1>

<div class="section">
    <h2>1. گرفتن سرتیفیکت برای دامنه‌ها</h2>
    <ol>
        <li>ابتدا برای دامنه‌هایی که می‌خواید استفاده کنید، سرتیفیکت بگیرید.</li>
        <li>یک ساب‌دامنه برای استفاده در کانفیگ و یک ساب‌دامنه برای اتصال سرور به پنل پاسارگارد ایجاد کنید.</li>
        <li>می‌تونید از اسکریپت آماده برای گرفتن سرتیفیکت استفاده کنید. بعد از گرفتن، فایل‌های سرتیفیکت رو به مسیر زیر منتقل کنید تا مرزبان سرتیفیکت (Certificate Manager) بخونه:
            <pre><code>/var/lib/pasarguard/certs/</code></pre>
        </li>
        <li>این کار رو برای هر دو دامنه انجام بدید.</li>
    </ol>
</div>

<div class="section">
    <h2>2. ایجاد هسته جدید در پنل پاسارگارد</h2>
    <p>به پنل برید و یک هسته جدید بسازید. کانفیگ زیر رو در بخش اینباند قرار بدید و ذخیره کنید:</p>
    <pre><code>{
  "tag": "VLESS+WS+NONE+4220",
  "listen": "0.0.0.0",
  "port": 4220,
  "protocol": "vless",
  "settings": {
    "clients": [],
    "decryption": "none"
  },
  "streamSettings": {
    "network": "ws",
    "security": "none",
    "wsSettings": {
      "acceptProxyProtocol": false,
      "path": "/admin?ed=1024",
      "host": "yourdomain.com",
      "headers": {}
    }
  },
  "sniffing": {
    "enabled": false
  }
}</code></pre>
</div>

<div class="section">
    <h2>3. نصب PG-Node روی سرور</h2>
    <ol>
        <li>وارد سروری که می‌خواید کانفیگ روش بسازید بشید و دستور زیر رو اجرا کنید:
            <pre><code>sudo bash -c "$(curl -sL https://github.com/PasarGuard/scripts/raw/main/pg-node.sh)" @ install</code></pre>
        </li>
        <li>در مراحل نصب، مسیر سرتیفیکت‌ها از شما پرسیده می‌شود:
            <pre><code>سرتیفیکت اصلی:
/var/lib/pasarguard/certs/sub.yourdomain.com/fullchain.pem

کلید خصوصی اتصال سرور به پنل:
/var/lib/pasarguard/certs/connecting.fibonet.shop/privkey.pem</code></pre>
        </li>
        <li>اگر PG-Node از شما پورت پرسید، یکی از پورت‌های مجاز HTTPS رو انتخاب کنید:
            <pre><code>443, 2053, 8443, 2083, 2087, 2096, 3087, 3096</code></pre>
        </li>
    </ol>
</div>

<div class="section">
    <h2>4. اتصال نود به پنل پاسارگارد</h2>
    <ol>
        <li>در پنل، به مسیر <strong>گره‌ها / Nodes</strong> برید و یک نود جدید بسازید.</li>
        <li>کلید API که ایجاد شده رو کپی کرده و داخل سرور وارد کنید.</li>
        <li>بعد از نصب سرتیفیکت، فایل‌ها رو از سرور کپی کنید و داخل پنل قرار بدید.</li>
        <li>برای دامنه، آدرس یکی از دامنه‌هایی که قرار نیست تیک پروکسیش روشن باشه رو انتخاب کنید.</li>
    </ol>
</div>

<div class="section">
    <h2>5. اضافه کردن کانفیگ به گروه و هاست</h2>
    <ol>
        <li>کانفیگ رو به گروه و هاست اضافه کنید.</li>
        <li>بعد از تست اتصال، تیک پروکسی دامنه اول (که پشت کانفیگ استفاده شده) رو روشن کنید.</li>
        <li>ساب‌دامنه‌ای که برای اتصال بین سرورها هست، تیک پروکسیش روشن نشه.</li>
    </ol>
</div>

</body>
</html>
