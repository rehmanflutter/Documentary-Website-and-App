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

## 2026-11-18 — Thumbnail / Poster Image (Title-Card Poster)

- Feature: Har project ke liye "Poster banao" button — project ke title, topic aur style bible se asal cinematic 16:9 poster image generate hoti hai (tasveer mein koi text nahi — title app khud overlay karta hai). Poster project ke saath save hoti hai aur "Meri videos" list ke project card par thumbnail ban kar dikhti hai. Player mein video shuru hone se pehle poster + title wala title card aata hai, aur .webm export ke opening card mein bhi wahi poster lagti hai. "Dobara banao" se nayi poster banti hai aur "Download karo" se poster file download hoti hai — YouTube thumbnail ke tor par use kar sakte hain. Project duplicate karne par poster bhi copy ke saath copy hoti hai.
- Code summary: Naya server action generatePoster — project ke title/topic/style bible se poster image generate karke project par posterUrl (poster blob key) ke tor par save karta hai; migration 0004 poster_blob_key applied. Client mein Studio mein "Poster banao" button (progress ke saath), project card par poster thumbnail, player mein poster + title wala opening title card, .webm export mein poster wala opening card, "Dobara banao" aur "Download karo" buttons; duplicateProject poster ko bhi copy karta hai. Purani features untouched.
- Verify hua: sandbox test project par generatePoster call ki — asal generated image ka blob URL mila; dobara kholne par posterUrl wahi raha (persistence confirm); duplicate ki copy mein bhi wahi posterUrl aaya; test projects delete kar diye.

---
## 2026-11-19 — Subtitle Size aur Rang (Subtitle Style)

- Feature: Studio player mein "Subtitle style" setting — Size: Chhota / Darmiyana / Bara; Rang: Safed, Peela, Hara, Gulabi, Neela. Choice stage par foran live lagoo hoti hai — Play ke dauraan subtitle naye size/rang mein dikhta hai. Choice project ke saath server par save hoti hai, dobara kholne par wahi style milta hai; naye project ka default Safed + Darmiyana hai. Export mein bhi wahi style jata hai: .webm video mein subtitles usi size/rang mein banti hain, aur .srt file mein rang ka <font color> tag shamil hota hai. Project duplicate karne par subtitle style bhi copy hoti hai.
- Code summary: Projects par naye fields subtitleSize aur subtitleColor (nayi migration) + naya server action updateSubtitleStyle — choice server par save hoti hai aur getProject/listProjects wahi style return karte hain. Client mein Studio ke player controls mein Subtitle style row — 3 size pills (Chhota/Darmiyana/Bara) aur 5 rang pills (Safed/Peela/Hara/Gulabi/Neela); stage ka live subtitle aur .webm export canvas wahi size/rang use karte hain; .srt export mein <font color> tag aata hai; duplicateProject subtitle style bhi copy karta hai. Purani features untouched.
- Verify hua: sandbox test par updateSubtitleStyle(large, yellow) save hua aur getProject ne large/yellow return kiya (persistence confirm); Studio mein Bara + Peela click karne par stage subtitle foran bara aur peela dikha; duplicate ki copy mein style saath aayi; build/audit pass, no console errors.


---

## 2026-11-20 — Video Search (Meri Videos Dhoondhna)

- Feature: "Meri videos" list ke upar ek search box — title ya prompt/topic likhte hi list foran filter ho jati hai (case-insensitive); jo videos match nahi karti woh chhup jati hain. Search ke dauraan count dikhta hai ("N videos mili — kul M mein se"); koi match na mile to "Koi video nahi mili" ka saaf message aur "Search saaf karein" button. Input ke andar ✕ button se search foran saaf hoti hai. Har video card par us ka Topic (prompt) bhi dikhta hai taake prompt se mile hue matches list mein hi nazar aayein.
- Code summary: Client-side filter — App.tsx mein search state; listProjects se aaye title + prompt par case-insensitive match; list filteredProjects render karti hai; count line, no-results empty state aur dono clear buttons wired hain. Koi server/schema change nahi — listProjects pehle se title aur prompt return karta hai. Purani features untouched.
- Verify hua: build/audit pass; search state, filter, count, empty state aur clear buttons App.tsx mein wired; list filteredProjects render karti hai aur cards par Topic line dikhti hai.


---

## 2026-11-21 — Background Music (Pas-e-Manzar Music)

- Feature: Studio player mein "Background Music" section — mood chuno: Koi music nahi, Sukoon (calm), Josh (epic), Khushi (happy), Suspense — aur volume slider (default 25%, halki awaz jo narration ke neeche dabi rahe). Music koi file nahi: browser mein Web Audio API se procedurally banti hai (har mood ka apna chord progression + bass + halki melody loop), bilkul royalty-free — koi copyright masla nahi. Video Play karne par music saath loop mein bajti hai, Pause/Stop ya video khatam hone par ruk jati hai, aur mood/volume badalna chalti music par foran lagoo hota hai. Pasand har project ke saath server par save hoti hai — video dobara kholo to wahi mood/volume milta hai, aur Duplicate (Copy banao) par copy mein bhi wahi music setting jati hai. Download wali .webm video mein bhi wahi music usi volume par record hoti hai.

