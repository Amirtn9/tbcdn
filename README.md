<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
"آموزش ساخت کانفیگ  سی دی ان در پاسارگارد"
</head>
<body>

<h1>آموزش ساخت کانفیگ TB-Cdn در پاسارگارد</h1>

<h2>توضیح مهم:</h2>
<p>برای اتصال درست، باید دو ساب‌دامنه انتخاب کنید:</p>
<ul>
  <li>دامنه اول: برای قرار گرفتن پشت کانفیگ و روشن کردن تیک پروکسی</li>
  <li>دامنه دوم: برای اتصال سرور به پنل پاسارگارد</li>
</ul>

<h2>۱. گرفتن سرتیفیکت برای دامنه‌ها</h2>
<p>ابتدا برای دامنه‌هایی که می‌خواهید استفاده کنید، سرتیفیکت بگیرید.</p>
<p>یک ساب‌دامنه برای کانفیگ و یک ساب‌دامنه برای اتصال سرور ایجاد کنید.</p>

<p>برای ذخیره سرتیفیکت، پوشه مخصوص دامنه‌تان را بسازید:</p>
<pre>mkdir /var/lib/pasarguard/certs/YOURDOMAIN.COM</pre>

<p>سپس فایل‌های سرتیفیکت را در مسیر زیر قرار دهید تا پاسارگارد آن‌ها را شناسایی کند:</p>
<pre>/var/lib/pasarguard/certs/</pre>

<h2>۲. ایجاد هسته جدید در پنل پاسارگارد</h2>
<p>به پنل بروید و یک هسته جدید بسازید. کانفیگ زیر را در بخش اینباند قرار دهید:</p>
<pre>{
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
}</pre>

<h2>۳. نصب PG-Node روی سرور</h2>
<p>دستور نصب:</p>
<pre>sudo bash -c "$(curl -sL https://github.com/PasarGuard/scripts/raw/main/pg-node.sh)" @ install</pre>

<p>مسیر سرتیفیکت‌ها:</p>
<pre>
سرتیفیکت اصلی:
/var/lib/pasarguard/certs/sub.yourdomain.com/fullchain.pem

کلید خصوصی اتصال سرور به پنل:
/var/lib/pasarguard/certs/connecting.fibonet.shop/privkey.pem
</pre>

<p>پورت‌های مجاز HTTPS:</p>
<pre>443, 2053, 8443, 2083, 2087, 2096, 3087, 3096</pre>

<p>پورت‌های مجاز HTTP:</p>
<pre>80, 8080, 8880, 4040, 4220</pre>

<h2>۴. اتصال نود به پنل پاسارگارد</h2>
<p>در پنل، به مسیر گره‌ها (Nodes) بروید و یک نود جدید بسازید.</p>
<p>کلید API تولیدشده را داخل سرور وارد کنید.</p>
<p>بعد از نصب سرتیفیکت، فایل‌ها را از سرور کپی کرده و داخل پنل قرار دهید.</p>
<p>برای دامنه، آدرس یکی از دامنه‌هایی که تیک پروکسی آن روشن نیست را انتخاب کنید.</p>

<h2>۵. اضافه کردن کانفیگ به گروه و هاست</h2>
<p>کانفیگ را به گروه و هاست اضافه کنید.</p>
<p>بعد از تست اتصال، تیک پروکسی دامنه اول را روشن کنید.</p>
<p>ساب‌دامنه اتصال بین سرورها، تیک پروکسی نداشته باشد.</p>

<h2>۶. ساخت کانفیگ آنلاین</h2>
<p>همچنین می‌توانید وارد وب‌سایت 
<a href="https://azavaxhuman.github.io/DDS-Xray-Inbound-Generator/" target="_blank">DDS Xray Inbound Generator</a>
شوید و کانفیگ دلخواه خود را بسازید. هنگام ساخت کانفیگ، از پورت‌های مجاز HTTP استفاده کنید تا اتصال درست انجام شود.</p>

</body>
</html>
