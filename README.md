<!DOCTYPE html>
<html lang="ar" dir="rtl" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ميدتاون | Midtown Restaurant & Cafe</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Cairo -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800;900&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        cairo: ['Cairo', 'sans-serif'],
                    },
                    colors: {
                        darkbg: '#0B0C10',
                        cardbg: '#14161D',
                        gold: '#D4AF37',
                        goldlight: '#F3E5AB',
                        emerald: '#10B981',
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #0B0C10;
            color: #F8F9FA;
        }
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        .gold-gradient {
            background: linear-gradient(135deg, #D4AF37 0%, #AA7C11 100%);
        }
    </style>
</head>
<body class="pb-28">

    <!-- شريط تجربة لوحة التحكم لصاحب المطعم -->
    <div class="bg-gradient-to-r from-amber-700 via-yellow-600 to-amber-700 text-black px-4 py-2.5 text-xs md:text-sm flex justify-between items-center sticky top-0 z-50 shadow-lg">
        <div class="flex items-center gap-2 font-extrabold">
            <span class="bg-black text-gold px-2 py-0.5 rounded animate-pulse">نسخة العرض للإدارة</span>
            <span>جرب تعديل الأسعار أو إضافة أطباق لميدتاون فوراً!</span>
        </div>
        <button onclick="toggleAdminModal()" class="bg-black text-gold font-extrabold px-3 py-1.5 rounded-lg shadow hover:bg-gray-900 transition flex items-center gap-1.5 border border-gold/40">
            <i class="fas fa-sliders-h"></i> لوحة التحكم
        </button>
    </div>

    <!-- الهيدر والغلاف الفاخر -->
    <header class="relative bg-cardbg border-b border-gray-800/80 pb-6">
        <div class="h-48 md:h-64 bg-cover bg-center relative" style="background-image: url('https://images.unsplash.com/photo-1517248135467-4c7edcad34c4?auto=format&fit=crop&w=1200&q=80');">
            <div class="absolute inset-0 bg-gradient-to-t from-darkbg via-darkbg/70 to-black/30"></div>
        </div>
        
        <div class="max-w-4xl mx-auto px-4 -mt-16 relative z-10 flex flex-col md:flex-row items-center md:items-end justify-between gap-4 text-center md:text-right">
            <div class="flex flex-col md:flex-row items-center gap-4">
                <div class="w-24 h-24 rounded-2xl gold-gradient p-0.5 shadow-2xl shadow-gold/20">
                    <div class="w-full h-full bg-darkbg rounded-2xl flex flex-col items-center justify-center">
                        <span class="text-xl font-black tracking-wider text-gold">MIDTOWN</span>
                        <span class="text-[10px] text-gray-400 font-bold -mt-1">ميدتاون</span>
                    </div>
                </div>
                <div>
                    <div class="flex items-center justify-center md:justify-start gap-2">
                        <h1 class="text-3xl font-black text-white">مطعم وكافيه ميدتاون</h1>
                        <span class="text-xs bg-emerald/20 text-emerald border border-emerald/30 px-2.5 py-0.5 rounded-full font-bold">مفتوح الآن</span>
                    </div>
                    <p class="text-gray-400 text-sm mt-1">تجربة استثنائية من الأطباق العالمية، الستيك، القهوة المختصة والحلويات ✨</p>
                </div>
            </div>
            
            <div class="flex gap-2">
                <a href="https://www.facebook.com/share/1M3aphqiWB/" target="_blank" class="bg-blue-600/15 text-blue-400 border border-blue-500/30 hover:bg-blue-600 hover:text-white px-4 py-2.5 rounded-xl text-sm font-bold transition flex items-center gap-2">
                    <i class="fab fa-facebook-f"></i> فيسبوك
                </a>
                <button onclick="openCart()" class="gold-gradient text-black font-black px-4 py-2.5 rounded-xl text-sm shadow-lg shadow-gold/20 flex items-center gap-2">
                    <i class="fas fa-utensils"></i> طلبي (<span id="header-cart-count">0</span>)
                </button>
            </div>
        </div>
    </header>

    <!-- شريط البحث والأقسام -->
    <section class="max-w-4xl mx-auto px-4 mt-6 sticky top-11 z-30 bg-darkbg/95 backdrop-blur py-3 border-b border-gray-800/80">
        <div class="relative mb-3">
            <input type="text" id="search-input" oninput="renderMenu()" placeholder="ابحث في منيو ميدتاون (ستيك، فيتوتشيني، برغر، كريب، موهيتو...)" class="w-full bg-cardbg border border-gray-800 rounded-xl py-2.5 pr-10 pl-4 text-sm text-white placeholder-gray-500 focus:outline-none focus:border-gold">
            <i class="fas fa-search absolute right-3.5 top-3 text-gold"></i>
        </div>

        <div class="flex gap-2 overflow-x-auto no-scrollbar pb-1" id="categories-container">
            <!-- يتم توليد الأقسام بالجافاسكربت -->
        </div>
    </section>

    <!-- شبكة عرض الوجبات -->
    <main class="max-w-4xl mx-auto px-4 mt-6">
        <div id="menu-grid" class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <!-- يتم توليد الوجبات بالجافاسكربت -->
        </div>
    </main>

    <!-- الشريط السفلي للسلة -->
    <div id="bottom-cart-bar" class="fixed bottom-0 left-0 right-0 bg-cardbg/95 backdrop-blur-md border-t border-gold/30 p-4 z-40 transform translate-y-full transition-transform duration-300 shadow-2xl">
        <div class="max-w-4xl mx-auto flex justify-between items-center">
            <div>
                <span class="text-xs text-gray-400 block">إجمالي الفاتورة</span>
                <span class="text-xl font-black text-gold"><span id="bottom-cart-total">0</span> د.ع</span>
            </div>
            <button onclick="openCart()" class="gold-gradient text-black font-black px-6 py-3 rounded-xl shadow-lg flex items-center gap-2 hover:opacity-95 transition">
                <span>تأكيد وإرسال الطلب</span>
                <i class="fab fa-whatsapp text-lg"></i>
            </button>
        </div>
    </div>

    <!-- نافذة سلة الطلبات وإرسال الواتساب -->
    <div id="cart-modal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-cardbg border border-gray-800 rounded-2xl max-w-md w-full max-h-[90vh] flex flex-col overflow-hidden shadow-2xl">
            <div class="p-4 border-b border-gray-800 flex justify-between items-center">
                <h3 class="text-lg font-extrabold flex items-center gap-2 text-gold">
                    <i class="fas fa-concierge-bell"></i> فاتورة طلبك - ميدتاون
                </h3>
                <button onclick="closeCart()" class="text-gray-400 hover:text-white text-xl">&times;</button>
            </div>
            
            <div id="cart-items-list" class="p-4 overflow-y-auto space-y-3 flex-1">
                <!-- عناصر السلة -->
            </div>

            <div class="p-4 bg-darkbg/80 border-t border-gray-800 space-y-3">
                <div class="flex justify-between text-base font-extrabold">
                    <span>المبلغ الإجمالي:</span>
                    <span class="text-gold"><span id="modal-cart-total">0</span> د.ع</span>
                </div>

                <!-- خيار نوع الطلب: داخل المطعم أو توصيل -->
                <div class="grid grid-cols-2 gap-2 text-xs font-bold">
                    <button type="button" onclick="setOrderType('توصيل منزلي')" id="btn-delivery" class="py-2 rounded-xl border border-gold bg-gold/20 text-gold">🛵 توصيل منزلي</button>
                    <button type="button" onclick="setOrderType('داخل المطعم (طاولة)')" id="btn-dinein" class="py-2 rounded-xl border border-gray-700 bg-cardbg text-gray-400">🍽️ داخل المطعم</button>
                </div>

                <input type="text" id="cust-name" placeholder="الاسم الكريم" class="w-full bg-cardbg border border-gray-700 rounded-xl p-2.5 text-sm text-white focus:outline-none focus:border-gold">
                <input type="text" id="cust-address" placeholder="العنوان الكامل (أو رقم الطاولة إذا كنت داخل المطعم)" class="w-full bg-cardbg border border-gray-700 rounded-xl p-2.5 text-sm text-white focus:outline-none focus:border-gold">
                <textarea id="cust-notes" placeholder="ملاحظات الشيف (درجة استواء الستيك، بدون بصل، صوص إضافي...)" rows="2" class="w-full bg-cardbg border border-gray-700 rounded-xl p-2.5 text-sm text-white focus:outline-none focus:border-gold"></textarea>
                
                <div class="bg-gold/10 border border-gold/30 rounded-xl p-2.5 text-xs text-goldlight">
                    <label class="block font-bold mb-1">📱 رقم واتساب استلام الطلبات (للتجربة ضع رقمك):</label>
                    <input type="text" id="whatsapp-number" value="9647700000000" class="w-full bg-darkbg border border-gray-700 rounded-lg p-1.5 text-left dir-ltr text-white font-mono">
                </div>

                <button onclick="sendOrderToWhatsApp()" class="w-full bg-emerald hover:bg-green-600 text-black font-black py-3 rounded-xl shadow-lg flex items-center justify-center gap-2 transition">
                    <i class="fab fa-whatsapp text-xl"></i> إرسال الطلب عبر واتساب الآن
                </button>
            </div>
        </div>
    </div>

    <!-- نافذة لوحة التحكم التجريبية لصاحب المطعم -->
    <div id="admin-modal" class="fixed inset-0 bg-black/85 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-cardbg border border-gold/40 rounded-2xl max-w-2xl w-full max-h-[90vh] flex flex-col overflow-hidden shadow-2xl">
            <div class="p-4 bg-gradient-to-r from-gray-900 to-black border-b border-gray-800 flex justify-between items-center">
                <div>
                    <h3 class="text-lg font-black text-gold flex items-center gap-2">
                        <i class="fas fa-crown"></i> لوحة تحكم إدارة "ميدتاون"
                    </h3>
                    <p class="text-xs text-gray-400">تحكم كامل بالأسعار والأطباق لحظياً دون الحاجة لمبرمج</p>
                </div>
                <button onclick="toggleAdminModal()" class="text-gray-400 hover:text-white text-2xl">&times;</button>
            </div>

            <div class="p-4 overflow-y-auto space-y-6 flex-1">
                <!-- إضافة طبق جديد -->
                <div class="bg-darkbg p-4 rounded-xl border border-gray-800">
                    <h4 class="font-bold text-sm text-gold mb-3">+ إضافة طبق أو مشروب جديد لمنيو ميدتاون</h4>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-3">
                        <input type="text" id="new-item-name" placeholder="اسم الطبق (مثال: سلمون مشوي)" class="bg-cardbg border border-gray-700 rounded-lg p-2 text-sm text-white">
                        <input type="number" id="new-item-price" placeholder="السعر بالدينار (مثال: 18000)" class="bg-cardbg border border-gray-700 rounded-lg p-2 text-sm text-white">
                        <select id="new-item-cat" class="bg-cardbg border border-gray-700 rounded-lg p-2 text-sm text-white">
                            <option value="الستيك والأطباق الرئيسية">الستيك والأطباق الرئيسية</option>
                            <option value="الباستا والبيتزا الإيطالية">الباستا والبيتزا الإيطالية</option>
                            <option value="البرغر والسندويشات">البرغر والسندويشات</option>
                            <option value="الحلويات والكريب">الحلويات والكريب</option>
                            <option value="القهوة والمشروبات">القهوة والمشروبات</option>
                        </select>
                    </div>
                    <input type="text" id="new-item-desc" placeholder="وصف الطبق والمكونات..." class="w-full mt-2 bg-cardbg border border-gray-700 rounded-lg p-2 text-sm text-white">
                    <button onclick="addNewMenuItem()" class="mt-3 gold-gradient text-black font-extrabold px-4 py-2 rounded-lg text-sm hover:opacity-90 transition">
                        حفظ وإضافة للمنيو فوراً
                    </button>
                </div>

                <!-- قائمة تعديل الأسعار والتوفر -->
                <div>
                    <h4 class="font-bold text-sm text-gray-300 mb-2">تعديل الأسعار أو حالة التوفر (متوفر / نفذت الكمية):</h4>
                    <div id="admin-items-list" class="space-y-2">
                        <!-- يتم توليدها بالجافاسكربت -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const categories = [
            "الكل",
            "الستيك والأطباق الرئيسية",
            "الباستا والبيتزا الإيطالية",
            "البرغر والسندويشات",
            "الحلويات والكريب",
            "القهوة والمشروبات"
        ];
        let activeCategory = "الكل";
        let orderType = "توصيل منزلي";

        let menuItems = [
            {
                id: 1,
                name: "ريب آي ستيك مشوي (Ribeye Steak)",
                category: "الستيك والأطباق الرئيسية",
                price: 22000,
                desc: "قطعة ستيك بقري فاخرة مشوية على اللهب، تقدم مع خضار سوتيه، بطاطا مهروسة وصوص المشروم بالكريمة",
                image: "https://images.unsplash.com/photo-1600891964092-4316c288032e?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: "توقيع الشيف 👨‍🍳"
            },
            {
                id: 2,
                name: "تشيكن ستراغونوف بالكريمة",
                category: "الستيك والأطباق الرئيسية",
                price: 13000,
                desc: "شرائح صدور دجاج طرية مطهوة بصوص الكريمة الغنية والفطر الطازج، تقدم مع الأرز الأبيض المبهر",
                image: "https://images.unsplash.com/photo-1604908176997-125f25cc6f3d?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: "الأكثر طلباً 🔥"
            },
            {
                id: 3,
                name: "فيتوتشيني ألفريدو بالدجاج",
                category: "الباستا والبيتزا الإيطالية",
                price: 10500,
                desc: "مكرونة فيتوتشيني إيطالية غارقة بصوص الكريمة وجبن البارميزان الأصلي مع قطع الدجاج المشوي",
                image: "https://images.unsplash.com/photo-1645112411341-6c4fd023714a?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: ""
            },
            {
                id: 4,
                name: "بيتزا ميدتاون سوبريم",
                category: "الباستا والبيتزا الإيطالية",
                price: 12000,
                desc: "عجينة إيطالية هشة، صوص طماطم طبيعي، جبن موزاريلا، ببروني، خضار طازجة وزيتون أسود",
                image: "https://images.unsplash.com/photo-1513104890138-7c749659a591?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: "عائلية"
            },
            {
                id: 5,
                name: "برغر ميدتاون الملكي (ترافل برغر)",
                category: "البرغر والسندويشات",
                price: 8500,
                desc: "شريحة لحم أنغوس صافي 200 غم، جبن سويسري ذائب، فطر مكرمل وصوص الترافل الفاخر مع البطاطا",
                image: "https://images.unsplash.com/photo-1568901346375-23c9450c58cd?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: "طعم استثنائي ⭐"
            },
            {
                id: 6,
                name: "كلوب سندويش ميدتاون",
                category: "البرغر والسندويشات",
                price: 7500,
                desc: "ثلاث طبقات من التوست المحمص مع الدجاج المشوي، التركي المدخن، البيض، الجبن والخس مع الفرايز",
                image: "https://images.unsplash.com/photo-1528735602780-2552fd46c7af?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: ""
            },
            {
                id: 7,
                name: "فيتوتشيني كريب شوكولاتة بلجيكية",
                category: "الحلويات والكريب",
                price: 7000,
                desc: "شرائح كريب مقطعة على طريقة الفيتوتشيني ومغرقة بالشوكولاتة البلجيكية الفاخرة مع الفواكه والآيس كريم",
                image: "https://images.unsplash.com/photo-1519676867240-f03562e64548?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: "قسم الكافيه 🍫"
            },
            {
                id: 8,
                name: "موهيتو بلو أوشن منعش",
                category: "القهوة والمشروبات",
                price: 4500,
                desc: "مزيج منعش من الليمون الطازج، أوراق النعناع، نكهة التوت الأزرق والثلج المجروش",
                image: "https://images.unsplash.com/photo-1513558161293-cdaf765ed2fd?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: "منعش ❄️"
            },
            {
                id: 9,
                name: "سبانيش لاتيه (حار / بارد)",
                category: "القهوة والمشروبات",
                price: 4500,
                desc: "دبل شوت إسبريسو من البن المختص مع الحليب المكثف المحلى والرغوة الكريمية",
                image: "https://images.unsplash.com/photo-1461023058943-07fcbe16d735?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: ""
            }
        ];

        let cart = {};

        function renderCategories() {
            const container = document.getElementById('categories-container');
            container.innerHTML = categories.map(cat => `
                <button onclick="setCategory('${cat}')" class="whitespace-nowrap px-4 py-2 rounded-xl text-sm font-bold transition ${activeCategory === cat ? 'gold-gradient text-black shadow-lg shadow-gold/20 font-black' : 'bg-cardbg text-gray-300 border border-gray-800 hover:border-gold/50'}">
                    ${cat}
                </button>
            `).join('');
        }

        function setCategory(cat) {
            activeCategory = cat;
            renderCategories();
            renderMenu();
        }

        function setOrderType(type) {
            orderType = type;
            const btnDelivery = document.getElementById('btn-delivery');
            const btnDinein = document.getElementById('btn-dinein');
            if (type === 'توصيل منزلي') {
                btnDelivery.className = "py-2 rounded-xl border border-gold bg-gold/20 text-gold";
                btnDinein.className = "py-2 rounded-xl border border-gray-700 bg-cardbg text-gray-400";
                document.getElementById('cust-address').placeholder = "العنوان الكامل بالتفصيل...";
            } else {
                btnDinein.className = "py-2 rounded-xl border border-gold bg-gold/20 text-gold";
                btnDelivery.className = "py-2 rounded-xl border border-gray-700 bg-cardbg text-gray-400";
                document.getElementById('cust-address').placeholder = "رقم الطاولة (مثال: طاولة رقم 5)";
            }
        }

        function renderMenu() {
            const search = document.getElementById('search-input').value.toLowerCase();
            const grid = document.getElementById('menu-grid');

            const filtered = menuItems.filter(item => {
                const matchCat = activeCategory === "الكل" || item.category === activeCategory;
                const matchSearch = item.name.toLowerCase().includes(search) || item.desc.toLowerCase().includes(search);
                return matchCat && matchSearch;
            });

            grid.innerHTML = filtered.map(item => `
                <div class="bg-cardbg border border-gray-800/90 rounded-2xl p-3.5 flex gap-3.5 items-center relative overflow-hidden transition hover:border-gold/50 ${!item.available ? 'opacity-50' : ''}">
                    ${item.badge ? `<span class="absolute top-2 right-2 bg-black/80 text-gold border border-gold/40 text-[10px] font-extrabold px-2 py-0.5 rounded-md shadow z-10">${item.badge}</span>` : ''}
                    
                    <img src="${item.image}" alt="${item.name}" class="w-24 h-24 rounded-xl object-cover flex-shrink-0 border border-gray-800">
                    
                    <div class="flex-1 min-w-0">
                        <h3 class="font-extrabold text-white text-base truncate">${item.name}</h3>
                        <p class="text-gray-400 text-xs line-clamp-2 mt-1 leading-relaxed">${item.desc}</p>
                        
                        <div class="flex justify-between items-center mt-3">
                            <span class="text-gold font-black text-base">${item.price.toLocaleString()} <small class="text-xs">د.ع</small></span>
                            
                            ${item.available ? `
                                <button onclick="addToCart(${item.id})" class="bg-gold/15 hover:bg-gold text-gold hover:text-black border border-gold/30 font-extrabold px-3.5 py-1.5 rounded-xl text-xs transition flex items-center gap-1.5">
                                    <i class="fas fa-plus"></i> أضف للطلب
                                </button>
                            ` : `
                                <span class="bg-red-500/20 text-red-400 text-xs font-bold px-2.5 py-1 rounded-lg">غير متوفر حالياً</span>
                            `}
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function addToCart(id) {
            cart[id] = (cart[id] || 0) + 1;
            updateCartUI();
        }

        function changeQty(id, delta) {
            if (!cart[id]) return;
            cart[id] += delta;
            if (cart[id] <= 0) delete cart[id];
            updateCartUI();
        }

        function updateCartUI() {
            let totalCount = 0;
            let totalPrice = 0;

            for (const [id, qty] of Object.entries(cart)) {
                const item = menuItems.find(i => i.id == id);
                if (item) {
                    totalCount += qty;
                    totalPrice += item.price * qty;
                }
            }

            document.getElementById('header-cart-count').innerText = totalCount;
            document.getElementById('bottom-cart-total').innerText = totalPrice.toLocaleString();
            document.getElementById('modal-cart-total').innerText = totalPrice.toLocaleString();

            const bottomBar = document.getElementById('bottom-cart-bar');
            if (totalCount > 0) {
                bottomBar.classList.remove('translate-y-full');
            } else {
                bottomBar.classList.add('translate-y-full');
                closeCart();
            }

            renderCartModalItems();
        }

        function renderCartModalItems() {
            const container = document.getElementById('cart-items-list');
            const entries = Object.entries(cart);
            if (entries.length === 0) {
                container.innerHTML = `<p class="text-center text-gray-500 py-6">قائمة طلباتك فارغة</p>`;
                return;
            }

            container.innerHTML = entries.map(([id, qty]) => {
                const item = menuItems.find(i => i.id == id);
                if (!item) return '';
                return `
                    <div class="flex justify-between items-center bg-darkbg p-3 rounded-xl border border-gray-800">
                        <div>
                            <h4 class="font-bold text-sm text-white">${item.name}</h4>
                            <span class="text-xs text-gold font-bold">${(item.price * qty).toLocaleString()} د.ع</span>
                        </div>
                        <div class="flex items-center gap-2">
                            <button onclick="changeQty(${item.id}, -1)" class="w-7 h-7 rounded-lg bg-cardbg border border-gray-700 flex items-center justify-center font-bold">-</button>
                            <span class="font-bold text-sm w-5 text-center">${qty}</span>
                            <button onclick="changeQty(${item.id}, 1)" class="w-7 h-7 rounded-lg gold-gradient text-black flex items-center justify-center font-bold">+</button>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function openCart() {
            if (Object.keys(cart).length === 0) {
                alert("يرجى اختيار طبق أو مشروب أولاً!");
                return;
            }
            document.getElementById('cart-modal').classList.remove('hidden');
        }

        function closeCart() {
            document.getElementById('cart-modal').classList.add('hidden');
        }

        function sendOrderToWhatsApp() {
            const name = document.getElementById('cust-name').value || "زبون ميدتاون";
            const address = document.getElementById('cust-address').value || "لم يحدد";
            const notes = document.getElementById('cust-notes').value || "لا توجد";
            const phone = document.getElementById('whatsapp-number').value.replace(/[^0-9]/g, '');

            let total = 0;
            let lines = [`*طلب جديد - مطعم وكافيه ميدتاون (Midtown)* ✨🍽️`, `------------------------`];
            for (const [id, qty] of Object.entries(cart)) {
                const item = menuItems.find(i => i.id == id);
                if (item) {
                    const sub = item.price * qty;
                    total += sub;
                    lines.push(`• ${item.name} (العدد: ${qty}) = ${sub.toLocaleString()} د.ع`);
                }
            }
            lines.push(`------------------------`);
            lines.push(`*المجموع الكلي:* ${total.toLocaleString()} د.ع`);
            lines.push(`*نوع الطلب:* ${orderType}`);
            lines.push(`*الاسم:* ${name}`);
            lines.push(`*العنوان / الطاولة:* ${address}`);
            lines.push(`*ملاحظات:* ${notes}`);

            const text = encodeURIComponent(lines.join('\n'));
            window.open(`https://wa.me/${phone}?text=${text}`, '_blank');
        }

        // وظائف لوحة التحكم التجريبية
        function toggleAdminModal() {
            const modal = document.getElementById('admin-modal');
            modal.classList.toggle('hidden');
            if (!modal.classList.contains('hidden')) {
                renderAdminItems();
            }
        }

        function renderAdminItems() {
            const list = document.getElementById('admin-items-list');
            list.innerHTML = menuItems.map(item => `
                <div class="flex flex-wrap justify-between items-center gap-2 bg-darkbg p-2.5 rounded-xl border border-gray-800">
                    <span class="font-bold text-sm flex-1">${item.name}</span>
                    <div class="flex items-center gap-2">
                        <input type="number" value="${item.price}" onchange="updatePrice(${item.id}, this.value)" class="w-24 bg-cardbg border border-gray-700 rounded-lg p-1 text-center text-sm text-gold font-bold">
                        <span class="text-xs text-gray-400">د.ع</span>
                        <button onclick="toggleAvailability(${item.id})" class="px-2.5 py-1 rounded-lg text-xs font-bold ${item.available ? 'bg-emerald/20 text-emerald' : 'bg-red-500/20 text-red-400'}">
                            ${item.available ? 'متوفر' : 'نفذت'}
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function updatePrice(id, newPrice) {
            const item = menuItems.find(i => i.id === id);
            if (item && newPrice > 0) {
                item.price = parseInt(newPrice);
                renderMenu();
                updateCartUI();
            }
        }

        function toggleAvailability(id) {
            const item = menuItems.find(i => i.id === id);
            if (item) {
                item.available = !item.available;
                renderAdminItems();
                renderMenu();
            }
        }

        function addNewMenuItem() {
            const name = document.getElementById('new-item-name').value.trim();
            const price = parseInt(document.getElementById('new-item-price').value);
            const category = document.getElementById('new-item-cat').value;
            const desc = document.getElementById('new-item-desc').value.trim() || "طبق مميز محضر بعناية من شيف ميدتاون";

            if (!name || !price) {
                alert("يرجى كتابة اسم الطبق والسعر!");
                return;
            }

            menuItems.unshift({
                id: Date.now(),
                name,
                price,
                category,
                desc,
                image: "https://images.unsplash.com/photo-1544025162-d76694265947?auto=format&fit=crop&w=600&q=80",
                available: true,
                badge: "جديد المنيو ✨"
            });

            document.getElementById('new-item-name').value = '';
            document.getElementById('new-item-price').value = '';
            document.getElementById('new-item-desc').value = '';

            renderAdminItems();
            renderMenu();
            alert("تمت إضافة الطبق بنجاح لمنيو ميدتاون!");
        }

        // تشغيل أولي
        renderCategories();
        renderMenu();
    </script>
</body>
</html>
