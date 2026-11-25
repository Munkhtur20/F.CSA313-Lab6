# F.CSA313 — Лабораторийн ажил №6

## Category-Partition and PICT

### 1. Ажлын зорилго

Энэхүү лабораторийн ажлын зорилго нь Category-Partition аргаар тестийн нөхцөлүүдийг тодорхойлох, PICT хэрэгслээр Pairwise болон 3-way combinatorial testing хийх, тестийн загвар болон үр дүнг харьцуулах юм.

### 2. Ашигласан хэрэгсэл

- macOS — MacBook Air M2
- Git — хувилбарын удирдлага
- CMake — PICT build хийх
- PICT — combinatorial test generation
- Markdown — тайлан, үр дүнгийн баримтжуулалт

PICT-ийн source кодыг лабораторийн repository-оос гадуур `~/tools/pict` хавтсанд байрлуулж build хийсэн. Build-ийн мэдээллийг `results/pict-build.txt` файлд хадгалсан.

### 3. A хэсэг — Category-Partition

POST `/transfer` хүсэлтийн From, To, Amount, Currency параметрүүдийг ангилж, боломжит утгуудыг тодорхойлсон.

- From: Valid, Frozen, Missing
- To: Valid, Missing, SameAsFrom
- Amount: Normal, Zero, OverBalance, OverLimit
- Currency: Same, Different

Далд орчны хүчин зүйлсэд дансны үлдэгдэл, өдрийн лимитийн үлдэгдэл, валютын ханшийг авч үзсэн. Эдгээрийг үндсэн параметрээс тусад нь тестийн өгөгдлөөр тохируулсан.

Анхны бүрэн комбинацын тоо 72 байсан. IF constraint-үүдийг хэрэглэсний дараа боломжит комбинац 42 болж буурсан. Өөрөөр хэлбэл 72 → 42 комбинац болсон.

Алдааны дарааллыг системийн шаардлагад тодорхой заагаагүй тул тестийн загварт тусгай таамаглал болгон тогтоосон. Category-Partition-ийн дэлгэрэнгүй мэдээлэл болон 12 тестийн кейсийг `results/category-partition.md` файлд хадгалсан.

### 4. B1 — Browser Pairwise testing

Browser загварт Content, Cookies, Popups, Addons, Attacks, Forgeries гэсэн 6 параметр ашигласан.

Бүрэн комбинацын тоо:
3 × 3 × 2 × 2 × 2 × 2 = 144

PICT-ийн үр дүн:
- 2-way: 10 мөр
- 3-way: 20 мөр

2-way тест нь параметрүүдийн бүх хос утгын хамралтыг хангана. 3-way тест нь гурван параметрийн утгуудын бүх боломжит хослолын хамралтыг хангана.

### 5. B2 — Transfer PICT models

Гурван өөр загвар үүсгэж харьцуулсан.

| Model | Description | PICT rows |
|---|---|---:|
| transfer-nc.model | Constraint байхгүй | 13 |
| transfer.model | IF constraint ашигласан | 12 |
| transfer-neg.model | Negative values ашигласан | 16 |

IF constraint нь From эсвэл To утга Missing үед Amount-ийг Normal байхаар хязгаарласан.

Negative model-д `~` тэмдэглэгээгээр алдаатай утгуудыг тодорхойлсон. Жишээлбэл, `~Missing`, `~Frozen`, `~Zero`, `~OverBalance`, `~OverLimit`.

### 6. B3 — Test case generation

PICT-ийн үүсгэсэн мөрүүдээс бодит тестийн кейсүүд гаргасан.

- TC-01: Happy path
- TC-02: Insufficient balance
- TC-03: Missing source account
- TC-04: Frozen source account

Тест бүрд Setup, Input, Expected result тодорхойлсон. Дэлгэрэнгүйг `results/transfer-test-cases.md` файлаас харж болно.

### 7. Дүгнэлт

Энэ лабораторийн ажлаар Category-Partition болон PICT хэрэгслийг ашиглан тестийн нөхцөлүүдийг системтэй тодорхойлж сурсан. Browser загварын 2-way тест 10 мөр, 3-way тест 20 мөр үүсгэсэн. PICT-ийн greedy алгоритм нь тухайн хамралтыг хангахын тулд сонголтуудыг үе шаттай хийдэг боловч заавал хамгийн цөөн мөртэй шийдэл олно гэсэн баталгаа байхгүй. Иймээс 2-way үр дүн 10 мөр гарсан нь гараар байгуулсан 9 мөрөөс нэгээр олон байж болно. 3-way тест нь хоёр параметрийн хамралтаас илүү нарийн, гурван параметрийн утгуудын хамралтыг шалгадаг. Transfer загварт IF constraint ашигласнаар боломжит комбинац 72-оос 42 болж буурсан. Negative testing ашиглан хэвийн болон алдаатай утгуудыг ялгаж тестийн кейс үүсгэсэн. Эцэст нь тестийн загвар, гаралтын үр дүн, бодит тестийн кейсүүдийг Git repository-д хадгалсан.

