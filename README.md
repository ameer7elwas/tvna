<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>TvNa - تيڤي نا</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css" rel="stylesheet">
  <style>
    body { background-color: #f8f9fa; font-family: 'Segoe UI', sans-serif; }
    .ad-card, .channel-card, .news-card {
      border-radius: 15px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      overflow: hidden;
      margin-bottom: 30px;
    }
    .ad-card img, .channel-card img, .news-card img {
      width: 100%;
      height: 200px;
      object-fit: cover;
    }
    h2.section-title {
      margin: 30px 0 20px;
      border-bottom: 3px solid #007bff;
      display: inline-block;
      padding-bottom: 5px;
    }
  </style>
</head>
<body class="container py-4">
  <h1 class="text-center mb-4">🎬 تيڤي نا - TvNa</h1>

  <h2 class="section-title">📢 الإعلانات</h2>
  <div id="ads" class="row"></div>

  <h2 class="section-title">📺 القنوات</h2>
  <div id="channels" class="row"></div>

  <h2 class="section-title">📰 آخر الأخبار</h2>
  <div id="news" class="row"></div>

  <script>
    const SUPABASE_URL = 'https://vpvvjascwgivdjyyhzwp.supabase.co';
    const API_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InZwdnZqYXNjd2dpdmRqeXloendwIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDk4MDYxMjYsImV4cCI6MjA2NTM4MjEyNn0.6AR2-MG4x9ugNTXe9jUqx-IwGEtj1m6MCYwQkTsSbUQ';
    const HEADERS = {
      'apikey': API_KEY,
      'Authorization': 'Bearer ' + API_KEY,
      'Content-Type': 'application/json'
    };

    async function loadAds() {
      const res = await fetch(`${SUPABASE_URL}/rest/v1/advertisements?select=*`, { headers: HEADERS });
      const ads = await res.json();
      const container = document.getElementById('ads');
      ads.forEach(ad => {
        if (ad.is_active) {
          container.innerHTML += `
            <div class="col-md-6">
              <div class="ad-card bg-white p-3">
                <img src="${ad.image_url}" alt="ad">
                <h5 class="mt-2">${ad.title}</h5>
                <p>${ad.description}</p>
                ${ad.link ? `<a href="${ad.link}" target="_blank">🔗 رابط</a>` : ''}
              </div>
            </div>`;
        }
      });
    }

    async function loadChannels() {
      const res = await fetch(`${SUPABASE_URL}/rest/v1/channels?select=*`, { headers: HEADERS });
      const channels = await res.json();
      const container = document.getElementById('channels');
      channels.forEach(ch => {
        if (ch.is_active && !ch.is_premium) {
          container.innerHTML += `
            <div class="col-md-4">
              <div class="channel-card bg-white p-3">
                <img src="${ch.image_url}" alt="channel">
                <h6 class="mt-2">${ch.name_ar}</h6>
                <span class="badge bg-primary">${ch.quality}</span>
              </div>
            </div>`;
        }
      });
    }

    async function loadNews() {
      const res = await fetch(`${SUPABASE_URL}/rest/v1/news?select=*`, { headers: HEADERS });
      const news = await res.json();
      const container = document.getElementById('news');
      news.slice(0, 5).forEach(item => {
        container.innerHTML += `
          <div class="col-md-6">
            <div class="news-card bg-white p-3">
              <img src="${item.urlToImage || 'https://via.placeholder.com/600x200?text=خبر'}" alt="news">
              <h6 class="mt-2">${item.titleAr || item.title}</h6>
              <p>${item.contentAr || item.content}</p>
              ${item.url ? `<a href="${item.url}" target="_blank">📎 مصدر</a>` : ''}
            </div>
          </div>`;
      });
    }

    loadAds();
    loadChannels();
    loadNews();
  </script>
</body>
</html>
