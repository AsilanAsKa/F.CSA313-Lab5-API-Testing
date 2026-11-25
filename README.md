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

Өөрийн API-д зориулан 10 үндсэн тестийн тохиолдол боловсруулсан. Нийтийн API-ийн тестийг тусдаа collection болгон салгасан.

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
| 09 | Double Error: Missing Student and Course | Оюутан болон хичээл хоёулаа байхгүй | 200 | `ERROR_NO_STUDENT` |
| 10 | Double Error: Inactive Student and Missing Prerequisite | Оюутан идэвхгүй бөгөөд prerequisite дутуу | 200 | `ERROR_INACTIVE_STUDENT` |

Амжилттай бүртгэлийн үед хариу JSON дотор `registrationID` талбар байгаа эсэх болон тоон утгатай эсэхийг шалгасан. Тогтмол ID утга тулгаагүй.

Давхар алдааны тестүүдээр API олон алдаа зэрэг үүсэх үед аль алдааг түрүүлж шалгаж байгааг баталгаажуулсан.

## 7. Postman Collection

Өөрийн API-ийн тестүүдийг `lab05-collection.json` файлд хадгалсан.

Үндсэн collection нь нийт 10 Folder, 20 хүсэлт, 38 assertion-тай. Үүнд амжилттай бүртгэл, оюутан байхгүй, идэвхгүй оюутан, хичээл байхгүй, prerequisite дутуу, шаардлагатай талбар дутуу, буруу JSON, хязгаарын нөхцөл болон хоёр давхар алдааны тестүүд багтсан.

Тест бүрт шаардлагатай оюутан болон хичээлийн мэдээллийг тухайн Folder дотор setup хүсэлтээр тохируулсан. Ингэснээр тестүүд өмнөх тестийн үүсгэсэн өгөгдлөөс хамаарахгүй ажиллах боломжтой болсон.

Мөн зориудаар буруу oracle ашигласан `lab05-collection-fail.json` файлыг үндсэн collection-оос хуулбарлан бэлтгэсэн. Энэ collection-д зөвхөн амжилттай бүртгэлийн HTTP 201 хариуг буруу 200 гэж шалгахаар өөрчилсөн. Бусад хүсэлт болон шалгалтууд үндсэн collection-тэй ижил.

JSONPlaceholder нийтийн API-ийн тестийг интернет холболтоос хамааралтай тул үндсэн collection-оос тусад нь `lab05-public-api.json` файлд хадгалсан.

## 8. Newman ашиглан тест ажиллуулах

### 8.1 PASS test

API серверийг ажиллуулсны дараа үндсэн collection-ийг ажиллуулсан.

Ажиллуулсан команд:

```bash
newman run lab05-collection.json 2>&1 | tee results/newman-pass.txt
```

Үр дүн:

| Үзүүлэлт | Утга |
|---|---:|
| Iterations | 1 |
| Requests | 20 |
| Failed requests | 0 |
| Test scripts | 20 |
| Failed test scripts | 0 |
| Assertions | 38 |
| Failed assertions | 0 |
| Exit code | 0 |

PASS тестийн бүх хүсэлт болон 38 assertion амжилттай ажилласан. Newman амжилттай дууссаныг exit code 0 баталсан.

### 8.2 FAIL test

FAIL collection дээр амжилттай бүртгэлийн HTTP status oracle-ийг зориудаар 201-ийн оронд 200 гэж өөрчилсөн.

Ажиллуулсан команд:

```bash
newman run lab05-collection-fail.json 2>&1 | tee results/newman-fail.txt
```

Үр дүн:

| Үзүүлэлт | Утга |
|---|---:|
| Requests | 20 |
| Failed requests | 0 |
| Test scripts | 20 |
| Assertions | 38 |
| Failed assertions | 1 |
| Exit code | 1 |

Алдааны мэдээлэл:

```text
Status code is 201
expected response status 200 but got 201
```

Энэ туршилтаар API зөв хариу буюу HTTP 201 буцаасан боловч зориудаар буруу oracle ашигласнаас нэг assertion амжилтгүй болсон. Бусад шалгалтууд амжилттай ажилласан.

### 8.3 DOWN test

DOWN тест хийхийн өмнө API серверийг ажиллаж байсан Terminal дээр `Ctrl+C` ашиглан зогсоосон.

Ажиллуулсан команд:

```bash
newman run lab05-collection.json 2>&1 | tee results/newman-down.txt
```

Сервер унтарсан үед дараах холболтын алдаа гарсан:

```text
connect ECONNREFUSED 127.0.0.1:3000
```

Үр дүн:

| Үзүүлэлт | Утга |
|---|---:|
| Requests | 20 |
| Failed requests | 20 |
| Test scripts | 20 |
| Failed test scripts | 1 |
| Assertions | 36 |
| Failed assertions | 36 |
| Exit code | 1 |

