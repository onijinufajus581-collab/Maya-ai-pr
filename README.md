# MAYA AI — GitHub থেকে APK বানানোর Android Project

এই প্রজেক্টে MAYA AI-এর নিজস্ব নীল-নিয়ন অ্যানিমে-স্টাইল **launcher app icon** যুক্ত করা আছে। AndroidManifest-এ `android:icon` ও `android:roundIcon` সেট করা, তাই APK ইনস্টল হলে ফোনের অ্যাপ ড্রয়ার/হোম স্ক্রিনে MAYA AI আইকন স্বয়ংক্রিয়ভাবে দেখাবে। আলাদা করে হাতে আইকন বসাতে হবে না। এটি Apple-এর লোগো নয়; MAYA AI-এর নিজস্ব আইকন।

## GitHub দিয়ে APK বানানোর ধাপ

1. ZIP ফাইলটি Extract করো।
2. GitHub-এ লগইন করে **New repository** চাপো। নাম দাও `MAYA-AI`, Public বা Private যেকোনোটি।
3. Extract করা ফোল্ডারের ভেতরের সব ফাইল ও ফোল্ডার (যেমন `app`, `backend`, `.github`, `build.gradle`, `settings.gradle`) repository-এর root-এ Upload files দিয়ে আপলোড করো। ZIP ফাইলটিকে ZIP হিসেবেই repository-তে দেবে না।
4. **Commit changes** চাপো।
5. Repository-র **Actions** tab খোলো। প্রথমবার হলে **I understand my workflows, go ahead and enable them** চাপো।
6. বাম পাশ থেকে **Build MAYA AI APK** workflow বেছে নাও। **Run workflow → Run workflow** চাপো। (কোড `main` branch-এ push করলেও workflow চলবে।)
7. Workflow শেষ হলে সবুজ টিক চিহ্ন আসবে। ওই run-টি খোলো, নিচে **Artifacts** অংশে `MAYA-AI-debug-apk` ডাউনলোড করো।
8. ডাউনলোড ZIP Extract করো। ভেতরের `app-debug.apk` Android ফোনে খোলো।
9. ফোনে চাইলে **Install unknown apps / এই উৎস থেকে ইনস্টল করার অনুমতি** দাও, তারপর Install চাপো।
10. ইনস্টল শেষ হলে App Drawer-এ **MAYA AI** নাম ও নতুন MAYA আইকন দেখা যাবে। Android launcher-এর নিজস্ব round/squircle mask অনুযায়ী আইকনের বাইরের আকৃতি সামান্য বদলাতে পারে।

## আইকন কোথায় সেট করা আছে?

- `app/src/main/AndroidManifest.xml` — `android:icon` এবং `android:roundIcon`
- `app/src/main/res/mipmap-anydpi-v21/` — পুরোনো Android-এর launcher vector icon
- `app/src/main/res/mipmap-anydpi-v26/` — Android 8.0+ adaptive icon
- `app/src/main/res/values/colors.xml` — আইকনের background color

নিজের PNG আইকন বসাতে চাইলে একই resource নাম ও Android adaptive-icon structure বজায় রেখে resource ফাইল বদলাতে হবে।

## AI Backend (ঐচ্ছিক)

`backend/`-এ Node/Express proxy আছে। GitHub APK build নিজে থেকে API server চালায় না।

1. GitHub repository-তে **Settings → Secrets and variables → Actions → New repository secret** থেকে নতুন API key `OPENAI_API_KEY_1` নামে রাখো।
2. Render-এ repository থেকে Web Service তৈরি করো, Root Directory `backend` দাও।
3. Render-এর Environment-এ `OPENAI_API_KEY_1` এবং প্রয়োজনীয় `OPENAI_MODEL` সেট করো।
4. Deploy হলে `https://YOUR-SERVICE.onrender.com/health` খোলো।

**সতর্কতা:** আগে চ্যাটে বা কোথাও প্রকাশ করা API key নিরাপদ নয়—সেগুলো revoke করে নতুন key তৈরি করো। API key কখনো Android source code, APK বা public GitHub-এ লিখবে না। ChatGPT Free subscription আর API billing আলাদা। API ব্যবহারে খরচ হতে পারে।

## এই starter build-এর সীমাবদ্ধতা

এটি এখনো পূর্ণাঙ্গ production assistant নয়। UI, বাংলা speech recognition/TTS entry point এবং কিছু app-opening command আছে। ফোনের speech service/বাংলা voice availability ডিভাইসভেদে বদলায়। Android-এর Chat button এখনো backend-এর সঙ্গে সম্পূর্ণ wired নয়; দীর্ঘমেয়াদি memory, live animated/lip-sync avatar, সম্পূর্ণ WhatsApp message sending বা অন্য অ্যাপের ভেতরে নির্ভরযোগ্য automation নেই। কোনো message পাঠানো বা post প্রকাশ করার আগে ব্যবহারকারীর confirmation দরকার।

## লোকাল build (ঐচ্ছিক)

Android Studio-তে project folder খোলো, JDK 17 দিয়ে Gradle sync করো, তারপর **Build → Build Bundle(s) / APK(s) → Build APK(s)** চাপো। APK সাধারণত `app/build/outputs/apk/debug/app-debug.apk`-এ তৈরি হবে।
