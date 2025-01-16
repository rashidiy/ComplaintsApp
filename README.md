# DigitalOcean Free Trial Offer

## Tavsif
DigitalOcean platformasida ro'yxatdan o'tish orqali siz $200 miqdoridagi kreditga ega bo'lishingiz mumkin. Ushbu kreditni platformadagi xizmatlarni sinab ko'rish uchun ishlatishingiz mumkin. Quyidagi ko'rsatmalarga amal qilib, siz tez va oson ro'yxatdan o'tishingiz mumkin.

---

## Shartlar
- Ro'yxatdan o'tish uchun quyidagilar talab qilinadi:
  - **Visa kredit yoki debet kartasi**.
  - Kartada kamida **$5 miqdorida mablag'** bo'lishi lozim.
  - Ushbu mablag' **yechib olinmaydi**, faqat kartaning haqiqiyligini tasdiqlash uchun zarur.

---

## Ro'yxatdan O'tish
1. Quyidagi havolani bosing:  
   [DigitalOcean Free Trial Offer](https://try.digitalocean.com/freetrialoffer/?utm_medium=affiliates&utm_source=impact&utm_campaign=4941389&gad_source=1&gclid=Cj0KCQiA-aK8BhCDARIsAL_-H9nWMvoGWf7c49HSgOSiL0_KZLOaLDjHdwtMcJEj0EntvWP78g1JhioaAvjWEALw_wcB)
2. Ro'yxatdan o'tish shaklini to'ldiring.
3. Kredit yoki debet kartangiz ma'lumotlarini kiriting.
4. Tasdiqlash jarayonini yakunlang.

---

## Foydali Tafsilotlar
- Sizga taqdim etilgan **$200 kredit** 60 kun davomida amal qiladi.
- Kreditni quyidagi xizmatlarda ishlatishingiz mumkin:
  - Virtual serverlar (Droplets)
  - Saqlash xizmatlari (Spaces)
  - Ma'lumotlar bazalari (Managed Databases)
  - Ko'plab boshqa xizmatlar.

---

## Bog'lanish
Agar sizda savollar bo'lsa, [DigitalOcean qo'llab-quvvatlash xizmatiga](https://www.digitalocean.com/support) murojaat qiling.

---

**Eslatma**: Ushbu promo havola orqali ro'yxatdan o'tish sizga bonus sifatida $200 kredit taqdim etadi. Faqat yuqoridagi shartlarni bajarish orqali undan foydalanishingiz mumkin.




# Server sozlash bo'yicha qo'llanma

## Boshlang'ich sozlash

1. **Serverga ulanish**:
   ```bash
   ssh root@<ip-adres>
   (yes)
   password
   ```

2. **Loyihani yuklash**:
   ```bash
   git clone <repository.git>
   cd ComplaintsApp
   ```

3. **Python versiyasini tekshirish**:
   ```bash
   python3 -V
   ```

4. **Virtual muhitni o'rnatish va sozlash**:
   ```bash
   apt install python3.12-venv
   python3 -m venv .venv
   .venv/bin/pip install -r req.txt
   ```

5. **`.env` faylini sozlash**:
   ```bash
   nano .env
   ```
   Quyidagilarni qo'shing:
   ```env
   DJANGO_SECRET_KEY=<sizning_secret_keyingiz>
   BOT_TOKEN=<sizning_bot_tokeningiz>

   POSTGRES_HOST=localhost
   POSTGRES_PORT=5432
   POSTGRES_DB=c1_db
   POSTGRES_USER=c1_admin
   POSTGRES_PASSWORD=<sizning_parolingiz>
   ```
   Saqlash uchun: `Ctrl+X`, `Y`, `Enter`.

## Redis va PostgreSQL ni o'rnatish

1. **Redis**:
   ```bash
   apt install redis
   ```

2. **PostgreSQL**:
   ```bash
   apt install postgresql
   ```

3. **PostgreSQL sozlash**:
   ```bash
   sudo -su postgres
   psql
   CREATE ROLE c1_admin WITH LOGIN PASSWORD 'your_password';
   CREATE DATABASE c1_db OWNER c1_admin;
   \q
   ```

4. **Kerakli kutubxonani o'rnatish**:
   ```bash
   .venv/bin/pip install psycopg2-binary
   ```

## Django sozlash va migratsiya

1. **Migratsiyalarni ishga tushirish**:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

## Botni ishga tushirish uchun servis fayl yaratish

1. **Servis faylini yaratish**:
   ```bash
   nano /etc/systemd/system/complaints-app-bot.service
   ```

   Faylga quyidagilarni qo'shing:
   ```ini
   [Unit] 
   Description=ComplaintsApp Bot Service
   After=network.target

   [Service]
   ExecStart=/root/ComplaintsApp/.venv/bin/python /root/ComplaintsApp/manage.py runbot
   WorkingDirectory=/root/ComplaintsApp
   Restart=always
   User=root
   Group=root

   [Install]
   WantedBy=multi-user.target
   ```

2. **Servisni faollashtirish va ishga tushirish**:
   ```bash
   sudo systemctl enable complaints-app-bot.service
   sudo systemctl start complaints-app-bot.service
   sudo systemctl status complaints-app-bot.service
   ```

## Django serverni ishga tushirish uchun servis fayl yaratish

1. **Servis faylini yaratish**:
   ```bash
   nano /etc/systemd/system/complate-runserver.service
   ```

   Faylga quyidagilarni qo'shing:
   ```ini
   [Unit] 
   Description=ComplaintsApp Runserver Service
   After=network.target

   [Service]
   ExecStart=/root/ComplaintsApp/.venv/bin/python /root/ComplaintsApp/manage.py runserver 0.0.0.0:80
   WorkingDirectory=/root/ComplaintsApp
   Restart=always
   User=root
   Group=root

   [Install]
   WantedBy=multi-user.target
   ```

2. **Servisni faollashtirish va ishga tushirish**:
   ```bash
   sudo systemctl enable complate-runserver.service
   sudo systemctl start complate-runserver.service
   sudo systemctl status complate-runserver.service
   ```

## Django admin panel uchun superuser yaratish

1. **Superuser yaratish**:
   ```bash
   cd ComplaintsApp
   .venv/bin/python manage.py createsuperuser
   ```
   Foydalanuvchi nomi va parolni kiriting.

## Loyihaning muvaffaqiyatli ishlashini tekshirish

Agar barcha servislar `active` bo'lsa, loyiha muvaffaqiyatli ishga tushgan bo'ladi. Django admin paneli orqali loyihani boshqarishingiz mumkin. ✅