- Code summary: Naya server action updateMusicSetting + projects par naye fields musicMood aur musicVolume (migration 0006; purane projects ko default mood "none" / volume 25 mila). Client mein MusicEngine — Web Audio API se procedural ambient loops (har mood: apne chords, bass aur halki melody; gain node se volume, narration ke neeche halka); Studio player mein Background Music section (5 mood pills + volume slider, mood band par slider disabled); play/pause/stop aur unmount par engine start/stop; .webm export mein MusicEngine ka MediaStreamDestination canvas video track ke saath mila kar MediaRecorder se record hota hai (opus); mood band ho to purana sirf-video export chalta hai; duplicateProject music setting bhi copy karta hai. Purani features untouched.

- Verify hua: sandbox par migration 0006 apply hui; updateMusicSetting ne calm/40 save kiya aur dobara kholne par wahi mila; duplicate ki copy mein musicMood "calm" / musicVolume 40 aya; live probe mein Studio mein paanchon moods + slider render hue aur reload ke baad Sukoon selected mila; build/audit pass, no console errors. Note: .webm export ki music-mixing wiring source + build mein confirm hai; downloaded file ki awaz ka live test probe mein mumkin nahi tha (poora real-time export chalana parta) — player mein music live verify hui hai.


---

## 2026-11-22 — Apni Scene Add Karo (Add Your Own Scene)

- Feature: Studio/editor mein "Apni scene add karein" section — user apni khud ki narration line likh sakta hai aur jagah chun sakta hai: video ke aakhir mein ya kisi maujooda scene ke baad. Add karte hi app us line ke liye matching image prompt banata hai aur uski tasveer auto-generate karta hai; timeline aur scene list mein "Ban rahi hai…" dikhta hai, aur fail ho to "Tasveer banao" se dobara ban sakti hai. Nayi scene bilkul aam scene ki tarah kaam karti hai: gapless narration queue mein boli jati hai, stage par tasveer + subtitle dikhte hain, Upar/Neeche reorder aur delete chalte hain, aur .webm/.srt export mein shaamil hoti hai. Ek se zyada apni scenes add ho sakti hain, aur Project Duplicate par apni scenes bhi copy mein jati hain.

- Code summary: Naya server action addCustomScene — user ki narration line se project ki style bible aur picture-match rule ke mutabiq matching English image prompt banata hai, scene ko sahi position par insert karta hai (baad wali scenes ki positions shift karke). Client mein Studio mein "Apni scene add karein" section (Publish Kit ke baad): narration textarea, jagah ka select (end ya kisi scene ke baad), khaali line par error; add par stage nayi scene par jata hai aur makeImage se tasveer banti hai; auto-editor loop se race rokne ke liye nayi scene autoTried mein daal di jati hai. Koi schema/migration change nahi. Purani features untouched.

- Verify hua: sandbox test project (4 scenes) par addCustomScene se ek scene Scene 2 ke baad position 3 par insert hui, purani scenes 3→4, 4→5 shift huin, aur image prompt narration se match karta tha; dusri custom scene end par position 6 par judi; generateSceneImage custom scene par chala (image URL mila); moveScene down custom scene par chala; duplicateProject ne copy mein 6 scenes (custom scenes aur bani hui tasveer samet) copy kiye; build/audit pass.

---

## 2026-11-23: Tasveer Dobara Banao (Regenerate Scene Image)

- Studio/editor mein har us scene ke liye jiski tasveer pehle se bani hui hai, "Tasveer dobara banao" button — scene list aur editor timeline dono jagah.
- Dabane par server wahi narration aur project ki style bible se nayi variation banata hai: har dafa 8 mukhtalif camera angles mein se ek random angle/composition ke saath, taake nayi tasveer purani se waazeh tor par mukhtalif ho.
- Banate waqt "Nayi tasveer ban rahi hai…" dikhta hai aur purani tasveer tab tak dikhti rehti hai jab tak nayi na aa jaye; fail ho to purani tasveer wahi rehti hai aur dobara koshish ho sakti hai.
- Nayi tasveer server par purani ki jagah save hoti hai — reload ke baad bhi player, timeline, .webm export aur project duplicate mein nayi tasveer aati hai. Jinki tasveer abhi bani nahi, unke liye "Tasveer banao" button waisa hi hai.
- Code summary: `generatesceneimage` action mein `force` option — nayi variation ke liye 8 angles mein se random angle seed; client mein alag `regenBusy` state jo purani imageUrl ko refetch tak barqarar rakhti hai; scene list, timeline aur stage par regenerate button aur status.

