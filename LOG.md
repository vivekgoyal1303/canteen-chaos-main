# Debug log

9 bugs fix kiye. Har ek ko pehle reproduce kiya fir fix kiya.

### CC-01 — "search suggestions behind everything"

**Reproduced:** Search me "momo" type kiya laptop pe. List aayi par click nahi ho raha tha. Sirf upar ka thoda part click hota tha, baki pe click bekar tha.

**Cause:** suggest-box ka z-index kam tha. Neeche wala menu uske upar aa raha tha, isliye clicks menu pe ja rahe the.

**Fix:** frontend/style.css me suggest-box ko z-index 100 diya, search-wrap ko 50, aur #menu ko 2 pe rakha. Taaki suggestion sabse upar rahe.

**Checked:** Fir se type karke har suggestion pe click kiya. Dish modal khul raha hai ab. Menu pe click block nahi kar raha.

**Time:** 25 min, z-index dhundne me time gaya.

### CC-02 — "dark mode me kuch dikh nahi raha"

**Reproduced:** Dark mode on kiya. Dish ka naam aur price gayab jaise ho gaye. Neeche wali grey line dikh rahi thi bas.

**Cause:** dish-body / name-price ka color dark bg me mix ho raha tha. Ink color use nahi ho raha tha.

**Fix:** style.css me dish-body ko color var(--ink) diya. Name aur price ab ink se lete hai to dark me light dikhte hai, light me dark.

**Checked:** Dark aur light dono me menu khol ke dekha. Naam price dono clear hai ab.

**Time:** 20 min.

### CC-03 — "menu phone se bahar ja raha"

**Reproduced:** F12 me width 500px ki. Sideways scroll aa raha tha. Right wale card ka Add to Cart kata hua tha. 760 pe dikkat nahi thi, 480-550 me thi.

**Cause:** 760 wale media me name-btn ko width max-content diya tha. Lamba naam wrap nahi hota tha aur 1fr column ko dhakka deke grid ko bahar nikal deta tha.

**Fix:** style.css me menu-grid ko repeat(2, minmax(0,1fr)) kiya. Isse column shrink ho sakta hai aur naam andar wrap hota hai.

**Checked:** 550 se 480 tak drag karke dekha. Ab sideways scroll nahi aata. Add to Cart pura dikhta hai.

**Time:** 40 min, breakpoint samajhne me time laga.

### CC-04 — "tablet pe button kaam nahi karte"

**Reproduced:** Width 800px karke dekha (tablet). Add to Cart aur star pe click karo to kuch nahi hota. Laptop aur phone pe sahi tha.

**Cause:** 761-900 wale media me dish-card::after aur img-wrap::after ka overlay buttons ke upar aa raha tha. Click uspe atak raha tha.

**Fix:** style.css me us overlay ko pointer-events none kiya aur transparent rakha. Taaki click neeche button tak jaye.

**Checked:** Fir se 800px pe click karke dekha. Add hua cart me, star on/off hua.

**Time:** 30 min.

### CC-06 — "stock se zyada order ho gaya"

**Reproduced:** Samosa ka stock 2 tha. Fir bhi qty 5 ka order kar diya. Order place ho gaya. Kitchen pe gaye to 2 hi the.

**Cause:** validation.js me reserveStock me stock check tha hi nahi. Seedha stock minus kar deta tha.

**Fix:** backend/logic/validation.js me check lagaya - if dish.stock < item.qty to error "Only X left". Ab zyada mango to conflict aata hai.

**Checked:** Order karke verify kiya. 5 mangne pe error aaya. 2 mangne pe order hua. Menu API me stock bhi sahi ghat ta hai.

**Time:** 30 min.

### CC-07 — "cancel pe stock aur kam ho gaya"

**Reproduced:** Ek order kiya, fir usko cancel kiya. Menu me stock wapas badhna chahiye tha par aur kam ho gaya. 2-3 baar karo to zero ho jata.

**Cause:** Cancel wale code me stock wapas add nahi ho raha tha, ulta minus ho raha tha.

**Fix:** backend/routes/orders.routes.js me cancel pe releaseStock lagaya jo stock + qty karta hai. Points aur coupon bhi wapas hote hai.

**Checked:** Order karke cancel karke verify kiya. Pehle menu API se stock dekha, fir order ke baad dekha, fir cancel ke baad dekha. Stock wapas aa gaya.

**Time:** 35 min.

### CC-08 — "purana coupon abhi bhi chal raha"

**Reproduced:** FRESHERS24 jo 2024 me expire ho gaya tha usse quote mara. Fir bhi discount mil gaya.

**Cause:** pricing.js me applyCoupon me expiry check nahi tha. Sirf code match karta tha.

**Fix:** backend/logic/pricing.js me expiresAt check lagaya. Agar expiry <= now to "has expired" error. Ab purana coupon reject hota hai.

**Checked:** Quote aur order dono karke verify kiya. FRESHERS24 pe error aata hai. BYTE10 pe discount milta hai.

**Time:** 20 min.

### CC-09 — "menu ek saath bahut saare dishes dikha raha"

**Reproduced:** Menu khola to ek saath lambi list aa gayi. Load more ka matlab hi nahi raha. Thode-thode karke aana chahiye tha.

**Cause:** backend/routes/menu.routes.js me paginate ho raha tha par response me puri list ja rahi thi, page wale items nahi.

**Fix:** list ki jagah items kiya - dishes: paged.items bheja. Ab pehle 5 aate hai, scroll / Load more pe next page aata hai.

**Checked:** Site khol ke dekha, ab 5-5 karke aa rahe hai. Scroll pe aur load ho rahe hai, pehle jaise lambi list nahi aa rahi.

**Time:** 25 min.

### CC-10 — "price sort ulta hai"

**Reproduced:** Sort me "price low to high" select kiya to mehengi dish pehle aayi. High to low pe sasti pehle aayi. Dono ulta tha.

**Cause:** backend/logic/search.js me SORTERS ulta likha tha.

**Fix:** price-asc ko a.price - b.price kiya aur price-desc ko b.price - a.price kiya.

**Checked:** API hit karke verify kiya. /menu?sort=price-asc pe 16.5, 21, 23... order me aaye. Desc pe ulta sahi aaya. Site pe dropdown se bhi check kiya.

**Time:** 15 min.


