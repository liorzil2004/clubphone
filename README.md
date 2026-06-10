import { useState, useEffect, useRef } from "react";
const COLORS = {
  bg: "#0a0a0f",
  surface: "#13131a",
  card: "#1a1a24",
  accent: "#ff6b00",
  accentGlow: "#ff6b0033",
  accentSoft: "#ff8c3a",
  blue: "#00b4ff",
  blueGlow: "#00b4ff22",
  text: "#f0f0f8",
  muted: "#8888a8",
  border: "#2a2a3a",
  gold: "#ffd700",
};

const nav = ["בית", "סמארטפונים", "אביזרים", "מעבדה", "אודות", "צור קשר"];

const brands = [
  { name: "iPhone", icon: "🍎", color: "#888", models: ["iPhone 16 Pro Max","iPhone 16 Pro","iPhone 16","iPhone 15 Pro Max","iPhone 15"], from: 3490 },
  { name: "Samsung Galaxy", icon: "🌀", color: "#1428A0", models: ["Galaxy S25 Ultra","Galaxy S25+","Galaxy S25","Galaxy A55","Galaxy A35"], from: 1490 },
  { name: "Xiaomi", icon: "⚡", color: "#ff6900", models: ["Xiaomi 14 Ultra","Xiaomi 14T Pro","Redmi Note 13 Pro","Poco X6 Pro"], from: 890 },
  { name: "Google Pixel", icon: "🔵", color: "#4285F4", models: ["Pixel 9 Pro","Pixel 9","Pixel 8a"], from: 2190 },
  { name: "OnePlus", icon: "🔴", color: "#eb0029", models: ["OnePlus 12","OnePlus Nord 4"], from: 1290 },
  { name: "OPPO", icon: "🟢", color: "#1d7a46", models: ["OPPO Find X8 Pro","OPPO Reno 12"], from: 990 },
];

const phones = {
  "iPhone 16 Pro Max": { price: 5490, storage: "256GB/512GB/1TB", chip: "A18 Pro", camera: "48MP + 48MP + 12MP", battery: "4685mAh", special: ["Camera Control חדש","Action Button","Titanium Frame","ProRes Video 4K120fps"], desc: "הדגל המוחלט של אפל. מסך 6.9 אינץ' Super Retina XDR, עיצוב טיטניום יוקרתי וביצועים שאין שני להם." },
  "iPhone 16 Pro": { price: 4690, storage: "128GB/256GB/512GB", chip: "A18 Pro", camera: "48MP + 48MP + 12MP", battery: "3582mAh", special: ["Camera Control","ProRes 4K120fps","Titanium","Display Always-On"], desc: "כל עוצמת ה-Pro במסגרת קומפקטית יותר. המצלמה הטובה בעולם בכף ידך." },
  "iPhone 16": { price: 3490, storage: "128GB/256GB/512GB", chip: "A18", camera: "48MP + 12MP", battery: "3561mAh", special: ["Camera Control","Action Button","iOS 18 AI Features","MagSafe"], desc: "הדור החדש של iPhone הבסיסי – עם יכולות AI מתקדמות ועיצוב מרענן." },
  "Galaxy S25 Ultra": { price: 5190, storage: "256GB/512GB/1TB", chip: "Snapdragon 8 Elite", camera: "200MP + 50MP + 10MP + 10MP", battery: "5000mAh", special: ["S Pen מובנה","Galaxy AI","Titanium Frame","Night Mode מתקדם"], desc: "המלך הבלתי מעורער של אנדרואיד. עם עט S-Pen, AI מובנה וצילום ב-200MP." },
  "Galaxy S25+": { price: 4190, storage: "256GB/512GB", chip: "Snapdragon 8 Elite", camera: "50MP + 12MP + 10MP", battery: "4900mAh", special: ["Galaxy AI","7 שנות עדכונים","Wi-Fi 7","ProVisual Engine"], desc: "המאוזן המושלם בסדרת S25. מסך גדול, סוללה ענקית וביצועים פנומנליים." },
  "Galaxy S25": { price: 3390, storage: "128GB/256GB", chip: "Snapdragon 8 Elite", camera: "50MP + 12MP + 10MP", battery: "4000mAh", special: ["Galaxy AI","Slim Design","Wi-Fi 7","ProVisual"], desc: "הכניסה המושלמת לעולם הדגלים של סמסונג 2025." },
};

const accessories = [
  { cat: "מגנים לטלפון", icon: "🛡️", items: ["כיסוי MagSafe Premium","כיסוי שקוף Anti-Shock","כיסוי ארנק עור","כיסוי Magsafe Silicone","כיסוי ממותג Spigen","כיסוי עמיד מים"], from: 29 },
  { cat: "מגני מסך", icon: "💎", items: ["זכוכית מחוסמת 9H","מגן Privacy","מגן Full Glue","מגן מט Anti-Glare","מגן עם מסגרת","מגן Privacy 3D"], from: 19 },
  { cat: "מטענים וכבלים", icon: "⚡", items: ["מטען GaN 65W","מטען אלחוטי MagSafe","כבל USB-C 3m","מטען רכב PD 45W","Power Bank 20000mAh","מטען שולחני 4 יציאות"], from: 39 },
  { cat: "אוזניות", icon: "🎧", items: ["AirPods Pro 2","Samsung Galaxy Buds 3","אוזניות Bluetooth ANC","אוזניות Gaming","אוזניות ספורט IP68","TWS Premium"], from: 89 },
  { cat: "שעונים חכמים", icon: "⌚", items: ["Apple Watch Series 10","Samsung Galaxy Watch 7","Xiaomi Smart Band 9","Garmin Vivoactive 5","Amazfit GTR 4"], from: 199 },
  { cat: "ציוד צילום", icon: "📸", items: ["עדשת Macro מגנטית","Gimbal חצובה","מיקרופון סטריאו","LED Ring Light","חצובה נייד","מגנט Adapter 17mm"], from: 49 },
];

