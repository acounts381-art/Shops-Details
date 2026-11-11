# Shop Details App

Android mobile app jisme shop ki details enter ki jati hain, location aur photo ke sath save hoti hain, aur report me dekh kar Excel me export ki ja sakti hain. Data phone ke andar **SQLite** database me save hota hai.

App web (HTML/JS) me bani hai aur **Capacitor** ke zariye APK banti hai. APK **GitHub Actions** khud build karta hai.

## Features

**Shop Entry (main page)**
- Top par live photo box (camera se photo lena)
- Latitude / Longitude (disabled boxes), Refresh button aur Last update (date, time)
- Date, Depo, Dr Name, Beat, Shop Name, Address, Mobile, CNIC, Remarks, aur Save button
- Depo, Dr Name, Beat ke sath checkbox: tick ho to Save ke baad us box ka data clear nahi hota
- Depo select karne par sirf usi Depo ke Dr Name list me aate hain
- Photo lene ke baad GPS location se Shop Name aur Address map (OpenStreetMap) se khud bhar jate hain (internet chahiye)
- Mobile 11 digits, CNIC 13 digits (`XXXXX-XXXXXXX-X` format khud banta hai)

**Setup (menu)**
- Depo list
- Dr Name list (har Dr ke sath Depo select hota hai)
- Beat list (A, B, C pehle se maujood)

**Report (menu)**
- Upar Date, Depo, Dr Name, Beat filter aur Search box
- Table: Sr, Date, Shop Name, Address, Mobile, CNIC, Depo, Dr Name, Beat, Latitude, Longitude, Pic, Remarks
- Export Excel button: filter ki hui records ki `.xlsx` file banti hai, share menu se save/send karein

## Folder structure

```
.
├── .github/workflows/build-apk.yml   # APK build ka workflow
├── www/index.html                    # poori app (UI + SQLite code)
├── capacitor.config.json             # app id / name / plugins
├── package.json                      # Capacitor aur plugins
├── .gitignore
└── README.md
```

## APK GitHub par kaise banayein

1. Is project ki saari files apni GitHub repository me upload karein (branch `main`). `index.html` `www` folder me aur `build-apk.yml` `.github/workflows` folder me honi chahiye.
2. Repository me **Actions** tab kholein. Push hote hi **Build APK** khud chal jata hai (3-6 minute). Dobara chalane ke liye **Run workflow** dabayein.
3. Run complete hone par us run ko kholein aur neeche **Artifacts** se `shop-details-apk` download karein.
4. Zip khol kar `app-debug.apk` phone me install karein (Unknown sources allow karna hoga).

## Permissions

| Permission | Kis liye |
|---|---|
| Location | Latitude / Longitude aur map se shop ka data |
| Camera | Shop ki live photo |
| Internet | Map se Shop Name / Address lena |

Pehli dafa app kholne par phone permission poochega, Allow karein.

## Database (SQLite)

Database ka naam `shopdb` hai. Tables:

- `shops`: date, depo, dr, beat, shop, addr, mob, cnic, lat, lng, photo, rem
- `setup`: Depo, Dr Name (depo ke sath) aur Beat ki lists
- `meta`: pehli dafa Beat A, B, C banane ka nishan

## Zaroori baatein

- Yeh **debug APK** hai, seedha install ho jati hai. Play Store ke liye signed release build alag se banani paregi.
- Data sirf usi phone me rehta hai. App uninstall karne ya app data clear karne par records khatam ho jate hain. Time time par Report se Excel export kar lein.
- Map se Shop Name tabhi milta hai jab dukan OpenStreetMap par mojood ho. Na mile to sirf Address bharta hai, name khud likhna hoga.
- Browser me `www/index.html` kholne par app localStorage use karti hai (SQLite aur camera sirf APK me chalte hain).

## Local build (optional)

```bash
npm install
cp node_modules/xlsx/dist/xlsx.full.min.js www/xlsx.full.min.js
npx cap add android
npx cap sync android
cd android && ./gradlew assembleDebug
```

APK yahan milegi: `android/app/build/outputs/apk/debug/app-debug.apk`

Location aur Camera permissions `android/app/src/main/AndroidManifest.xml` me add karni hongi (workflow file me dekh lein ke kaun si lines add hoti hain).
