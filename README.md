# 1️⃣ Pull یا clone آخرین نسخه مخزن
git clone https://github.com/username/daberna-farman.git
cd daberna-farman

# 2️⃣ راه‌اندازی سرویس‌ها (DB, Redis, MinIO, backend)
docker compose up -d

# 3️⃣ اعمال مهاجراسیون‌ها و seed اولیه
docker compose exec backend alembic upgrade head
docker compose exec backend python manage.py seed_cards

# 4️⃣ ساخت backend و اجرای تست‌ها
docker build -t daberna-backend ./repo/backend
docker run --rm -e DATABASE_URL="$DATABASE_URL" daberna-backend pytest -q

# 5️⃣ ساخت Flutter APK و امضا نهایی
cd repo/flutter-app
flutter pub get
flutter build apk --release
../../ci/sign_apk.sh# -