Локал сервер унтарсан тул Newman-ийн 20 хүсэлт бүгд холболтын алдаатай болсон. Нийт 36 assertion амжилтгүй болж, нэг test script JSONError үүсгэсэн. Энэ нь сервертэй холбогдож чадаагүйгээс үүдсэн DOWN нөхцөл бөгөөд зориудаар буруу oracle ашигласан FAIL нөхцөлөөс ялгаатай.

### 8.4 FAIL ба DOWN-ийн ялгаа

FAIL тест нь API хариу өгсөн боловч хүлээгдэж буй үр дүнг зориудаар буруу oracle-той харьцуулснаас assertion алдаа үүсгэдэг.

Харин DOWN тест нь сервер унтарсан тул хүсэлт API-д хүрч чадахгүй, `ECONNREFUSED` холболтын алдаа үүсгэдэг. Иймээс эдгээр нь өөр өөр төрлийн алдааг илрүүлдэг.

## 9. Нийтийн API тест

Нэмэлтээр JSONPlaceholder нийтийн API-г ашиглан дараах endpoint-ийг шалгасан:

`GET https://jsonplaceholder.typicode.com/users`

Энэ тестийг `lab05-public-api.json` collection-д тусад нь хадгалсан.

Дараах гурван шалгалтыг хийсэн:

1. HTTP status `200` байх.
2. Хариу өгөгдөл массив хэлбэртэй байх.
3. Эхний хэрэглэгчийн нэр `Leanne Graham` байх.

Ажиллуулсан команд:

```bash
newman run lab05-public-api.json 2>&1 | tee results/newman-public-api.txt
```

Үр дүн:

| Үзүүлэлт | Утга |
|---|---:|
| Requests | 1 |
| Failed requests | 0 |
| Test scripts | 1 |
| Assertions | 3 |
| Failed assertions | 0 |
| Exit code | 0 |

JSONPlaceholder нь зөвхөн GET хүсэлтээр мэдээлэл авдаг тул нэмэлт setup шаардлагагүй байсан. Харин өөрийн API дээр өгөгдөл үүсгэж, өөр өөр нөхцөлөөр турших шаардлагатай байсан.

## 10. Үр дүнгийн файлууд

Туршилтын үр дүнг `results/` хавтсанд хадгалсан.

```text
results/
├── newman-pass.txt
├── newman-fail.txt
├── newman-down.txt
└── newman-public-api.txt
```

Эдгээр файлд Newman-ийн туршилтын дэлгэрэнгүй гаралт болон үр дүнгийн мэдээлэл хадгалагдсан.

## 11. Repository бүтэц

```text
lab05/
├── .gitignore
├── README.md
├── server.js
├── lab05-collection.json
├── lab05-collection-fail.json
├── lab05-public-api.json
└── results/
    ├── newman-pass.txt
    ├── newman-fail.txt
    ├── newman-down.txt
    └── newman-public-api.txt
```

## 12. Дүгнэлт

Энэхүү лабораторийн ажлаар Postman болон Newman ашиглан REST API системийг тестэлж сурлаа. Өөрийн API-д зориулан 10 үндсэн тестийн тохиолдол боловсруулж, нийт 20 хүсэлт болон 38 assertion ашиглан шалгалт хийсэн. Эквивалент ангилал болон хязгаарын нөхцөлийн аргыг ашигласан. Тест боловсруулахад хамгийн их бодол шаардсан хэсэг нь тест бүрийн өгөгдлийг бие даан тохируулах болон давхар алдааны үед API аль алдааг түрүүлж шалгахыг тодорхойлох явдал байлаа. Оюутан болон хичээл байхгүй нөхцөлийг хамтад нь шалгахад API `ERROR_NO_STUDENT` үр дүнг буцаасан. Идэвхгүй оюутан болон prerequisite дутуу нөхцөлд `ERROR_INACTIVE_STUDENT` алдааг түрүүлж шалгасан. PASS туршилтаар 20 хүсэлт, 38 assertion амжилттай болж, FAIL туршилтаар зориудаар буруу oracle ашиглахад нэг assertion алдаа илэрсэн. DOWN туршилтаар сервер унтарсан үед 20 хүсэлт бүгд `ECONNREFUSED` алдаатай болж, Newman exit code 1 буцаасан. Мөн JSONPlaceholder нийтийн API-г тусдаа collection болгон шалгаж, гурван assertion амжилттай ажиллуулсан. Ингэснээр API-ийн хэвийн ажиллагаа, буруу оролт болон серверийн холболтын алдааг Postman, Newman ашиглан системтэйгээр шалгах практик мэдлэг эзэмшлээ.