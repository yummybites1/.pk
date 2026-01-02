<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>YUMMY BITES | Fresh Snacks</title>  
      
    <script src="https://cdn.tailwindcss.com"></script>  
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>  
      
    <link href="https://fonts.googleapis.com/css2?family=Syncopate:wght@700&family=Outfit:wght@300;400;600;900&display=swap" rel="stylesheet">  
  
    <style>  
        :root { --brand: #FF6B35; --deep: #0F0F0F; }  
        body { font-family: 'Outfit', sans-serif; background: var(--deep); color: white; scroll-behavior: smooth; overflow-x: hidden; }  
        .font-brand { font-family: 'Syncopate', sans-serif; }  
          
        img { image-rendering: -webkit-optimize-contrast; object-fit: cover; width: 100%; height: 100%; transition: 0.5s; }  
        .product-card:hover img { transform: scale(1.1); }  
        .product-card { background: #1a1a1a; border: 1px solid rgba(255,255,255,0.05); transition: 0.3s; }  
        .product-card:hover { border-color: var(--brand); }  
  
        #checkoutOverlay { position: fixed; inset: 0; background: black; z-index: 500; display: none; flex-direction: column; overflow-y: auto; }  
  
        .elite-input {  
            background: rgba(255,255,255,0.05); border: 1px solid rgba(255, 107, 53, 0.2);  
            padding: 1.2rem; border-radius: 15px; width: 100%; outline: none; color: white; margin-bottom: 12px;  
        }  
  
        .btn-main { background: var(--brand); color: white; font-weight: 900; transition: 0.3s; }  
        .btn-main:hover { transform: scale(1.02); box-shadow: 0 0 20px rgba(255, 107, 53, 0.4); }  
  
        #thankYouScreen { display: none; position: fixed; inset: 0; background: black; z-index: 600; align-items: center; justify-content: center; text-align: center; }  
    </style>  
</head>  
<body>  
  
    <div class="bg-[#FF6B35] text-white py-2 px-4 text-center text-[10px] font-black uppercase tracking-widest fixed top-0 w-full z-[200]">  
        PREMIUM SNACKS | FAST DELIVERY  
    </div>  
  
    <div id="thankYouScreen">  
        <div class="px-6">  
            <h2 class="font-brand text-2xl text-[#FF6B35] mb-4">ORDER RECEIVED!</h2>  
            <p class="text-gray-400 text-xs tracking-widest uppercase mb-10">We will process your yummy bites shortly.</p>  
            <button onclick="window.location.reload()" class="bg-white text-black px-12 py-5 rounded-2xl font-black uppercase text-[10px]">Back to Shop</button>  
        </div>  
    </div>  
  
    <div id="checkoutOverlay">  
        <div class="max-w-2xl mx-auto w-full px-6 py-24">  
            <div class="flex justify-between items-center mb-12">  
                <h2 class="font-brand text-lg text-[#FF6B35]">MY CART</h2>  
                <button onclick="toggleCheckout()" class="text-4xl text-gray-500">&times;</button>  
            </div>  
            <div id="checkoutItems" class="space-y-4 mb-10"></div>  
            <div class="bg-zinc-900 p-8 rounded-[2rem] mb-10">  
                <div class="flex justify-between items-center">  
                    <span class="text-xs text-gray-400 uppercase font-black">Total Payable</span>  
                    <span id="finalPrice" class="text-4xl font-black text-[#FF6B35]">Rs 0</span>  
                </div>  
            </div>  
            <form onsubmit="handleFinalOrder(event)">  
                <input type="text" placeholder="NAME" required class="elite-input">  
                <input type="tel" placeholder="WHATSAPP NUMBER" required class="elite-input">  
                <textarea placeholder="FULL ADDRESS" required class="elite-input h-24 resize-none"></textarea>  
                <button type="submit" class="w-full btn-main py-6 rounded-[1.5rem] font-black uppercase text-xs mt-6">Place Order</button>  
            </form>  
        </div>  
    </div>  
  
    <nav class="fixed top-8 w-full z-[100] bg-black/80 backdrop-blur-xl px-8 py-6 flex justify-between items-center border-b border-white/5">  
        <div class="font-brand text-xl tracking-tighter">YUMMY<span class="text-[#FF6B35]">BITES</span></div>  
        <button onclick="toggleCheckout()" class="relative bg-white/5 p-3 rounded-full">  
            🛒 <span id="cartCount" class="absolute -top-1 -right-1 bg-[#FF6B35] text-white text-[10px] w-5 h-5 flex items-center justify-center rounded-full font-black">0</span>  
        </button>  
    </nav>  
  
    <header class="h-[50vh] flex items-center justify-center text-center px-6 pt-20">  
        <div>  
            <h1 class="font-brand text-5xl md:text-7xl mb-4 uppercase leading-none">The Snack <br><span class="text-[#FF6B35]">Hub</span></h1>  
            <p class="text-gray-500 tracking-widest text-[10px] uppercase">Freshness in every single bite</p>  
        </div>  
    </header>  
  
    <main id="mainGrid" class="max-w-[1400px] mx-auto px-8 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 pb-40"></main>  
  
    <script>  
        let cart = [];  
  
        // Updated with your requested items, prices, and images  
        const products = [  
            { n: "Yummy Bites Chocolate", p: 100, img: "https://images.unsplash.com/photo-1614088685112-0a760b71a3c8?q=80&w=500&auto=format&fit=crop" },  
            { n: "Sooper Cake", p: 30, img: "https://i.ibb.co/4ZPMFN6G/image.png" }, // Using your first provided link  
            { n: "Lays", p: 100, img: "https://i.ibb.co/wZ211JXn/image.png" },    // Using your second provided link  
            { n: "Biscuits Box", p: 350, img: "https://i.ibb.co/V0jM02Jy/image.png" } // Using your third provided link  
        ];  
  
        const grid = document.getElementById('mainGrid');  
        products.forEach((p, i) => {  
            grid.innerHTML += `  
                <div class="product-card p-4 rounded-[1.5rem]">  
                    <div class="aspect-square rounded-xl overflow-hidden mb-4 bg-black">  
                        <img src="${p.img}" alt="${p.n}">  
                    </div>  
                    <div class="flex justify-between items-end">  
                        <div>  
                            <h4 class="text-[11px] font-bold uppercase mb-1">${p.n}</h4>  
                            <span class="text-xl font-black text-[#FF6B35]">Rs ${p.p}</span>  
                        </div>  
                        <button onclick="addToCart(${i})" class="bg-white text-black px-4 py-2 rounded-lg font-black text-[10px] uppercase">Add</button>  
                    </div>  
                </div>`;  
        });  
  
        function toggleCheckout() {  
            const el = document.getElementById('checkoutOverlay');  
            el.style.display = (el.style.display === 'flex') ? 'none' : 'flex';  
        }  
  
        function addToCart(i) {  
            cart.push(products[i]);  
            updateUI();  
            confetti({ particleCount: 20, spread: 40, origin: { y: 0.9 }, colors: ['#FF6B35'] });  
        }  
  
        function updateUI() {  
            document.getElementById('cartCount').innerText = cart.length;  
            let total = cart.reduce((s, item) => s + item.p, 0);  
            document.getElementById('finalPrice').innerText = `Rs ${total}`;  
            const container = document.getElementById('checkoutItems');  
            container.innerHTML = cart.map(item => `  
                <div class="flex justify-between items-center bg-white/5 p-4 rounded-xl">  
                    <span class="text-[10px] font-bold uppercase">${item.n}</span>  
                    <span class="text-[#FF6B35] font-black">Rs ${item.p}</span>  
                </div>`).join('');  
        }  
  
        function handleFinalOrder(e) {  
            e.preventDefault();  
            document.getElementById('checkoutOverlay').style.display = 'none';  
            document.getElementById('thankYouScreen').style.display = 'flex';  
            confetti({ particleCount: 150, spread: 70 });  
        }  
    </script>  
</body>  
</html>  