const reviews = [
  { name: "יוסי כהן", stars: 5, text: "שירות מדהים, דני עזר לי לבחור טלפון בדיוק לפי הצרכים שלי. המחירים הכי טובים באילת ללא ספק!", date: "ינואר 2025" },
  { name: "מיכל לוי", stars: 5, text: "תיקנו לי את המסך תוך שעה ובמחיר מדהים. המקצועיות כאן ברמה אחרת לגמרי", date: "פברואר 2025" },
  { name: "אבי שמעון", stars: 5, text: "קניתי iPhone 16 Pro ב-500 שקל פחות מכל מקום אחר. ללא מע\"מ זה משנה הכל!", date: "מרץ 2025" },
  { name: "רחל אברהם", stars: 5, text: "הגעתי לתקן מסך שבור ויצאתי עם טלפון חדש לגמרי :-) השירות חם ואנושי כמו פעם", date: "אפריל 2025" },
  { name: "דוד בוזגלו", stars: 5, text: "10 שנים קונה רק אצל דני. כל פעם מחדש אותה חוויה מדהימה, שירות אישי ואמין", date: "מאי 2025" },
];

const deals = [
  { label: "🔥 Black Friday", bg: "linear-gradient(135deg,#1a0a00,#3a1500)", border: "#ff6b00", title: "עד 40% הנחה", sub: "על כל סמארטפוני הדגל", badge: "נובמבר 2025" },
  { label: "🎁 קיץ 2025", bg: "linear-gradient(135deg,#001a2a,#002a4a)", border: "#00b4ff", title: "קנה מכשיר, קבל מגן מסך חינם", sub: "בכל רכישה מעל 1,500₪", badge: "פעיל עכשיו" },
  { label: "🌟 מבצע ייחודי", bg: "linear-gradient(135deg,#0a1a00,#1a3000)", border: "#4caf50", title: "ללא מע\"מ כל השנה", sub: "חיסכון של 18% על כל מוצר", badge: "אילת בלבד" },
];

function StarRating({ n }) {
  return <span style={{ color: COLORS.gold, fontSize: 14 }}>{"★".repeat(n)}{"☆".repeat(5 - n)}</span>;
}

function Tag({ children, color = COLORS.accent }) {
  return (
    <span style={{
      background: color + "22",
      color,
      border: `1px solid ${color}55`,
      borderRadius: 6,
      padding: "2px 10px",
      fontSize: 12,
      fontWeight: 700,
      whiteSpace: "nowrap",
    }}>{children}</span>
  );
}

