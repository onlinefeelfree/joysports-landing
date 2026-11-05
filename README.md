# Joy Sports — แลนดิ้งเพจของพรีเมียมติดโลโก้

หน้าเว็บหน้าเดียวสำหรับแคมเปญ ChatGPT Ads ของ **บริษัท จอย สปอร์ต จำกัด** (เจ้าของแบรนด์ FEELFREE)
กลุ่มเป้าหมาย: ลูกค้าองค์กรที่ต้องจัดหาของพรีเมียมติดโลโก้ และนายหน้า/ตัวแทนที่รับงานของพรีเมียมมาแล้วต้องหาโรงงานผลิต

เว็บสแตติกล้วน ไม่มี build step — Vercel เสิร์ฟไฟล์ตรงจาก repo นี้ได้เลย

```
index.html     หน้าเว็บทั้งหน้า (CSS + JS อยู่ในไฟล์เดียว)
images/        รูปสินค้า 15 ไฟล์
vercel.json    cache header ของรูป
```

---

## ⚠️ ต้องเติม 2 ค่าก่อนเปิดแอด

เปิด `index.html` เลื่อนไปท้ายไฟล์ หาบล็อก `CONFIG`

```js
var CONFIG = {
  pixelId:   "",              // ← Pixel ID จาก ads.openai.com → Settings → Conversions
  lineOaId:  "",              // ← LINE OA id ไม่ต้องใส่ @ (ปล่อยว่าง = ซ่อนแถว LINE ทั้งแถว)
  leadEvent: "lead_created"
};
```

ถ้า `pixelId` ว่าง หน้าเว็บจะ **ไม่ยิง conversion เลย** แอดจะวัดผลไม่ได้

---

## Conversion tracking

พิกเซล OpenAI ฝังในโค้ดโดยตรง ไม่ผ่าน GTM

| พฤติกรรมของคนเข้าเว็บ | Event |
|---|---|
| เปิดหน้า | `page_viewed` |
| กดปุ่มทัก LINE | `lead_created` (`lead_<pageview>_line`) |
| กดเบอร์โทร | `lead_created` (`…_phone` / `…_phone2`) |
| กดลิงก์ Messenger | `lead_created` (`…_messenger`) |
| กดปุ่มคัดลอกเบอร์ | `lead_created` (`…_phone_copy`) |
| กดปุ่ม "ขอใบเสนอราคา" บนหัวเว็บ | ไม่ยิง — แค่เลื่อนหน้า ยังไม่ใช่ลีด |

`event_id` ผูกกับการเปิดหน้าหนึ่งครั้ง + ช่องทาง → กดซ้ำไม่นับเพิ่ม

**ห้ามใส่ฟอร์มคัดกรองก่อนถึงช่องทางติดต่อ** — เคสจริงของติดจรวด ใส่แล้ว conversion ตกจาก 13/วัน เหลือ 0

---

## UTM ที่ตั้งระดับแคมเปญ

```
?utm_source=chatgpt&utm_medium=cpc&utm_campaign=joysports_corporate&seg=corporate
?utm_source=chatgpt&utm_medium=cpc&utm_campaign=joysports_agent&seg=agent
?utm_source=chatgpt&utm_medium=cpc&utm_campaign=joysports_event&seg=event
```

`seg` สลับหัวข้อ คำโปรย และป้ายบน hero ให้ตรงกับแอดที่คนคลิกมา และติดไปกับข้อความทัก LINE ด้วย
ค่าที่รองรับ: `corporate` · `agent` · `event` · `brand` · `export` (ไม่ใส่ = หัวข้อกลางของกลุ่มองค์กร)

---

## หมายเหตุสำหรับคนแก้โค้ด

กฎ `img` ในไฟล์มี `height:auto` พร้อมคอมเมนต์เตือน — **อย่าเอาออก**
แท็ก `<img>` ทุกตัวมี attribute `height` เพื่อกันหน้ากระตุกตอนโหลด ซึ่งจะชนะ `aspect-ratio` ใน CSS
ถ้าลบ `height:auto` รูปทุกใบจะถูกยืดสูงแล้ว `object-fit:cover` จะซูมตัดเหลือแค่กลางภาพ

---

ไฟล์ต้นทาง บรีฟธุรกิจ และแผนแคมเปญ เก็บแยกไว้ในเครื่อง ไม่ได้อยู่ใน repo นี้