## 24 November 2026 — Scene ki Narration Badlo (Edit Scene Narration)
- Studio/editor mein har scene ke saath "Narration badlo" button (scene list aur editor timeline dono jagah); timeline se dabane par editor usi scene par khul kar scroll ho jata hai.
- Dabane par us scene ki narration line editable textarea mein khulti hai — Save/Cancel; khaali line save nahi hoti.
- Save karne par nayi line server par save hoti hai (reload ke baad bhi rehti hai) aur player, stage, subtitles, .srt aur .webm export — sab mein nayi line aati hai; scene ki jagah/order aur baaki scenes bilkul waisi hi rehti hain.
- Jis scene ki tasveer pehle se bani hui hai, save ke baad poochha jata hai "Tasveer bhi nayi line ke mutabiq dobara banao?" — Haan par tasveer nayi line ke mutabiq dobara banti hai, Na par purani tasveer rehti hai; bina-tasveer scene par ye sawal nahi aata.
- Code: server par maujood updateSceneNarration action ko UI se jora gaya (scene list + editor timeline); scene list mein pehle se maujood basic "Line edit karein" editor ko poori feature mein badla gaya; regen prompt sirf tab aata hai jab line badli ho aur scene ki tasveer maujood ho, aur "Haan" maujooda generateSceneImage (force=true) se tasveer dobara banata hai.

## 25 November 2026 — Video ka Naam Badlo (Rename Project)
- "Meri videos" list ke har card mein "Naam badlo" button aur Studio mein title ke saath waisa hi editor — title inline edit hota hai (Save/Cancel; khaali naam save nahi hota).
- Naya naam server par save hota hai: reload ke baad bhi list, Studio, player/title card aur search mein naya naam dikhta hai; project duplicate karne par copy ko naya naam milta hai.
- Sirf title badalta hai — scenes, tasveerein, narration, poster, background music, subtitle style aur YouTube publish kit bilkul waisi hi rehti hain.
- Code summary: naya server action `renameProject` (title trim + min 1 validation, sirf title update karta hai); client mein list card aur Studio dono jagah inline rename editor; save par project detail refetch aur projects list invalidate hoti hai taake har jagah naya naam dikhe.




## 2026-11-26 — Video ki Script Download karein
- Studio mein YouTube Publish Kit ke neeche naya "Script" section: project ka title aur topic upar, phir har scene ki narration line "Scene 1, Scene 2, ..." number ke saath, scenes ke maujooda order mein.
- "Script download karein (.txt)" button: dabane par asal .txt file download hoti hai jisme title, topic aur saari numbered narration lines hoti hain.
- Script koi alag stored copy nahi — Studio ke live scenes data se banti hai, is liye narration badalne, scene add/delete ya reorder karne ke baad script aur download dono mein wahi nayi haalat dikhti hai.
- Code summary: client-side hi hai — App.tsx mein naya ScriptSection component (PublishKit ke baad render hota hai, getProject detail query ke live scenes istemal karta hai); download pehle se tested downloadText/downloadBlob helper (Blob + anchor click) se hoti hai, wahi jo Publish Kit ki .txt download istemal karti hai. Koi naya server action, schema ya migration nahi; baaki features untouched.




## 2026-11-27 — Narration Speed Control (0.75x / 1x / 1.25x)
- Player mein "Awaz ki speed" dropdown mein teen standard presets add hue: 0.75x (Aahista), 1x (Normal), 1.25x (Tez).
- Maujooda options (0.9 Slow, 0.98 Normal, 1.08 Tez) barkarar hain; dropdown mein ab 6 options ascending order mein hain.
- Chuni hui speed har narration utterance par lagu hoti hai (SpeechSynthesisUtterance.rate) aur localStorage "svm-voice-rate" mein save rehti hai; reload validation range 0.75–1.25 tak badha di gayi taake naye presets video dobara kholne par bane rahein.
- Code summary: client-side change in client/src/App.tsx only — voice-rate select options extended, chooseRate persistence unchanged, no server action, schema ya migration nahi; baaki features untouched.
  


## 2026-11-28 — Scene ki Copy Banao (Duplicate Scene)

- Feature: Studio/editor mein har scene ke saath "Copy banao" button (scene list aur editor timeline dono jagah). Dabate hi us scene ki poori copy usi ke foran baad lag jati hai — wohi narration, wohi image prompt aur wohi bani hui tasveer. Baaki scenes ki tartib waisi hi rehti hai, aur stage nayi copy par chala jata hai.
- Copy bilkul alag scene hai: uski narration ya tasveer badalne se original par koi farq nahi padta; copy par Upar/Neeche reorder, narration badlo, tasveer dobara banao aur delete bhi aam scene ki tarah chalte hain.
- Player, timeline aur .webm/.srt export sab copy ko uski jagah par dikhate/chalate hain, kyunke sab scenes ko position ke hisaab se padhte hain.
- Code summary: Naya server action `duplicateScene` — scene ki copy nayi id ke saath banata hai (narration, image prompt, image blob URL aur "tasveer na banao" choice samet), copy ko original ke foran baad insert karta hai aur baad wali scenes ki positions shift karta hai. Client mein scene list aur editor timeline dono mein "Copy banao" button; copy banne par stage nayi copy par jata hai. Koi schema/migration change nahi. Purani features untouched.
- Verify hua: sandbox test mein scene (position 2, tasveer samet) duplicate ki — copy position 3 par wohi narration/image prompt/image ke saath aayi, baad wali scenes 3→4, 4→5 shift huin; copy ki narration badalne par original unchanged raha; build/audit pass.
