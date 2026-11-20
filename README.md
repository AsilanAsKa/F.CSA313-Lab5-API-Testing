# Лаборатори №5 — API систем тест: Postman ба Newman

## 1. Оюутны мэдээлэл

- **Оюутны нэр:** С.Асилан
- **Оюутны код:** B232270115
- **Хичээл:** F.CSA313
- **Лабораторийн ажил:** №5
- **Сэдэв:** API систем тест — Postman ба Newman

## 2. Лабораторийн зорилго

Энэхүү лабораторийн ажлын зорилго нь REST API-ийн функциональ ажиллагааг Postman ашиглан тестлэх, хүлээгдэж буй үр дүнг assertion буюу oracle-аар шалгах, Newman ашиглан collection-ийг командын мөрөөс автомат ажиллуулахад оршино.

Туршилтаар амжилттай бүртгэл, байхгүй болон идэвхгүй оюутан, байхгүй хичээл, prerequisite дутуу байх, шаардлагатай талбар дутуу байх, буруу JSON, хязгаарын нөхцөл болон давхар алдааны тохиолдлуудыг шалгасан.

## 3. Ашигласан технологи

| Технологи | Хувилбар / утга |
|---|---|
| Node.js | v22.22.2 |
| Newman | 6.2.3 |
| API хаяг | `http://localhost:3000` |

Хувилбаруудыг дараах командаар шалгасан:

```bash
node -v
newman -v
```

## 4. API-ийн мэдээлэл

API серверийг дараах командаар ажиллуулна:

```bash
node server.js
```

Үндсэн endpoint-үүд:

| HTTP арга | Endpoint | Зориулалт |
|---|---|---|
| PUT | `/students/:id` | Оюутны мэдээлэл үүсгэх, шинэчлэх |
| PUT | `/courses/:id` | Хичээлийн мэдээлэл болон prerequisite тохируулах |
| GET | `/courses/:id` | Хичээлийн мэдээлэл авах |
| POST | `/registrations` | Оюутныг хичээлд бүртгэх |
| GET | `/registrations` | Бүртгэлийн мэдээлэл авах |

## 5. Choice / Value analysis

| Хувьсагч | Боломжит утгууд |
|---|---|
| Student status | active, inactive, байхгүй |
| Student coursesTaken | prerequisite хангасан, хангаагүй, хоосон |
| Course existence | байгаа, байхгүй |
| Course prerequisites | prerequisite байхгүй, нэг prerequisite, олон prerequisite |
| Request fields | хоёр талбар бүрэн, studentID дутуу, courseID дутуу |
| Request format | зөв JSON, буруу JSON |
| Алдааны нөхцөл | нэг алдаа, хоёр алдаа зэрэг |
| HTTP status | 200, 201, 400 |

## 6. Test specification

Нийт 11 үндсэн тестийн тохиолдол боловсруулсан.

| № | Тест | Нөхцөл | Хүлээгдэж буй status | Хүлээгдэж буй result |
|---|---|---|---:|---|
| 01 | Successful Registration | Идэвхтэй оюутан, байгаа хичээл, prerequisite хангасан | 201 | `OK` |
| 02 | Student Not Found | Оюутны ID бүртгэлгүй | 200 | `ERROR_NO_STUDENT` |
| 03 | Inactive Student | Оюутан байгаа боловч идэвхгүй | 200 | `ERROR_INACTIVE_STUDENT` |
| 04 | Course Not Found | Хичээлийн ID бүртгэлгүй | 200 | `ERROR_NO_COURSE` |
| 05 | Missing Prerequisite | Оюутан шаардлагатай CS201 хичээлийг үзээгүй | 200 | `ERROR_PREREQUISITES` |
| 06 | Missing Required Fields | studentID эсвэл courseID талбар дутуу | 400 | `ERROR_BAD_REQUEST` |
| 07 | Invalid JSON | Хүсэлтийн JSON формат алдаатай | 400 | `ERROR_BAD_JSON` |
| 08 | Boundary Test | Prerequisite шаардахгүй хичээлд бүртгүүлэх | 201 | `OK` |
| 09 | Public API | JSONPlaceholder хэрэглэгчдийн мэдээлэл авах GET хүсэлт | 200 | Хэрэглэгчдийн массив |
| 10 | Double Error: Missing Student and Course | Оюутан болон хичээл хоёулаа байхгүй | 200 | `ERROR_NO_STUDENT` |
| 11 | Double Error: Inactive Student and Missing Prerequisite | Оюутан идэвхгүй бөгөөд prerequisite дутуу | 200 | `ERROR_INACTIVE_STUDENT` |

