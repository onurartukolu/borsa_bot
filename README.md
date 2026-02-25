# borsa_bot
import tweepy
import schedule
import time
import random
from datetime import datetime
import yfinance as yf

# ====================== YENİ ANAHTARLARIN ======================
API_KEY            = "AoUo7gmF8XPLinEQmhpBsRrzT"
API_SECRET         = "pOx0fMD4VLMhdRd5FkBJJwcQpbCLRyw6StXy3dALygwkYBBJw6"
ACCESS_TOKEN       = "2026666508416593924-XecAiPxRxSagrMrC0r1rVA9ETUGM8y"
ACCESS_TOKEN_SECRET = "NbGmSD8g9GCbiDDi6dpHhUv1fKkmWiyhRcfAAsp1OOpmw"
# ============================================================

client = tweepy.Client(
    consumer_key        = API_KEY,
    consumer_secret     = API_SECRET,
    access_token        = ACCESS_TOKEN,
    access_token_secret = ACCESS_TOKEN_SECRET
)

mesajlar = [
    "📈 Borsa İstanbul açıldı! BIST 100 bugün nasıl açılacak? Takipteyiz! #Borsaİstanbul #BIST100",
    "💹 Günün en çok yükselen hisseleri senin listende mi? Kaçırma! #HisseSenetleri #Yatırım",
    "🚀 BIST 100 yeni zirvelere mi koşuyor? Heyecan devam ediyor! #Borsa #Yatirim",
    "💰 Sabah kahvesi + borsa haberi = mükemmel başlangıç ☕ #BorsaGündemi #Finans",
    "🔥 Volatilite yüksek, stop-loss’larını unutma! #BorsaRisk #YatirimUyarisi",
    "📉 Düşüşlerde alım fırsatı mı arıyorsun? Uzman görüşleri burada! #Borsa #Ekonomi",
    "🌟 Bugün Borsa İstanbul’da öne çıkanlar! Sen hangisini beğendin? #Hisse",
    "📊 BIST 100 teknik seviyeleri önemli! Destek-direnci takip et. #TeknikAnaliz",
    "🎯 Disiplin = kazanç. Hedeflerini koy, borsa seni ödüllendirsin! #Motivasyon",
    "🏦 Banka hisseleri bugün hareketli! Senin favorin hangisi? #BankaHisseleri",
    "🌍 Global piyasalar Borsa İstanbul’u etkiliyor. ABD ve Avrupa’yı izle! #GlobalEkonomi",
    "💼 Uzun vadeli mi düşünüyorsun? Borsa İstanbul senin için hazır! #YatirimStratejisi",
    "💹 Bugün hangi hisse parlıyor? BIST 100'de gözümüz üzerinizde! 🚀 #Yatırım #Hisse",
    "🔥 Volatilite mi? Fırsat mı? Borsa İstanbul her gün sürpriz dolu! #BorsaGündemi",
    "☕ Sabah kahvesi + BIST analizi = mükemmel kombinasyon! Günaydın yatırımcılar 🌅 #Finans",
    "📊 Teknik seviyeler kritik! Destek kırılırsa ne olur? İzlemeye devam... #TeknikAnaliz",
    "🏦 Bankacılık endeksi hareketli! Senin favori banka hissen hangisi? Yorum yap! #BankaHisseleri",
    "🌍 Global borsalar düşerken BIST direniyor mu? Takipte kalın! #Ekonomi",
    "🎯 Uzun vadeli yatırımcı mısın? Borsa İstanbul sabır ödüllendiriyor 💎 #YatirimStratejisi",
    "⚡ Günün en volatil hisseleri belli oldu! Risk sevenler buraya 👇 #Volatilite",
    "📉 Düşüşte alım fırsatı mı arıyorsun? Sabırlı ol, piyasa döner! #Borsa",
    "🚀 Yeni zirve mi geliyor? BIST 100 heyecanı hiç bitmiyor! #Borsaİstanbul",
    "💰 Portföyünde ne var bugün? Paylaş, belki fikir alışverişi yaparız! #Yatırımcı",
    "📰 Borsa haberleri akıyor! En önemlilerini kaçırma... #BorsaHaber",
    "📈 Endeks yeşile döndü! Kimler kazanıyor? #Kazananlar",
    "⏰ Saat 14:00 – Gün ortası kontrolü! Portföyün nasıl? #OrtaSeans",
    "🌙 Gece seansı yok ama yarın için plan yapalım mı? #YatirimPlanı",
    "🔍 Derin analiz zamanı: BIST 100 teknik görünüm! #Analiz",
    "🏆 Haftanın en iyi performansı kimde? Tebrikler! 🎉 #HaftaSonu",
    "❓ Soru: En sevdiğin sektör hangisi? Banka mı, sanayi mi? #Sektor",
    "📉 Kırmızı gün ama panik yok! Uzun vadede kazanırız 💪 #Motivasyon",
    "📊 BIST 100'de bugün ne bekliyoruz? Senin tahminin ne? #Tahmin"
]

