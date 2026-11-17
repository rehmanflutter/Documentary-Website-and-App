# Documentary — Daily Feature Log

Is file mein Documentary Website and App (story-video-maker) ki rozana nayi features ka record hai.

## 2026-11-14 — Ek click mein poori video tayyar (Poori Video Tayyar Karo)

**Kya karti hai:** Project page ke progress card mein ek bara button "Poori Video Tayyar Karo" hai. Ek click par app pehle project ki target length tak baaqi saare scenes khud plan karta hai (narration + image prompts), phir har us scene ki tasveer auto-generate karta hai jis ki tasveer abhi nahi bani. Kaam ke dauraan live progress dikhta hai (misal: "Scene 4/4 tayyar ho raha hai — tasveer 3/3 ban rahi hai…") aur button disabled rehta hai. Sab mukammal hone par "Video tayyar hai!" ka message aata hai aur player Scene 1 se poori video chalane ke liye tayyar ho jata hai.

**Code summary:** Naya server action jo planMoreScenes aur generateSceneImage ko ek loop mein chalata hai — pehle target length tak scenes plan karta hai, phir har scene ki image banata hai. Pehle se tayyar scenes aur tasveerein dobara nahi banti; jin scenes ke liye user ne "Tasveer na banao" kaha hai, woh skip rehte hain; koi scene ya tasveer fail ho jaye to usay note karke baaqi kaam jaari rehta hai aur end mein summary mein bataya jata hai. UI mein progress card par button, live status text, disabled state aur completion message add hua.

## 2026-11-13 — YouTube Publish Kit

**Kya karti hai:** Har project ke liye YouTube publish kit banata hai — click-worthy title, description aur hashtags — jo project ke title, topic aur opening narration se likha jata hai. Kit ko edit karke save bhi kiya ja sakta hai.

**Code summary:** Naye server actions generatepublishkit aur updatepublishkit; project par publishTitle, publishDescription aur publishTags fields save hote hain.

## 2026-11-15 — Naye Video Styles — Kainaat (Space), Khel (Sports), Bachon ki Seekh / General Knowledge

**Date:** 15 November 2026

**Feature:** Documentary Website and App mein 3 naye video types/styles add hue — (1) Documentary — Kainaat (Space): khala, sitare, sayyaron ki documentary, hairat-angez narration, cinematic realistic space photography style tasveerein; (2) Documentary — Khel (Sports): khel aur khiladiyon ki documentary, joshili narration, dynamic sports photography style tasveerein; (3) Documentary — Bachon ki Seekh / General Knowledge: bachon ke liye simple asaan narration, bright friendly educational illustration style tasveerein.

**Kya karti hai:** video banane wale form ke style picker mein teenon naye options "Documentary" group ke neeche dikhte hain; har style ka apna planner tone aur apna imageStyle hai, is liye scenes ki kahani aur tasveerein usi style ke mutabiq banti hain; purane styles aur pehle se bane projects bilkul waise hi kaam karte hain.

**Code summary:** client ke style picker (STYLES list) mein 3 naye options — space, sports, kids_gk; server ke STYLES record mein har naye style ke liye apna tone aur imageStyle (space: cinematic realistic space photography; sports: dynamic sports photography; kids_gk: bright friendly educational illustration); createProject ke style enum mein teenon naye styles shamil; koi schema/migration change nahi, style text column hai.
## 2026-11-16 — Scene Reorder (Upar/Neeche)
- Feature: Har scene ke saath "Upar" (↑) aur "Neeche" (↓) buttons — scene list mein bhi aur editor timeline ke har clip ke neeche bhi. Scene pichle/aglay scene se jagah badalta hai aur nayi tartib server par save hoti hai, is liye player, timeline aur .webm/.srt export sab nayi tartib mein chalte hain.
- Pehle scene ka "Upar" aur aakhri scene ka "Neeche" button disabled hai; tasveer aur narration scene ke saath move hote hain; "Na karo" (skip) wali scene bhi reorder ho sakti hai.
- Code summary: Naya server action `moveScene` — scene ki position ko padosi scene se swap karta hai (position column update, scenes hamesha position ke hisaab se sort hoti hain). Client mein Studio ke scene-list action row aur editor timeline clip strip dono mein Upar/Neeche buttons; pehle/aakhri scene par disabled. Purani features (auto tasveerein, gapless voice, YouTube Publish Kit, ek-click poori video) untouched.
- Verify hua: sandbox test project (4 scenes) par move down/up karke tartib badli aur dobara kholne par wahi tartib mili; pehle scene par "up" safe no-op raha.

## 2026-11-17 — Project Duplicate (Copy Banao)

- Feature: Meri videos list mein har project card ke neeche "Duplicate (Copy banao)" button — ek click par us project ki poori copy ban jati hai: title wahi + " (Copy)", saari scenes (narration, image prompts aur bani hui tasveerein) aur YouTube Publish Kit bhi copy hota hai. Copy list mein sab se upar aati hai aur bilkul alag project ki tarah khulti hai — copy ko edit karne se original par koi farq nahi padta, dobara prompt likhne ki zaroorat nahi.
- Code summary: Naya server action `duplicateProject` — project ki copy banata hai (nayi id), uski saari scenes nayi ids ke saath copy karta hai (narration, image prompt, image URL aur "tasveer na banao" choice samet) aur publish kit fields bhi copy karta hai. Client mein "Meri videos" list ke har project card par Duplicate button; copy banne par list refresh hoti hai aur copy sab se upar dikhti hai. Purani features untouched.
- Verify hua: sandbox test project (4 scenes, 4 tasveerein) duplicate kiya — copy mein 4/4 scenes aur 4/4 tasveerein aayeen; copy ki narration edit karne par original unchanged raha; test copy delete kar di.