Амжилттай бүртгэлийн үед хариу JSON дотор `registrationID` талбар байгаа эсэх болон тоон утгатай эсэхийг шалгасан. Тогтмол ID утга тулгаагүй.

Давхар алдааны тестүүдээр API олон алдаа зэрэг үүсэх үед аль алдааг түрүүлж шалгаж байгааг баталгаажуулсан.

## 7. Postman Collection

Тестүүдийг `lab05-collection.json` файлд хадгалсан.

Collection нь нийт 11 Folder, 20 хүсэлттэй. Үүнд:

- Амжилттай бүртгэл
- Оюутан байхгүй үеийн шалгалт
- Идэвхгүй оюутны шалгалт
- Хичээл байхгүй үеийн шалгалт
- Урьдчилсан нөхцөл хангаагүй үеийн шалгалт
- Шаардлагатай талбар дутуу үеийн шалгалт
- Буруу JSON формат
- Урьдчилсан нөхцөлгүй хичээлийн хязгаарын тест
- JSONPlaceholder нийтийн API тест
- Давхар алдааны хоёр тест

Тест бүрт шаардлагатай оюутан болон хичээлийн мэдээллийг тухайн Folder дотор setup хүсэлтээр тохируулсан. Ингэснээр тестүүд өмнөх тестийн үүсгэсэн өгөгдлөөс хамаарахгүй ажиллах боломжтой болсон.

Мөн зориудаар буруу oracle ашигласан `lab05-collection-fail.json` файлыг тусад нь бэлтгэсэн. Уг collection-д амжилттай бүртгэлийн HTTP 201 хариуг буруу 200 гэж шалгадаг.

## 8. Newman ашиглан тест ажиллуулах

### 8.1 PASS test

Ажиллуулсан команд:

```bash
newman run lab05-collection.json 2>&1 | tee results/newman-pass.txt
```

Үр дүн:

| Үзүүлэлт | Утга |
|---|---:|
| Iterations | 1 |
| Requests | 20 |
| Test scripts | 20 |
| Assertions | 39 |
| Failed assertions | 0 |
| Exit code | 0 |

PASS тестийн бүх assertion амжилттай ажилласан.

### 8.2 FAIL test

FAIL collection дээр амжилттай бүртгэлийн HTTP status oracle-ийг зориудаар 201-ийн оронд 200 гэж өөрчилсөн.

Ажиллуулсан команд:

```bash
newman run lab05-collection-fail.json 2>&1 | tee results/newman-fail.txt
```

Үр дүн:

| Үзүүлэлт | Утга |
|---|---:|
| Requests | 3 |
| Assertions | 4 |
| Failed assertions | 1 |
| Exit code | 1 |

Алдааны мэдээлэл:

```text
expected response to have status code 200 but got 201
```

Энэ нь API зөв хариу өгсөн боловч зориудаар буруу oracle ашигласнаас assertion амжилтгүй болсныг харуулсан.

### 8.3 DOWN test

DOWN тест хийхийн өмнө API серверийг `Ctrl+C` ашиглан зогсоосон.

Ажиллуулсан команд:

```bash
newman run lab05-collection.json 2>&1 | tee results/newman-down.txt
```

Сервер унтарсан үед дараах холболтын алдаа гарсан:

```text
connect ECONNREFUSED 127.0.0.1:3000
```