export default function ClubPhone() {
  const [page, setPage] = useState("בית");
  const [selectedBrand, setSelectedBrand] = useState(null);
  const [selectedPhone, setSelectedPhone] = useState(null);
  const [menuOpen, setMenuOpen] = useState(false);
  const [scrolled, setScrolled] = useState(false);

  useEffect(() => {
    const fn = () => setScrolled(window.scrollY > 40);
    window.addEventListener("scroll", fn);
    return () => window.removeEventListener("scroll", fn);
  }, []);

  const go = (p) => { setPage(p); setMenuOpen(false); setSelectedBrand(null); setSelectedPhone(null); window.scrollTo(0, 0); };

  return (
    <div dir="rtl" style={{ minHeight: "100vh", background: COLORS.bg, color: COLORS.text, fontFamily: "'Segoe UI', 'Arial Hebrew', sans-serif", direction: "rtl" }}>
      {/* HEADER */}
      <header style={{
        position: "sticky", top: 0, zIndex: 100,
        background: scrolled ? "rgba(10,10,15,0.97)" : "rgba(10,10,15,0.85)",
        backdropFilter: "blur(18px)",
        borderBottom: `1px solid ${scrolled ? COLORS.border : "transparent"}`,
        transition: "all 0.3s",
        padding: "0 24px",
      }}>
        <div style={{ maxWidth: 1200, margin: "0 auto", display: "flex", alignItems: "center", justifyContent: "space-between", height: 68 }}>
          {/* Logo */}
          <div onClick={() => go("בית")} style={{ cursor: "pointer", display: "flex", alignItems: "center", gap: 10 }}>
            <div style={{
              width: 42, height: 42, borderRadius: 12,
              background: `linear-gradient(135deg, ${COLORS.accent}, ${COLORS.accentSoft})`,
              display: "flex", alignItems: "center", justifyContent: "center",
              fontSize: 22, boxShadow: `0 0 20px ${COLORS.accentGlow}`,
            }}>📱</div>
            <div>
              <div style={{ fontWeight: 900, fontSize: 20, letterSpacing: -0.5, lineHeight: 1.1 }}>
                <span style={{ color: COLORS.accent }}>Club</span>
                <span style={{ color: COLORS.text }}>Phone</span>
              </div>
              <div style={{ fontSize: 10, color: COLORS.muted, letterSpacing: 1 }}>אילת • ללא מע"מ</div>
            </div>
          </div>

          {/* Desktop Nav */}
          <nav style={{ display: "flex", gap: 4 }}>
            {nav.map(n => (
              <button key={n} onClick={() => go(n)} style={{
                background: page === n ? `${COLORS.accent}18` : "transparent",
                color: page === n ? COLORS.accent : COLORS.muted,
                border: page === n ? `1px solid ${COLORS.accent}44` : "1px solid transparent",
                borderRadius: 8, padding: "7px 14px", cursor: "pointer",
                fontWeight: page === n ? 700 : 500, fontSize: 14, transition: "all 0.2s",
              }}>{n}</button>
            ))}
          </nav>

          {/* Contact pill */}
          <div style={{
            display: "flex", alignItems: "center", gap: 8,
            background: COLORS.card, borderRadius: 12, padding: "8px 14px",
            border: `1px solid ${COLORS.border}`,
          }}>
            <span style={{ fontSize: 16 }}>📞</span>
            <div>
              <div style={{ fontSize: 13, fontWeight: 700, color: COLORS.text }}>08-6374444</div>
              <div style={{ fontSize: 10, color: COLORS.muted }}>דני • ClubPhone</div>
            </div>
          </div>
        </div>
      </header>

      {/* PAGES */}
      <main style={{ maxWidth: 1200, margin: "0 auto", padding: "0 24px 80px" }}>

        {/* ===== בית ===== */}
        {page === "בית" && (
          <div>
            {/* Hero */}
            <div style={{
              margin: "40px 0 48px",
              borderRadius: 24,
              overflow: "hidden",
              position: "relative",
              minHeight: 360,
              background: "linear-gradient(135deg, #0a0a0f 0%, #1a0800 40%, #0a0f1a 100%)",
              border: `1px solid ${COLORS.border}`,
              display: "flex", alignItems: "center",
            }}>
              {/* glow orbs */}
              <div style={{ position: "absolute", width: 300, height: 300, borderRadius: "50%", background: `radial-gradient(circle, ${COLORS.accentGlow} 0%, transparent 70%)`, top: -80, right: -60 }} />
              <div style={{ position: "absolute", width: 200, height: 200, borderRadius: "50%", background: `radial-gradient(circle, ${COLORS.blueGlow} 0%, transparent 70%)`, bottom: -60, left: 100 }} />

              <div style={{ position: "relative", zIndex: 2, padding: "48px 56px", flex: 1 }}>
                <div style={{ display: "flex", gap: 8, marginBottom: 20, flexWrap: "wrap" }}>
                  <Tag>ללא מע"מ 🏷️</Tag>
                  <Tag color={COLORS.blue}>אילת • מרכז העיר</Tag>
                  <Tag color="#4caf50">מעבדה מקצועית</Tag>
                </div>
                <h1 style={{ fontSize: 48, fontWeight: 900, margin: "0 0 12px", lineHeight: 1.1 }}>
                  <span style={{ color: COLORS.accent }}>ClubPhone</span>
                  <br />
                  <span style={{ color: COLORS.text }}>הטכנולוגיה</span>
                  <span style={{ color: COLORS.muted }}> שלך,</span>
                  <br />
                  <span style={{ color: COLORS.text }}>במחיר הכי</span>
                  <span style={{ color: COLORS.accentSoft }}> טוב</span>
                </h1>
                <p style={{ color: COLORS.muted, fontSize: 16, maxWidth: 460, marginBottom: 28 }}>
                  מגוון הכי גדול של סמארטפונים, אביזרים וציוד טכנולוגי — ללא מע"מ. 
                  <strong style={{ color: COLORS.text }}> חיסכון של 18%</strong> על כל מוצר!
                </p>
                <div style={{ display: "flex", gap: 12, flexWrap: "wrap" }}>
                  <button onClick={() => go("סמארטפונים")} style={{
                    background: `linear-gradient(135deg, ${COLORS.accent}, ${COLORS.accentSoft})`,
                    color: "#fff", border: "none", borderRadius: 12, padding: "13px 28px",
                    fontSize: 15, fontWeight: 700, cursor: "pointer",
                    boxShadow: `0 4px 24px ${COLORS.accentGlow}`,
                  }}>🛒 לכל הטלפונים</button>
                  <button onClick={() => go("צור קשר")} style={{
                    background: "transparent", color: COLORS.text,
                    border: `1px solid ${COLORS.border}`, borderRadius: 12,
                    padding: "13px 28px", fontSize: 15, fontWeight: 600, cursor: "pointer",
                  }}>📞 צור קשר</button>
                </div>
              </div>

              {/* Store image placeholder */}
              <div style={{
                position: "relative", zIndex: 2, padding: "40px 48px 40px 0",
                display: "flex", flexDirection: "column", alignItems: "center", gap: 16,
              }}>
                <div style={{
                  width: 200, height: 200, borderRadius: 20,
                  background: "linear-gradient(135deg, #1a1a28, #2a2a3a)",
                  border: `1px solid ${COLORS.border}`,
                  display: "flex", flexDirection: "column", alignItems: "center", justifyContent: "center",
                  fontSize: 64,
                }}>
                  🏪
                  <div style={{ fontSize: 13, color: COLORS.muted, marginTop: 8, textAlign: "center", padding: "0 16px" }}>
                    קלאב פון<br />שדרות התמרים 37
                  </div>
                </div>
              </div>
            </div>

            {/* Deals */}
            <section style={{ marginBottom: 56 }}>
              <h2 style={{ fontSize: 26, fontWeight: 800, marginBottom: 24 }}>
                <span style={{ color: COLORS.accent }}>🔥</span> מבצעים ואירועים
              </h2>
              <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(280px, 1fr))", gap: 20 }}>
                {deals.map((d, i) => (
                  <div key={i} style={{
                    borderRadius: 18, overflow: "hidden",
                    background: d.bg,
                    border: `1px solid ${d.border}44`,
                    padding: "28px 24px",
                    position: "relative",
                    boxShadow: `0 4px 24px ${d.border}18`,
                  }}>
                    <div style={{ fontSize: 13, color: d.border, fontWeight: 700, marginBottom: 8 }}>{d.label}</div>
                    <div style={{ fontSize: 24, fontWeight: 900, marginBottom: 6, color: COLORS.text }}>{d.title}</div>
                    <div style={{ fontSize: 14, color: COLORS.muted, marginBottom: 16 }}>{d.sub}</div>
                    <span style={{ background: d.border + "22", color: d.border, border: `1px solid ${d.border}44`, borderRadius: 20, padding: "4px 12px", fontSize: 12, fontWeight: 700 }}>{d.badge}</span>
                  </div>
                ))}
              </div>
            </section>

            {/* Quick brands */}
            <section style={{ marginBottom: 56 }}>
              <h2 style={{ fontSize: 26, fontWeight: 800, marginBottom: 24 }}>📱 חפש לפי מותג</h2>
              <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(160px, 1fr))", gap: 14 }}>
                {brands.map(b => (
                  <div key={b.name} onClick={() => { setSelectedBrand(b.name); go("סמארטפונים"); }} style={{
                    background: COLORS.card, border: `1px solid ${COLORS.border}`,
                    borderRadius: 16, padding: "20px 16px", cursor: "pointer", textAlign: "center",
                    transition: "all 0.2s",
                  }}
                    onMouseEnter={e => { e.currentTarget.style.borderColor = COLORS.accent; e.currentTarget.style.transform = "translateY(-3px)"; }}
                    onMouseLeave={e => { e.currentTarget.style.borderColor = COLORS.border; e.currentTarget.style.transform = ""; }}>
                    <div style={{ fontSize: 32, marginBottom: 8 }}>{b.icon}</div>
                    <div style={{ fontWeight: 700, fontSize: 14 }}>{b.name}</div>
                    <div style={{ color: COLORS.accent, fontSize: 12, marginTop: 4 }}>מ-{b.from}₪</div>
                  </div>
                ))}
              </div>
            </section>

            {/* Reviews */}
            <section style={{ marginBottom: 8 }}>
              <h2 style={{ fontSize: 26, fontWeight: 800, marginBottom: 24 }}>⭐ מה אומרים הלקוחות</h2>
              <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(260px, 1fr))", gap: 16 }}>
                {reviews.map((r, i) => (
                  <div key={i} style={{
                    background: COLORS.card, borderRadius: 16,
                    border: `1px solid ${COLORS.border}`, padding: "20px",
                  }}>
                    <div style={{ display: "flex", justifyContent: "space-between", marginBottom: 10 }}>
                      <span style={{ fontWeight: 700 }}>{r.name}</span>
                      <StarRating n={r.stars} />
                    </div>
                    <p style={{ color: COLORS.muted, fontSize: 13, lineHeight: 1.6, margin: 0 }}>{r.text}</p>
                    <div style={{ fontSize: 11, color: COLORS.border, marginTop: 10 }}>{r.date}</div>
                  </div>
                ))}
              </div>
            </section>
          </div>
        )}

        {/* ===== סמארטפונים ===== */}
        {page === "סמארטפונים" && (
          <div style={{ display: "grid", gridTemplateColumns: "260px 1fr", gap: 32, marginTop: 40 }}>
            {/* Sidebar */}
            <aside>
              <div style={{ background: COLORS.card, border: `1px solid ${COLORS.border}`, borderRadius: 18, overflow: "hidden", position: "sticky", top: 90 }}>
                <div style={{ padding: "18px 20px", borderBottom: `1px solid ${COLORS.border}`, fontSize: 14, fontWeight: 700, color: COLORS.muted, letterSpacing: 1 }}>
                  📱 מותגים
                </div>
                {brands.map(b => (
                  <div key={b.name} onClick={() => { setSelectedBrand(b.name === selectedBrand ? null : b.name); setSelectedPhone(null); }}
                    style={{
                      padding: "14px 20px", cursor: "pointer", display: "flex", alignItems: "center", gap: 12,
                      background: selectedBrand === b.name ? `${COLORS.accent}15` : "transparent",
                      borderRight: selectedBrand === b.name ? `3px solid ${COLORS.accent}` : "3px solid transparent",
                      transition: "all 0.2s",
                    }}>
                    <span style={{ fontSize: 20 }}>{b.icon}</span>
                    <div>
                      <div style={{ fontSize: 14, fontWeight: 600, color: selectedBrand === b.name ? COLORS.accent : COLORS.text }}>{b.name}</div>
                      <div style={{ fontSize: 11, color: COLORS.muted }}>מ-{b.from}₪</div>
                    </div>
                  </div>
                ))}

                <div style={{ padding: "18px 20px", borderTop: `1px solid ${COLORS.border}`, borderBottom: `1px solid ${COLORS.border}`, fontSize: 14, fontWeight: 700, color: COLORS.muted, letterSpacing: 1, marginTop: 8 }}>
                  🔧 שירותים
                </div>
                {["תיקון מסכים", "החלפת סוללה", "תיקון מטען", "שחזור נתונים"].map(s => (
                  <div key={s} onClick={() => go("מעבדה")} style={{ padding: "12px 20px", cursor: "pointer", fontSize: 13, color: COLORS.muted, display: "flex", alignItems: "center", gap: 8 }}>
                    <span style={{ color: COLORS.accent }}>›</span> {s}
                  </div>
                ))}
              </div>
            </aside>

            {/* Main content */}
            <div>
              {!selectedPhone ? (
                <>
                  <div style={{ marginBottom: 28 }}>
                    <h1 style={{ fontSize: 32, fontWeight: 900, margin: "0 0 8px" }}>
                      {selectedBrand ? `📱 ${selectedBrand}` : "כל הסמארטפונים"}
                    </h1>
                    <p style={{ color: COLORS.muted, margin: 0 }}>מחירים ללא מע"מ — חיסכון של 18% לעומת שאר הארץ</p>
                  </div>

                  {(selectedBrand ? brands.filter(b => b.name === selectedBrand) : brands).map(brand => (
                    <div key={brand.name} style={{ marginBottom: 40 }}>
                      <div style={{ display: "flex", alignItems: "center", gap: 12, marginBottom: 18 }}>
                        <span style={{ fontSize: 28 }}>{brand.icon}</span>
                        <h2 style={{ fontSize: 22, fontWeight: 800, margin: 0 }}>{brand.name}</h2>
                        <Tag>מ-{brand.from}₪</Tag>
                      </div>
                      <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fill, minmax(220px, 1fr))", gap: 14 }}>
                        {brand.models.map(m => {
                          const info = phones[m];
                          return (
                            <div key={m} onClick={() => setSelectedPhone(m)} style={{
                              background: COLORS.card, border: `1px solid ${COLORS.border}`,
                              borderRadius: 16, padding: "20px", cursor: "pointer", transition: "all 0.2s",
                            }}
                              onMouseEnter={e => { e.currentTarget.style.borderColor = COLORS.accent; e.currentTarget.style.transform = "translateY(-3px)"; }}
                              onMouseLeave={e => { e.currentTarget.style.borderColor = COLORS.border; e.currentTarget.style.transform = ""; }}>
                              <div style={{ fontSize: 36, marginBottom: 10, textAlign: "center" }}>{brand.icon}</div>
                              <div style={{ fontWeight: 700, fontSize: 14, marginBottom: 6 }}>{m}</div>
                              {info ? (
                                <>
                                  <div style={{ color: COLORS.accent, fontWeight: 800, fontSize: 18, marginBottom: 8 }}>₪{info.price.toLocaleString()}</div>
                                  <div style={{ fontSize: 11, color: COLORS.muted }}>{info.chip}</div>
                                  <div style={{ fontSize: 11, color: COLORS.muted }}>{info.camera}</div>
                                </>
                              ) : (
                                <div style={{ color: COLORS.muted, fontSize: 12 }}>לפרטים ומחיר — לחץ</div>
                              )}
                              <div style={{ marginTop: 12, fontSize: 12, color: COLORS.blue, fontWeight: 600 }}>לפרטים מלאים ›</div>
                            </div>
                          );
                        })}
                      </div>
                    </div>
                  ))}
                </>
              ) : (
                /* Phone Detail */
                (() => {
                  const info = phones[selectedPhone] || {};
                  const brand = brands.find(b => b.models.includes(selectedPhone));
                  return (
                    <div>
                      <button onClick={() => setSelectedPhone(null)} style={{
                        background: "transparent", color: COLORS.muted, border: `1px solid ${COLORS.border}`,
                        borderRadius: 8, padding: "8px 16px", cursor: "pointer", marginBottom: 24, fontSize: 13,
                      }}>← חזרה</button>

                      <div style={{ background: COLORS.card, borderRadius: 24, border: `1px solid ${COLORS.border}`, overflow: "hidden" }}>
                        <div style={{ background: "linear-gradient(135deg, #1a0800, #0a0a1a)", padding: "40px", display: "flex", alignItems: "center", gap: 32 }}>
                          <div style={{ fontSize: 80 }}>{brand?.icon}</div>
                          <div>
                            <h1 style={{ fontSize: 36, fontWeight: 900, margin: "0 0 8px" }}>{selectedPhone}</h1>
                            <div style={{ fontSize: 42, fontWeight: 900, color: COLORS.accent, marginBottom: 12 }}>
                              {info.price ? `₪${info.price.toLocaleString()}` : "לפרטים צלצל"}
                            </div>
                            <div style={{ display: "flex", gap: 8, flexWrap: "wrap" }}>
                              <Tag>ללא מע"מ</Tag>
                              <Tag color={COLORS.blue}>אחריות יבואן</Tag>
                              <Tag color="#4caf50">במלאי</Tag>
                            </div>
                          </div>
                        </div>

                        <div style={{ padding: "32px 40px" }}>
                          {info.desc && <p style={{ color: COLORS.muted, fontSize: 16, lineHeight: 1.7, marginBottom: 32 }}>{info.desc}</p>}

                          <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 24, marginBottom: 32 }}>
                            {info.chip && <div style={{ background: COLORS.surface, borderRadius: 12, padding: "16px 20px" }}>
                              <div style={{ fontSize: 11, color: COLORS.muted, marginBottom: 4 }}>מעבד</div>
                              <div style={{ fontWeight: 700 }}>{info.chip}</div>
                            </div>}
                            {info.camera && <div style={{ background: COLORS.surface, borderRadius: 12, padding: "16px 20px" }}>
                              <div style={{ fontSize: 11, color: COLORS.muted, marginBottom: 4 }}>מצלמה</div>
                              <div style={{ fontWeight: 700 }}>{info.camera}</div>
                            </div>}
                            {info.storage && <div style={{ background: COLORS.surface, borderRadius: 12, padding: "16px 20px" }}>
                              <div style={{ fontSize: 11, color: COLORS.muted, marginBottom: 4 }}>אחסון</div>
                              <div style={{ fontWeight: 700 }}>{info.storage}</div>
                            </div>}
                            {info.battery && <div style={{ background: COLORS.surface, borderRadius: 12, padding: "16px 20px" }}>
                              <div style={{ fontSize: 11, color: COLORS.muted, marginBottom: 4 }}>סוללה</div>
                              <div style={{ fontWeight: 700 }}>{info.battery}</div>
                            </div>}
                          </div>

                          {info.special && (
                            <div style={{ marginBottom: 32 }}>
                              <div style={{ fontSize: 14, fontWeight: 700, marginBottom: 14, color: COLORS.accent }}>✨ חידושים בדגם זה</div>
                              <div style={{ display: "flex", gap: 8, flexWrap: "wrap" }}>
                                {info.special.map(s => <Tag key={s} color={COLORS.blue}>{s}</Tag>)}
                              </div>
                            </div>
                          )}

                          <div style={{ display: "flex", gap: 12 }}>
                            <a href="https://wa.me/9720863744444" style={{
                              background: "#25D366", color: "#fff", borderRadius: 12,
                              padding: "13px 24px", fontWeight: 700, fontSize: 15, textDecoration: "none",
                              display: "inline-flex", alignItems: "center", gap: 8,
                            }}>💬 שלח וואטסאפ</a>
                            <a href="tel:0863744444" style={{
                              background: `linear-gradient(135deg, ${COLORS.accent}, ${COLORS.accentSoft})`,
                              color: "#fff", borderRadius: 12, padding: "13px 24px",
                              fontWeight: 700, fontSize: 15, textDecoration: "none",
                              display: "inline-flex", alignItems: "center", gap: 8,
                            }}>📞 התקשר עכשיו</a>
                          </div>
                        </div>
                      </div>
                    </div>
                  );
                })()
              )}
            </div>
          </div>
        )}

        {/* ===== אביזרים ===== */}
        {page === "אביזרים" && (
          <div style={{ marginTop: 40 }}>
            <h1 style={{ fontSize: 32, fontWeight: 900, marginBottom: 8 }}>🛍️ אביזרים נלווים</h1>
            <p style={{ color: COLORS.muted, marginBottom: 36 }}>כל מה שהטלפון שלך צריך — ללא מע"מ!</p>
            <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(320px, 1fr))", gap: 24 }}>
              {accessories.map(a => (
                <div key={a.cat} style={{ background: COLORS.card, border: `1px solid ${COLORS.border}`, borderRadius: 20, overflow: "hidden" }}>
                  <div style={{ background: "linear-gradient(135deg, #1a1020, #0a1520)", padding: "20px 24px", display: "flex", alignItems: "center", gap: 12 }}>
                    <span style={{ fontSize: 28 }}>{a.icon}</span>
                    <div>
                      <div style={{ fontWeight: 800, fontSize: 17 }}>{a.cat}</div>
                      <div style={{ color: COLORS.accent, fontSize: 13 }}>מ-{a.from}₪</div>
                    </div>
                  </div>
                  <div style={{ padding: "16px 24px" }}>
                    {a.items.map(item => (
                      <div key={item} style={{ display: "flex", alignItems: "center", gap: 10, padding: "10px 0", borderBottom: `1px solid ${COLORS.border}` }}>
                        <span style={{ color: COLORS.accent, fontSize: 12 }}>●</span>
                        <span style={{ fontSize: 13 }}>{item}</span>
                      </div>
                    ))}
                    <button onClick={() => go("צור קשר")} style={{
                      marginTop: 16, width: "100%", background: `${COLORS.accent}18`,
                      color: COLORS.accent, border: `1px solid ${COLORS.accent}44`,
                      borderRadius: 10, padding: "10px", fontSize: 13, fontWeight: 700, cursor: "pointer",
                    }}>לפרטים ומחירים 📞</button>
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ===== מעבדה ===== */}
        {page === "מעבדה" && (
          <div style={{ marginTop: 40 }}>
            <h1 style={{ fontSize: 32, fontWeight: 900, marginBottom: 8 }}>🔧 מעבדה מקצועית</h1>
            <p style={{ color: COLORS.muted, marginBottom: 36 }}>תיקון מקצועי, מהיר ובמחיר שווה — בלב אילת</p>

            <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(260px, 1fr))", gap: 20, marginBottom: 40 }}>
              {[
                { icon: "📱", title: "החלפת מסך", desc: "מסכים מקוריים לכל הדגמים. שעה עד שעתיים בלבד", price: "מ-199₪", hot: true },
                { icon: "🔋", title: "החלפת סוללה", desc: "חיים חדשים לטלפון ישן. סוללות מקוריות בלבד", price: "מ-149₪", hot: false },
                { icon: "🔌", title: "תיקון יציאת טעינה", desc: "כשהכבל לא נכנס — אנחנו מטפלים", price: "מ-99₪", hot: false },
                { icon: "💧", title: "נזקי מים", desc: "טיפול מקצועי בנזקי נוזלים ושחזור נתונים", price: "מ-249₪", hot: true },
                { icon: "📸", title: "תיקון מצלמה", desc: "מצלמה מטושטשת? החלפת עדשה ומודול מצלמה", price: "מ-179₪", hot: false },
                { icon: "💾", title: "שחזור נתונים", desc: "תמונות, אנשי קשר, הודעות — הכל ניתן לשחזור", price: "מ-299₪", hot: true },
                { icon: "🛡️", title: "הדבקת מגן מסך", desc: "הדבקה מקצועית ללא בועות אוויר תוך דקות", price: "מ-29₪", hot: false },
                { icon: "🔧", title: "תיקון כפתורים", desc: "כפתור הבית, עוצמה, עמעום — כל כפתור בנפרד", price: "מ-89₪", hot: false },
              ].map(s => (
                <div key={s.title} style={{
                  background: COLORS.card, border: `1px solid ${s.hot ? COLORS.accent + "55" : COLORS.border}`,
                  borderRadius: 18, padding: "24px", position: "relative",
                  boxShadow: s.hot ? `0 4px 20px ${COLORS.accentGlow}` : "none",
                }}>
                  {s.hot && <div style={{ position: "absolute", top: 14, left: 14, background: COLORS.accent, color: "#fff", borderRadius: 20, padding: "2px 10px", fontSize: 11, fontWeight: 700 }}>פופולרי 🔥</div>}
                  <div style={{ fontSize: 36, marginBottom: 12 }}>{s.icon}</div>
                  <div style={{ fontWeight: 800, fontSize: 17, marginBottom: 8 }}>{s.title}</div>
                  <div style={{ color: COLORS.muted, fontSize: 13, lineHeight: 1.6, marginBottom: 12 }}>{s.desc}</div>
                  <div style={{ color: COLORS.accent, fontWeight: 800, fontSize: 18 }}>{s.price}</div>
                </div>
              ))}
            </div>

            <div style={{
              background: "linear-gradient(135deg, #0a1a00, #1a2a00)", border: `1px solid #4caf5044`,
              borderRadius: 20, padding: "28px 32px", textAlign: "center",
            }}>
              <div style={{ fontSize: 32, marginBottom: 12 }}>⚡</div>
              <h3 style={{ fontSize: 22, fontWeight: 800, marginBottom: 8 }}>תיקון תוך שעה</h3>
              <p style={{ color: COLORS.muted, marginBottom: 20 }}>רוב התיקונים מתבצעים בזמן שאתה ממתין בחנות. אין צורך להשאיר את הטלפון!</p>
              <a href="tel:0863744444" style={{
                background: `linear-gradient(135deg, ${COLORS.accent}, ${COLORS.accentSoft})`,
                color: "#fff", borderRadius: 12, padding: "13px 28px",
                fontWeight: 700, fontSize: 15, textDecoration: "none", display: "inline-block",
              }}>📞 קבע תור עכשיו — 08-6374444</a>
            </div>
          </div>
        )}

        {/* ===== אודות ===== */}
        {page === "אודות" && (
          <div style={{ marginTop: 40, maxWidth: 860, margin: "40px auto 0" }}>
            <h1 style={{ fontSize: 36, fontWeight: 900, marginBottom: 8 }}>📖 אודות קלאב פון</h1>
            <p style={{ color: COLORS.muted, fontSize: 16, marginBottom: 40 }}>הסיפור שלנו, הערכים שלנו, ומה שמייחד אותנו</p>

            {/* Story */}
            <div style={{ background: COLORS.card, borderRadius: 20, padding: "32px", border: `1px solid ${COLORS.border}`, marginBottom: 24 }}>
              <h2 style={{ fontSize: 22, fontWeight: 800, marginBottom: 16, color: COLORS.accent }}>🏪 הסיפור שלנו</h2>
              <p style={{ color: COLORS.muted, lineHeight: 1.9, fontSize: 15 }}>
                קלאב פון אילת הוא עסק ותיק ומוכר הפועל בלב אילת, ברחוב שדרות התמרים 37. במשך שנים רבות, <strong style={{ color: COLORS.text }}>דני</strong> ואנשי הצוות שלו הפכו את החנות לאחד המקומות הכי אמינים ואהובים בעיר לרכישת מכשירי סלולר, אלקטרוניקה וציוד טכנולוגי.
              </p>
              <p style={{ color: COLORS.muted, lineHeight: 1.9, fontSize: 15, marginTop: 12 }}>
                הייתרון הגדול? <strong style={{ color: COLORS.accent }}>אנחנו נמצאים באילת — עיר חופשית מס</strong>, מה שאומר שאצלנו תחסכו <strong style={{ color: COLORS.accent }}>18% מע"מ</strong> על כל מוצר, לעומת כל חנות בשאר הארץ. זה לא סייל, זה לא מבצע — זה כל יום, כל שנה.
              </p>
            </div>

            {/* Why us */}
            <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(200px, 1fr))", gap: 16, marginBottom: 24 }}>
              {[
                { icon: "💰", title: "ללא מע\"מ", desc: "18% חיסכון על כל מוצר, כל יום" },
                { icon: "🔧", title: "מעבדה במקום", desc: "תיקון תוך שעה ברוב המקרים" },
                { icon: "🤝", title: "שירות אישי", desc: "דני וצוותו מכירים כל לקוח" },
                { icon: "📦", title: "מלאי עשיר", desc: "כל המותגים והדגמים המובילים" },
              ].map(w => (
                <div key={w.title} style={{ background: COLORS.card, borderRadius: 16, padding: "20px", border: `1px solid ${COLORS.border}`, textAlign: "center" }}>
                  <div style={{ fontSize: 32, marginBottom: 10 }}>{w.icon}</div>
                  <div style={{ fontWeight: 700, marginBottom: 6 }}>{w.title}</div>
                  <div style={{ fontSize: 13, color: COLORS.muted }}>{w.desc}</div>
                </div>
              ))}
            </div>

            {/* Hours */}
            <div style={{ background: COLORS.card, borderRadius: 20, padding: "32px", border: `1px solid ${COLORS.border}`, marginBottom: 24 }}>
              <h2 style={{ fontSize: 22, fontWeight: 800, marginBottom: 20, color: COLORS.accent }}>🕐 שעות פתיחה</h2>
              {[
                ["ראשון – חמישי", "9:30 – 20:00", true],
                ["שישי", "9:30 – 18:15", true],
                ["שבת", "סגור", false],
              ].map(([day, time, open]) => (
                <div key={day} style={{
                  display: "flex", justifyContent: "space-between", alignItems: "center",
                  padding: "13px 16px", borderRadius: 10, marginBottom: 8,
                  background: open ? `${COLORS.accent}08` : `${COLORS.border}22`,
                  border: `1px solid ${open ? COLORS.accent + "22" : COLORS.border}`,
                }}>
                  <span style={{ fontWeight: 600 }}>יום {day}</span>
                  <span style={{ fontWeight: 700, color: open ? COLORS.accent : COLORS.muted }}>{time}</span>
                  <span style={{
                    fontSize: 11, fontWeight: 700,
                    color: open ? "#4caf50" : COLORS.muted,
                    background: open ? "#4caf5018" : COLORS.border + "44",
                    borderRadius: 20, padding: "2px 10px",
                  }}>{open ? "פתוח" : "סגור"}</span>
                </div>
              ))}
              <div style={{ marginTop: 16, padding: "12px 16px", background: `${COLORS.blue}10`, border: `1px solid ${COLORS.blue}33`, borderRadius: 10, fontSize: 13, color: COLORS.muted }}>
                ℹ️ בחגים ושבתות מיוחדות ייתכנו שינויים בשעות. מומלץ לבדוק בטלפון לפני ביקור.
              </div>
            </div>

            {/* Policy */}
            <div style={{ background: COLORS.card, borderRadius: 20, padding: "32px", border: `1px solid ${COLORS.border}`, marginBottom: 24 }}>
              <h2 style={{ fontSize: 22, fontWeight: 800, marginBottom: 20, color: COLORS.accent }}>📋 תקנון ומדיניות החזרות</h2>
              {[
                { title: "החזרת מוצר", text: "ניתן להחזיר מוצר תוך 14 יום מיום הרכישה, בתנאי שהמוצר באריזתו המקורית, ללא שימוש וללא פגמים. יש לצרף חשבונית מקורית." },
                { title: "אחריות", text: "כל המוצרים מגיעים עם אחריות יבואן מלאה. מוצרי אפל — שנה. סמסונג — שנה. שאר היצרנים — שנה עד שנתיים לפי הספק." },
                { title: "תיקונים", text: "תיקונים שבוצעו במעבדה מגיעים עם אחריות של 90 יום על חלקים ועבודה. במקרה של תקלה חוזרת — תוקן ללא עלות." },
                { title: "מחיר", text: "המחירים המוצגים הינם ללא מע\"מ בהתאם לחוק אזור סחר חופשי אילת. רכישה על ידי תושבי שאר הארץ — בהתאם לתקנות." },
                { title: "ביטול עסקה", text: "ביטול עסקה תוך 14 יום בהתאם לחוק הגנת הצרכן. מוצרים שנפתחו ייגבה דמי ביטול של עד 5% מהסכום הכולל." },
              ].map(p => (
                <div key={p.title} style={{ marginBottom: 20, paddingBottom: 20, borderBottom: `1px solid ${COLORS.border}` }}>
                  <div style={{ fontWeight: 700, marginBottom: 6, color: COLORS.text }}>📌 {p.title}</div>
                  <div style={{ color: COLORS.muted, fontSize: 14, lineHeight: 1.7 }}>{p.text}</div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ===== צור קשר ===== */}
        {page === "צור קשר" && (
          <div style={{ marginTop: 40, maxWidth: 800, margin: "40px auto 0" }}>
            <h1 style={{ fontSize: 36, fontWeight: 900, marginBottom: 8 }}>📞 צור קשר</h1>
            <p style={{ color: COLORS.muted, marginBottom: 36 }}>אנחנו כאן בשבילך — צלצל, כתוב או הגיע אלינו</p>

            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 20, marginBottom: 24 }}>
              <a href="tel:0863744444" style={{
                background: "linear-gradient(135deg, #1a0800, #2a1200)",
                border: `1px solid ${COLORS.accent}44`,
                borderRadius: 20, padding: "28px", textDecoration: "none", color: COLORS.text, display: "block",
                boxShadow: `0 4px 20px ${COLORS.accentGlow}`,
              }}>
                <div style={{ fontSize: 40, marginBottom: 12 }}>📞</div>
                <div style={{ fontWeight: 800, fontSize: 18, marginBottom: 6 }}>טלפון החנות</div>
                <div style={{ fontSize: 22, fontWeight: 900, color: COLORS.accent }}>08-6374444</div>
                <div style={{ fontSize: 13, color: COLORS.muted, marginTop: 8 }}>ראשון–חמישי 9:30–20:00</div>
              </a>
              <a href="https://wa.me/9720863744444" style={{
                background: "linear-gradient(135deg, #001a08, #002a12)",
                border: "1px solid #25D36644",
                borderRadius: 20, padding: "28px", textDecoration: "none", color: COLORS.text, display: "block",
              }}>
                <div style={{ fontSize: 40, marginBottom: 12 }}>💬</div>
                <div style={{ fontWeight: 800, fontSize: 18, marginBottom: 6 }}>וואטסאפ</div>
                <div style={{ fontSize: 22, fontWeight: 900, color: "#25D366" }}>שלח הודעה</div>
                <div style={{ fontSize: 13, color: COLORS.muted, marginTop: 8 }}>תגובה מהירה!</div>
              </a>
            </div>

            {/* Location */}
            <div style={{ background: COLORS.card, borderRadius: 20, padding: "32px", border: `1px solid ${COLORS.border}`, marginBottom: 20 }}>
              <h2 style={{ fontSize: 20, fontWeight: 800, marginBottom: 20 }}>📍 איך מגיעים?</h2>
              <div style={{ display: "flex", alignItems: "flex-start", gap: 16, marginBottom: 24 }}>
                <div style={{ fontSize: 40 }}>🏪</div>
                <div>
                  <div style={{ fontWeight: 800, fontSize: 18, marginBottom: 4 }}>קלאב פון אילת</div>
                  <div style={{ color: COLORS.accent, fontSize: 16, fontWeight: 700, marginBottom: 8 }}>שדרות התמרים 37, אילת</div>
                  <div style={{ color: COLORS.muted, fontSize: 14, lineHeight: 1.8 }}>
                    📍 מרכז אילת — נגיש מכל חלקי העיר<br />
                    🚌 קרוב לתחנה המרכזית של אילת<br />
                    🏪 אזור מסחרי שוקק — בנקים, בתי קפה, משרדים<br />
                    🏨 מרחק הליכה מהמלונות המרכזיים
                  </div>
                </div>
              </div>
              <div style={{ display: "flex", gap: 12, flexWrap: "wrap" }}>
                <a href="https://waze.com/ul?q=שדרות התמרים 37 אילת" target="_blank" rel="noreferrer" style={{
                  background: "#00B4D8", color: "#fff", borderRadius: 12,
                  padding: "12px 20px", textDecoration: "none", fontWeight: 700, fontSize: 14,
                  display: "inline-flex", alignItems: "center", gap: 8,
                }}>🚗 פתח ב-Waze</a>
                <a href="https://maps.google.com/?q=שדרות התמרים 37 אילת" target="_blank" rel="noreferrer" style={{
                  background: "#4285F4", color: "#fff", borderRadius: 12,
                  padding: "12px 20px", textDecoration: "none", fontWeight: 700, fontSize: 14,
                  display: "inline-flex", alignItems: "center", gap: 8,
                }}>🗺️ פתח ב-Google Maps</a>
              </div>
            </div>

            {/* Social */}
            <div style={{ background: COLORS.card, borderRadius: 20, padding: "28px", border: `1px solid ${COLORS.border}` }}>
              <h2 style={{ fontSize: 20, fontWeight: 800, marginBottom: 16 }}>🌐 עקבו אחרינו</h2>
              <div style={{ display: "flex", gap: 14, flexWrap: "wrap" }}>
                {[
                  { name: "Facebook", icon: "📘", color: "#1877F2", link: "#" },
                  { name: "Instagram", icon: "📸", color: "#E1306C", link: "#" },
                  { name: "WhatsApp", icon: "💬", color: "#25D366", link: "https://wa.me/9720863744444" },
                ].map(s => (
                  <a key={s.name} href={s.link} target="_blank" rel="noreferrer" style={{
                    background: s.color + "18", color: s.color,
                    border: `1px solid ${s.color}44`, borderRadius: 12,
                    padding: "12px 20px", textDecoration: "none", fontWeight: 700, fontSize: 14,
                    display: "inline-flex", alignItems: "center", gap: 8,
                  }}>{s.icon} {s.name}</a>
                ))}
              </div>
            </div>
          </div>
        )}
      </main>

      {/* FOOTER */}
      <footer style={{
        borderTop: `1px solid ${COLORS.border}`,
        background: COLORS.surface,
        padding: "40px 24px 24px",
      }}>
        <div style={{ maxWidth: 1200, margin: "0 auto" }}>
          <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(200px, 1fr))", gap: 32, marginBottom: 32 }}>
            <div>
              <div style={{ fontWeight: 900, fontSize: 20, marginBottom: 12 }}>
                <span style={{ color: COLORS.accent }}>Club</span>Phone
              </div>
              <div style={{ color: COLORS.muted, fontSize: 13, lineHeight: 1.8 }}>
                שדרות התמרים 37, אילת<br />
                08-6374444<br />
                ללא מע"מ • כל השנה
              </div>
            </div>
            <div>
              <div style={{ fontWeight: 700, marginBottom: 12 }}>קישורים מהירים</div>
              {nav.map(n => (
                <div key={n} onClick={() => go(n)} style={{ color: COLORS.muted, fontSize: 13, marginBottom: 8, cursor: "pointer", transition: "color 0.2s" }}
                  onMouseEnter={e => e.target.style.color = COLORS.accent}
                  onMouseLeave={e => e.target.style.color = COLORS.muted}>{n}</div>
              ))}
            </div>
            <div>
              <div style={{ fontWeight: 700, marginBottom: 12 }}>שעות פתיחה</div>
              <div style={{ color: COLORS.muted, fontSize: 13, lineHeight: 2 }}>
                ראשון–חמישי: 9:30–20:00<br />
                שישי: 9:30–18:15<br />
                שבת: סגור
              </div>
            </div>
            <div>
              <div style={{ fontWeight: 700, marginBottom: 12 }}>צור קשר</div>
              <div style={{ display: "flex", flexDirection: "column", gap: 8 }}>
                <a href="tel:0863744444" style={{ color: COLORS.accent, fontSize: 13, textDecoration: "none" }}>📞 08-6374444</a>
                <a href="https://wa.me/9720863744444" style={{ color: "#25D366", fontSize: 13, textDecoration: "none" }}>💬 WhatsApp</a>
                <a href="https://waze.com/ul?q=שדרות התמרים 37 אילת" target="_blank" rel="noreferrer" style={{ color: COLORS.blue, fontSize: 13, textDecoration: "none" }}>🚗 Waze</a>
                <a href="https://maps.google.com/?q=שדרות התמרים 37 אילת" target="_blank" rel="noreferrer" style={{ color: "#4285F4", fontSize: 13, textDecoration: "none" }}>🗺️ Google Maps</a>
              </div>
            </div>
          </div>
          <div style={{ borderTop: `1px solid ${COLORS.border}`, paddingTop: 20, textAlign: "center", color: COLORS.muted, fontSize: 12 }}>
            © 2025 ClubPhone אילת • שדרות התמרים 37 • ללא מע"מ • כל הזכויות שמורות
          </div>
        </div>
      </footer>
    </div>
  );
}