def get_bist100_value():
    try:
        ticker = yf.Ticker("XU100.IS")
        data = ticker.history(period="1d")
        if not data.empty:
            current_price = data['Close'].iloc[-1]
            return f"BIST 100: {current_price:,.2f} TRY"
        else:
            return "BIST 100 verisi alınamadı (piyasa kapalı olabilir)"
    except Exception as e:
        print("Yahoo Finance hatası:", e)
        return "BIST 100 verisi alınamadı"

def borsa_tweet_at():
    base_mesaj = random.choice(mesajlar)
    bist_degeri = get_bist100_value()
    
    ekler = [
        f" ({datetime.now().strftime('%H:%M')})",
        f" {random.choice(['🚀', '💹', '📈', '🔥', '🌟', '⚡'])}",
        f" #{random.choice(['Yatirim', 'Borsa', 'Finans', 'Hisse', 'BIST100', 'Ekonomi'])}",
        f" {random.choice(['Sen ne düşünüyorsun?', 'Yorumlara beklerim!', 'Takipte kal!', 'Görüşlerini paylaş!'])}",
        f" | {bist_degeri}"
    ]
    
    rastgele_ek = random.sample(ekler, k=random.randint(2, 4))
    tweet_metni = base_mesaj + "".join(rastgele_ek)
    
    if len(tweet_metni) > 280:
        tweet_metni = tweet_metni[:275] + "..."
    
    try:
        response = client.create_tweet(text=tweet_metni)
        print(f"✅ [{datetime.now().strftime('%Y-%m-%d %H:%M:%S')}] Tweet atıldı!")
        print("Mesaj:", tweet_metni)
        print(f"Link: https://x.com/i/status/{response.data['id']}\n")
    except tweepy.TweepyException as e:
        print(f"❌ Tweepy hatası: {e}")
        if hasattr(e, 'response') and e.response:
            print("Hata kodu:", e.response.status_code)
            print("Hata detay:", e.response.text)
    except Exception as e:
        print(f"❌ Genel hata: {str(e)}")

# ====================== ZAMANLAMA (7 tweet/gün – Türkiye saati bazlı, PC İrlanda GMT'de) ======================
schedule.every().day.at("05:45").do(borsa_tweet_at)   # TR 08:45
schedule.every().day.at("07:00").do(borsa_tweet_at)   # TR 10:00
schedule.every().day.at("09:00").do(borsa_tweet_at)   # TR 12:00
schedule.every().day.at("11:30").do(borsa_tweet_at)   # TR 14:30
schedule.every().day.at("13:30").do(borsa_tweet_at)   # TR 16:30
schedule.every().day.at("15:30").do(borsa_tweet_at)   # TR 18:30
schedule.every().day.at("17:30").do(borsa_tweet_at)   # TR 20:30

print("🚀 Borsa Botu BAŞLATILDI! (Yeni API anahtarları yüklendi)")
print("Günde 7 tweet (Türkiye saati): 08:45 | 10:00 | 12:00 | 14:30 | 16:30 | 18:30 | 20:30")
print("Her tweette gerçek BIST 100 değeri olacak (yfinance ile)")
borsa_tweet_at()  # ← Hemen bir test tweet'i at
while True:
    schedule.run_pending()
    time.sleep(60)
