# Melolo API — Free Drama & Short Film API 🎬

Akses ribuan drama dan film pendek dari Melolo lewat REST API. Gratis, tanpa biaya.

**Website:** [cutad.web.id](https://www.cutad.web.id)
**Dokumentasi:** [cutad.web.id/docs](https://www.cutad.web.id/docs)
**Powered by:** [CUTAD API](https://github.com/rudiansyah1998/cutad-api-docs)

---

## 🚀 Mulai dalam 30 Detik

### 1. Dapatkan API Key (Gratis)

```bash
curl -X POST https://www.cutad.web.id/public/api/v1/key/generate \
  -H "Content-Type: application/json" \
  -d '{"name": "Nama Kamu", "email": "kamu@email.com"}'
```

Response:
```json
{
  "message": "API key generated successfully",
  "apiKey": "cutad_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "rateLimit": "30 requests/minute"
}
```

### 2. Gunakan API Key

```bash
# Header (recommended)
curl https://www.cutad.web.id/public/api/v1/melolo/rank \
  -H "X-API-Key: cutad_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Query parameter
curl "https://www.cutad.web.id/public/api/v1/melolo/rank?api_key=cutad_xxx"
```

---

## 📡 Endpoint

| Endpoint | Method | Deskripsi |
|----------|--------|-----------|
| `/public/api/v1/melolo/rank` | GET | Daftar drama/film populer |
| `/public/api/v1/melolo/detail?id={id}` | GET | Detail konten + episode |
| `/public/api/v1/melolo/watch?id={id}` | GET | URL video untuk diputar |
| `/public/api/v1/melolo/search?q={keyword}` | GET | Cari drama/film |

---

### 🏠 Rank / Home

Ambil daftar drama dan film populer dari Melolo.

```
GET /public/api/v1/melolo/rank
```

```bash
curl https://www.cutad.web.id/public/api/v1/melolo/rank \
  -H "X-API-Key: YOUR_KEY"
```

Response:
```json
{
  "data": {
    "sections": [
      {
        "name": "Melolo",
        "items": [
          {
            "id": "12345",
            "title": "Judul Drama",
            "cover": "https://...",
            "description": "Deskripsi singkat...",
            "totalEpisodes": 24,
            "type": "series"
          }
        ]
      }
    ]
  }
}
```

### 📄 Detail

Ambil detail konten termasuk daftar episode.

```
GET /public/api/v1/melolo/detail?id={content_id}
```

```bash
curl "https://www.cutad.web.id/public/api/v1/melolo/detail?id=12345" \
  -H "X-API-Key: YOUR_KEY"
```

Response:
```json
{
  "data": {
    "fakeId": "12345",
    "title": "Judul Drama",
    "coverImgUrl": "https://...",
    "introduce": "Deskripsi lengkap drama...",
    "episodes": [
      {
        "id": "ep_001",
        "videoFakeId": "ep_001",
        "title": "Episode 1",
        "episodeNumber": 1,
        "thumbnail": "https://...",
        "duration": "10:30"
      },
      {
        "id": "ep_002",
        "videoFakeId": "ep_002",
        "title": "Episode 2",
        "episodeNumber": 2,
        "thumbnail": "https://...",
        "duration": "11:15"
      }
    ],
    "totalOfEpisodes": 24
  }
}
```

### 🎥 Watch / Video URL

Ambil URL video untuk episode tertentu.

```
GET /public/api/v1/melolo/watch?id={video_id}
```

```bash
curl "https://www.cutad.web.id/public/api/v1/melolo/watch?id=ep_001" \
  -H "X-API-Key: YOUR_KEY"
```

Response:
```json
{
  "data": {
    "videoUrl": "https://www.cutad.web.id/api/proxy?u=...",
    "quality": "auto"
  }
}
```

> **Catatan:** URL video di-proxy lewat CUTAD untuk keamanan dan stabilitas.

### 🔍 Search

Cari drama atau film berdasarkan kata kunci.

```
GET /public/api/v1/melolo/search?q={keyword}
```

```bash
curl "https://www.cutad.web.id/public/api/v1/melolo/search?q=love" \
  -H "X-API-Key: YOUR_KEY"
```

Response:
```json
{
  "data": {
    "sections": [
      {
        "name": "Hasil Pencarian",
        "items": [
          {
            "id": "67890",
            "title": "Love Story",
            "cover": "https://...",
            "description": "Deskripsi...",
            "totalEpisodes": 18
          }
        ]
      }
    ]
  }
}
```

---

## ⚙️ Rate Limit

| Parameter | Nilai |
|-----------|-------|
| Limit | **30 request / menit** |
| Header response | `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` |
| Jika limit terlewati | HTTP 429 |

```bash
curl -I https://www.cutad.web.id/public/api/v1/melolo/rank \
  -H "X-API-Key: YOUR_KEY"

# Response headers:
# X-RateLimit-Limit: 30
# X-RateLimit-Remaining: 28
# X-RateLimit-Reset: 1712745600
```

---

## 📦 Contoh Kode

### JavaScript (fetch)

```javascript
const API_KEY = 'cutad_xxxxxxxxxxxx';
const BASE = 'https://www.cutad.web.id/public/api/v1/melolo';

// Ambil drama populer
async function getRank() {
  const res = await fetch(`${BASE}/rank`, {
    headers: { 'X-API-Key': API_KEY }
  });
  const data = await res.json();
  return data;
}

// Cari drama
async function searchDrama(keyword) {
  const res = await fetch(`${BASE}/search?q=${encodeURIComponent(keyword)}`, {
    headers: { 'X-API-Key': API_KEY }
  });
  const data = await res.json();
  return data;
}

// Ambil detail + episode
async function getDetail(id) {
  const res = await fetch(`${BASE}/detail?id=${id}`, {
    headers: { 'X-API-Key': API_KEY }
  });
  const data = await res.json();
  return data;
}

// Ambil URL video
async function getVideo(id) {
  const res = await fetch(`${BASE}/watch?id=${id}`, {
    headers: { 'X-API-Key': API_KEY }
  });
  const data = await res.json();
  return data.data.videoUrl;
}

// Contoh penggunaan
const popular = await getRank();
console.log(popular.data.sections[0].items);

const results = await searchDrama('romance');
console.log(results.data.sections[0].items);

const detail = await getDetail('12345');
console.log(detail.data.episodes);

const videoUrl = await getVideo('ep_001');
console.log(videoUrl);
```

### Python (requests)

```python
import requests

API_KEY = 'cutad_xxxxxxxxxxxx'
BASE = 'https://www.cutad.web.id/public/api/v1/melolo'
HEADERS = {'X-API-Key': API_KEY}

# Ambil drama populer
def get_rank():
    res = requests.get(f'{BASE}/rank', headers=HEADERS)
    return res.json()

# Cari drama
def search_drama(keyword):
    res = requests.get(f'{BASE}/search', 
        params={'q': keyword},
        headers=HEADERS
    )
    return res.json()

# Ambil detail + episode
def get_detail(content_id):
    res = requests.get(f'{BASE}/detail',
        params={'id': content_id},
        headers=HEADERS
    )
    return res.json()

# Ambil URL video
def get_video(video_id):
    res = requests.get(f'{BASE}/watch',
        params={'id': video_id},
        headers=HEADERS
    )
    return res.json()['data']['videoUrl']

# Contoh penggunaan
popular = get_rank()
for item in popular['data']['sections'][0]['items']:
    print(f"{item['title']} - {item['totalEpisodes']} episode")

results = search_drama('romance')
print(results)

detail = get_detail('12345')
for ep in detail['data']['episodes']:
    print(f"Episode {ep['episodeNumber']}: {ep['title']}")

video_url = get_video('ep_001')
print(f"Video: {video_url}")
```

### Node.js (node-fetch)

```javascript
import fetch from 'node-fetch';

const API_KEY = 'cutad_xxxxxxxxxxxx';
const BASE = 'https://www.cutad.web.id/public/api/v1/melolo';

const headers = { 'X-API-Key': API_KEY };

// Ambil drama populer
const rank = await fetch(`${BASE}/rank`, { headers }).then(r => r.json());
console.log(rank.data.sections[0].items);

// Cari drama
const search = await fetch(`${BASE}/search?q=love`, { headers }).then(r => r.json());
console.log(search.data.sections[0].items);

// Ambil detail
const detail = await fetch(`${BASE}/detail?id=12345`, { headers }).then(r => r.json());
console.log(detail.data.title, detail.data.episodes.length, 'episodes');

// Ambil video
const video = await fetch(`${BASE}/watch?id=ep_001`, { headers }).then(r => r.json());
console.log('Video URL:', video.data.videoUrl);
```

### cURL

```bash
# Drama populer
curl https://www.cutad.web.id/public/api/v1/melolo/rank \
  -H "X-API-Key: YOUR_KEY"

# Cari drama romantis
curl "https://www.cutad.web.id/public/api/v1/melolo/search?q=romance" \
  -H "X-API-Key: YOUR_KEY"

# Detail konten
curl "https://www.cutad.web.id/public/api/v1/melolo/detail?id=12345" \
  -H "X-API-Key: YOUR_KEY"

# URL video episode
curl "https://www.cutad.web.id/public/api/v1/melolo/watch?id=ep_001" \
  -H "X-API-Key: YOUR_KEY"
```

---

## 🚫 Error Handling

| HTTP Code | Arti |
|-----------|------|
| 200 | OK |
| 400 | Parameter kurang / salah |
| 401 | API key tidak dikirim |
| 403 | API key tidak valid |
| 429 | Rate limit terlewati |
| 500 | Error di server / upstream |

Contoh response error:
```json
{
  "error": "Rate limit exceeded",
  "message": "Limit: 30 requests per minute. Try again after ...",
  "limit": 30,
  "remaining": 0,
  "resetAt": 1712745600
}
```

```json
{
  "error": "Missing id parameter"
}
```

---

## 💡 Contoh Proyek yang Bisa Dibuat

- 🤖 Bot Telegram untuk notifikasi episode baru Melolo
- 📱 Aplikasi mobile nonton drama Melolo
- 🌐 Website mirror/clone Melolo
- 📊 Dashboard daftar drama Melolo + statistik
- 🔔 Notifikasi otomatis kalau ada episode baru

---

## 📖 Dokumentasi Lengkap

- **CUTAD API Docs:** [cutad.web.id/docs](https://www.cutad.web.id/docs)
- **Semua Provider:** [cutad-api-docs](https://github.com/rudiansyah1998/cutad-api-docs)
- **Website:** [cutad.web.id](https://www.cutad.web.id)

---

## 📜 Lisensi

MIT License — bebas digunakan, dimodifikasi, dan didistribusikan.

---

## ❤️ Dibuat oleh

[CUTAD](https://www.cutad.web.id) — Streaming Gratis Drama & Film Pendek Tanpa Iklan
