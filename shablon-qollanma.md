# O'z uy rejangizni JSON orqali kiritish

[shablon-reja.json](shablon-reja.json) — tayyor namuna: yotoqxona, hammom va mehmonxonadan iborat 8000 × 6000 mm xonadon.
Uni nusxalab, raqamlarni o'z rejangizga moslang va ilovada **Fayl ▾ → Rejani import qilish** orqali yuklang.

## Koordinatalar

- Barcha o'lchamlar **millimetrda**.
- `x` chapdan o'ngga, `y` **yuqoridan pastga** o'sadi.
- (0, 0) nuqtani qulay joyga qo'ying, masalan, chap yuqori tashqi devorning ichki burchagiga. Manfiy qiymatlar ham bo'ladi.
- To'rtburchak har doim `[x0, y0, x1, y1]` ko'rinishida yoziladi: ikki qarama-qarshi burchak.

## `plan` bo'limlari

### `walls`: devorlar (majburiy)
`[x0, y0, x1, y1, "tur"]`. Har bir devor to'rtburchak bo'lib, uning qalinligi ham to'rtburchak ichida bo'ladi.

| tur | ma'nosi |
|---|---|
| `"b"` | yuk ko'taruvchi devor (buzib bo'lmaydi) |
| `"e"` | tashqi devor |
| `"n"` | oddiy ichki devor ("Buzish" rejimida olib tashlasa bo'ladi) |
| `"low"` | past devor (taxminan 1 m) |

Eshik va deraza o'rnida devor **bo'lmaydi**: devorni ikki bo'lakka bo'lib, oradagi bo'sh joyni qoldirasiz.

### `windows`: derazalar
`[x0, y0, x1, y1, tokcha_balandligi]`. Deraza devordagi bo'sh joyni to'liq to'ldiradi. Tokcha balandligi mm'da beriladi va ixtiyoriy (standart 900).

### `doors`: oddiy (ochiladigan) eshiklar
```json
{ "name": "Yotoqxona eshigi", "rect": [4000,2500,4120,3300], "h": [4000,3300], "c": [0,-1], "o": [-1,0], "len": 800 }
```
- `rect`: devordagi eshik o'rni;
- `h`: lo'kidon (petlya) nuqtasi, odatda `rect` burchaklaridan biri;
- `c`: eshik **yopiq** holatda lo'kidondan qaysi tomonga yo'naladi: `[1,0]` o'ngga, `[-1,0]` chapga, `[0,1]` pastga, `[0,-1]` yuqoriga;
- `o`: eshik **ochilganda** qaysi tomonga buriladi (xona ichiga qarab);
- `len`: eshik kengligi (mm);
- `"entry": true` faqat **kirish eshigiga** qo'yiladi: 2D'da "Kirish" strelkasi chiziladi va 3D "Sayr" rejimi shu eshik oldidan boshlanadi.

### `slides`: surma eshiklar
`{ "rect": [...], "v": true }`. `v: true` eshik vertikal devorda, `false` gorizontal devorda turganini bildiradi.

### `openings`: eshiksiz o'tish joylari (erker va boshqalar)
`{ "rect": [...], "h": 2400 }`. 3D'da faqat ustiga to'sin qo'yiladi; `h` o'tish joyining balandligi.

### `rooms`: xonalar (majburiy)
```json
{ "id": "bedroom", "name": "Yotoqxona", "rect": [0,0,4000,3500], "mat": "wood" }
```
- `id`: lotin harflarida, takrorlanmaydigan nom;
- `rect` (to'rtburchak xona) **yoki** `poly` (murakkab shakl: `[[x,y], [x,y], ...]` burchaklar ketma-ket);
- `mat`: pol qoplamasi: `wood` (eman parket), `walnut` (yong'oq parket), `tile800`, `tile600`, `marble`, `antislip`, `terrazzo`, `carpet`;
- `at`: nom yozuvining joyi `[x,y]`, ixtiyoriy (bo'lmasa xona markaziga qo'yiladi);
- `"counted": false` bo'lsa, xona (masalan, erker) umumiy maydonga qo'shilmaydi.

Xona chegarasi devorlarning **ichki** chizig'i bo'ylab o'tadi.

### `dims`: o'lcham chiziqlari (ixtiyoriy)
`{ "dir": "h", "at": -750, "start": 0, "segs": [1500, 1500, 2000] }`
- `dir`: `"h"` gorizontal, `"v"` vertikal;
- `at`: chiziq qayerda turadi (gorizontalda `y`, vertikalda `x`);
- `start`: boshlanish nuqtasi, `segs`: bo'laklar uzunligi.

Agar `dims` berilmasa, umumiy eni va bo'yi avtomatik chiziladi.

## `furniture`: mebel (ixtiyoriy)
`{ "type": "bed", "cx": 2000, "cy": 1150, "w": 1600, "d": 2000, "rot": 0 }`. `cx, cy` mebel markazi, `w` eni, `d` chuqurligi, `rot` burilish burchagi (0/90/180/270).
Mebelni import qilgandan keyin chapdagi kutubxonadan sudrab qo'yish osonroq, shuning uchun bu ro'yxatni bo'sh (`[]`) qoldirsangiz ham bo'ladi.

## Maslahatlar

- **Eng oson yo'l:** standart rejani **Fayl ▾ → Rejani JSON ga eksport qilish** orqali yuklab oling. Endi u ham `plan` bo'limini o'z ichiga oladi, shuning uchun to'liq ishlaydigan katta namuna sifatida foydalansa bo'ladi.
- Xato bo'lsa, ilova qaysi qatorda ekanini ko'rsatadi, masalan: `Rejada xato: rooms[2].poly`.
- Import qilingan reja brauzerda saqlanadi. Asl rejaga qaytish uchun **Fayl ▾ → Standart holatga qaytarish** tugmasini bosing.