Локал сервер унтарсан тул Newman локал API-ийн хүсэлтүүдэд холбогдож чадаагүй. DOWN тестээр нийт 20 хүсэлт ажилласнаас локал API-ийн 19 хүсэлт ECONNREFUSED алдаатай болж, JSONPlaceholder-ийн 1 GET хүсэлт амжилттай болсон. Нийт 37 assertion-оос 34 нь амжилтгүй болж, Newman Exit code 1 буцаасан.

### 8.4 FAIL ба DOWN-ийн ялгаа

FAIL тест нь API хариу өгсөн боловч хүлээгдэж буй үр дүнг зориудаар буруу oracle-той харьцуулснаас assertion алдаа үүсгэдэг.

Харин DOWN тест нь сервер унтарсан тул хүсэлт API-д хүрч чадахгүй, `ECONNREFUSED` холболтын алдаа үүсгэдэг. Иймээс эдгээр нь өөр өөр төрлийн алдааг илрүүлдэг.

## 9. Нийтийн API тест

Нэмэлтээр JSONPlaceholder нийтийн API-г ашиглан дараах endpoint-ийг шалгасан:

`GET https://jsonplaceholder.typicode.com/users`

Дараах гурван шалгалтыг хийсэн:

1. HTTP status `200` байх.
2. Хариу өгөгдөл массив хэлбэртэй байх.
3. Эхний хэрэглэгчийн нэр `Leanne Graham` байх.

JSONPlaceholder нь зөвхөн GET хүсэлтээр мэдээлэл авдаг тул нэмэлт setup шаардлагагүй байсан. Харин өөрийн API дээр өгөгдөл үүсгэж, өөр өөр нөхцөлөөр турших шаардлагатай байсан.

## 10. Үр дүнгийн файлууд

Туршилтын үр дүнг `results/` хавтсанд хадгалсан.

```text
results/
├── newman-pass.txt
├── newman-fail.txt
└── newman-down.txt
```

## 11. Repository бүтэц

```text
lab05/
├── .gitignore
├── README.md
├── server.js
├── lab05-collection.json
├── lab05-collection-fail.json
└── results/
    ├── newman-pass.txt
    ├── newman-fail.txt
    └── newman-down.txt
```

## 12. Дүгнэлт

Энэхүү лабораторийн ажлаар Postman болон Newman ашиглан REST API системийг тестэлж сурлаа. Бүртгэлийн API-д зориулан зөв болон буруу оролтын нөхцөлүүдийг тодорхойлж, 11 бүлэг тест боловсруулсан. Эквивалент ангилал болон хязгаарын нөхцөлийн аргыг ашигласан. Тест боловсруулахад хамгийн их бодол шаардсан хэсэг нь тест бүрийн өгөгдлийг бие даан тохируулах болон давхар алдааны үед API аль алдааг түрүүлж шалгахыг тодорхойлох явдал байлаа. Оюутан болон хичээл байхгүй нөхцөлийг хамтад нь шалгахад API ERROR_NO_STUDENT үр дүнг буцаасан. Идэвхгүй оюутан болон prerequisite дутуу нөхцөлд ERROR_INACTIVE_STUDENT алдааг түрүүлж шалгасан. PASS туршилтаар 20 хүсэлт, 39 assertion амжилттай болж, FAIL туршилтаар зориудаар буруу oracle ашиглахад алдаа илэрсэн. DOWN туршилтаар локал сервер унтарсан үед 19 хүсэлт ECONNREFUSED алдаатай болж, гаднын JSONPlaceholder хүсэлт амжилттай ажилласан. JSONPlaceholder нь GET хүсэлтээр бэлэн өгөгдөл авдаг бол өөрийн API дээр өгөгдөл үүсгэж, өөр өөр нөхцөлөөр шалгах шаардлагатай байсан. Ингэснээр API-ийн хэвийн ажиллагаа болон алдааны нөхцөлүүдийг Postman, Newman ашиглан системтэйгээр шалгах практик мэдлэг эзэмшлээ.
