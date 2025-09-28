# a<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Grocery / Food / Medicine - Sample App</title>
  <style>
    /* Theme: white + skyblue */
    :root{
      --sky: #a7e0ff;
      --sky-600: #6ec9f2;
      --accent: #0ea5d8;
      --bg: #ffffff;
      --muted: #666;
      --card-shadow: 0 2px 8px rgba(16,24,40,0.06);
      --radius: 10px;
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    html,body{height:100%; margin:0; background:linear-gradient(180deg,var(--bg), #f7fbff); color:#0b2540;}
    .container{max-width:1100px; margin:18px auto; padding:18px;}
    header{display:flex; align-items:center; justify-content:space-between; gap:12px;}
    .brand{display:flex; align-items:center; gap:10px;}
    .logo{
      width:58px; height:58px; border-radius:12px; background:linear-gradient(135deg,var(--sky),var(--sky-600)); display:flex;
      align-items:center; justify-content:center; color:#023047; font-weight:700; box-shadow:var(--card-shadow);
    }
    nav{display:flex; gap:10px; flex-wrap:wrap;}
    button.nav-btn{
      background:transparent; border:2px solid var(--sky-600); padding:8px 12px; border-radius:8px;
      cursor:pointer; font-weight:600;
    }
    main{margin-top:18px; display:flex; gap:18px;}
    /* left column = content, right column = order panel */
    .content{flex:1; min-height:60vh; padding:18px; border-radius:var(--radius); background:rgba(255,255,255,0.9); box-shadow:var(--card-shadow);}
    .right{width:320px; padding:18px; border-radius:var(--radius); background:linear-gradient(180deg, #ffffff, #f0fbff); box-shadow:var(--card-shadow);}
    .category-bar{display:flex; gap:8px; flex-wrap:wrap; margin-bottom:12px;}
    .cat{
      background:var(--bg); border:1px solid #e6f6ff; padding:8px 10px; border-radius:8px; cursor:pointer; font-weight:600;
    }
    .cat.active{background:linear-gradient(90deg,var(--sky),var(--sky-600)); color:#042; box-shadow:0 4px 14px rgba(14,165,216,0.12);}
    .grid{display:grid; grid-template-columns:repeat(auto-fill,minmax(120px,1fr)); gap:12px;}
    .card{background:#fff; border-radius:10px; padding:8px; text-align:center; box-shadow:var(--card-shadow);}
    .card img{width:100%; height:96px; object-fit:cover; border-radius:8px;}
    .card .name{font-size:13px; margin-top:6px;}
    .card button{margin-top:8px;padding:6px 8px;border-radius:8px;border:none;background:var(--accent); color:#fff;cursor:pointer;}
    /* order panel */
    .order-panel h3{margin:6px 0 12px;}
    .form-row{display:flex; gap:8px; margin-bottom:8px;}
    .form-row input, .form-row textarea{flex:1;padding:8px;border-radius:8px;border:1px solid #e6f6f6;}
    textarea{resize:vertical; min-height:64px;}
    .order-items{max-height:250px; overflow:auto; margin-bottom:10px;}
    .order-item{display:flex; gap:8px; align-items:center; padding:6px; border-radius:8px; border:1px dashed #eaf6ff; margin-bottom:8px;}
    .order-item img{width:48px;height:48px;object-fit:cover;border-radius:6px;}
    .order-actions{display:flex; gap:8px; justify-content:space-between; align-items:center;}
    .btn{padding:8px 10px; border-radius:8px; border:none; cursor:pointer; font-weight:700;}
    .btn-primary{background:var(--accent); color:white;}
    .btn-ghost{background:transparent; border:1px solid var(--sky-600); color:#033;}
    footer{margin-top:18px; font-size:13px; color:var(--muted);}
    .small{font-size:12px;color:#445;}
    /* orders list */
    .orders-list .order-card{border-radius:8px;padding:10px;margin-bottom:10px;border:1px solid #e8f7ff;background:#fff;}
    .top-actions{display:flex; gap:8px; align-items:center; margin-left:auto;}
    /* responsive */
    @media(max-width:880px){
      main{flex-direction:column;}
      .right{width:100%;}
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="brand">
        <div class="logo">G</div>
        <div>
          <div style="font-weight:800; font-size:18px;">Grocery / Food / Medicine</div>
          <div class="small">White • Skyblue theme — Orders via WhatsApp</div>
        </div>
      </div>

      <nav>
        <button class="nav-btn" id="nav-home">Home</button>
        <button class="nav-btn" id="nav-camera">Camera</button>
        <button class="nav-btn" id="nav-maps">Google Maps</button>
        <button class="nav-btn" id="nav-orders">Orders</button>
      </nav>
    </header>

    <main>
      <section class="content" id="page-content">
        <!-- Home / Category view will be rendered here -->
        <div id="home-view">
          <div class="category-bar" id="category-bar"></div>
          <div id="category-title" style="font-weight:700;margin:8px 0;color:#025;">Select category</div>
          <div class="grid" id="product-grid"></div>
        </div>

        <div id="camera-view" style="display:none;">
          <h3>Camera (capture image)</h3>
          <video id="camera-stream" autoplay playsinline style="width:100%; border-radius:8px; background:#000;"></video>
          <div style="margin-top:8px; display:flex; gap:8px;">
            <button class="btn btn-primary" id="capture-btn">Capture</button>
            <button class="btn btn-ghost" id="stop-camera-btn">Stop Camera</button>
          </div>
          <div style="margin-top:12px;">
            <canvas id="capture-canvas" style="display:none;"></canvas>
            <img id="captured-img" style="max-width:100%; border-radius:8px; display:none; margin-top:8px;" />
          </div>
        </div>

        <div id="maps-view" style="display:none;">
          <h3>Open Google Maps</h3>
          <div class="small">Enter address (or leave empty to open maps):</div>
          <div style="margin-top:8px; display:flex; gap:8px;">
            <input id="maps-address" placeholder="Delivery address or location" />
            <button class="btn btn-primary" id="maps-open-btn">Open Maps</button>
          </div>
        </div>

        <div id="orders-view" style="display:none;">
          <h3>Saved Orders</h3>
          <div class="orders-list" id="orders-list"></div>
          <div class="small">Orders are stored locally in your browser. Use "Resend via WhatsApp" to re-open WhatsApp with the order text.</div>
        </div>
      </section>

      <aside class="right">
        <div class="order-panel">
          <h3>Order Panel</h3>
          <div class="small">Selected category: <span id="selected-cat">Groceries → Milk</span></div>

          <div style="margin-top:10px;">
            <div class="form-row">
              <input id="cust-name" placeholder="Name" />
              <input id="cust-phone" placeholder="Phone" />
            </div>
            <div style="margin-top:8px;">
              <textarea id="cust-address" placeholder="Delivery address"></textarea>
            </div>
          </div>

          <div style="margin-top:10px; font-weight:700;">Order items</div>
          <div class="order-items" id="order-items"></div>

          <div style="margin-top:8px;">
            <button class="btn btn-primary" id="whatsapp-order">Order via WhatsApp</button>
            <button class="btn btn-ghost" id="clear-order">Clear</button>
          </div>

          <div style="margin-top:12px;">
            <div class="small">Captured image (optional)</div>
            <img id="order-captured" style="width:100%; height:120px; object-fit:cover; border-radius:8px; display:none; margin-top:8px;" />
          </div>

        </div>
      </aside>
    </main>

    <footer>
      <div class="small">How it works: pick a category → click items to add → fill name/phone/address → click "Order via WhatsApp" → WhatsApp opens with the message. Camera can capture a picture attached to the order (image shown but note: the image isn't uploaded to server, only local data URL).</div>
      <div style="margin-top:6px;" class="small">WhatsApp number used: <strong>+91 8977143043</strong></div>
    </footer>
  </div>

  <script>
    /**********************
     * Data: categories + 10 placeholder images each
     **********************/
    const WHATSAPP_NUMBER = '8977143043'; // as requested
    const CATEGORIES = {
      "Groceries - Milk": makeItems("Milk", 10),
      "Groceries - Meat": makeItems("Meat", 10),
      "Groceries - Flowers": makeItems("Flowers", 10),
      "Groceries - Vegetables": makeItems("Vegetables", 10),
      "Groceries - Fruits": makeItems("Fruits", 10),
      "Groceries - Others": makeItems("Others", 10),
      "Food": makeItems("Food", 10),
      "Medicine": makeItems("Medicine", 10)
    };

    function makeItems(prefix, n){
      const arr = [];
      for(let i=1;i<=n;i++){
        arr.push({
          id: prefix.toLowerCase() + "-" + i,
          name: `${prefix} item ${i}`,
          // placeholder image service (works online). If offline, browser will show broken images — you can replace with local images.
          img: `https://via.placeholder.com/400x300.png?text=${encodeURIComponent(prefix + ' ' + i)}`
        });
      }
      return arr;
    }

    /**********************
     * App state
     **********************/
    let selectedCategory = Object.keys(CATEGORIES)[0]; // default
    let basket = []; // array of items {id,name,img,category}
    let capturedDataUrl = null; // camera image for order

    /**********************
     * DOM references
     **********************/
    const categoryBar = document.getElementById('category-bar');
    const productGrid = document.getElementById('product-grid');
    const selectedCatSpan = document.getElementById('selected-cat');
    const orderItemsDiv = document.getElementById('order-items');
    const custName = document.getElementById('cust-name');
    const custPhone = document.getElementById('cust-phone');
    const custAddress = document.getElementById('cust-address');
    const whatsappBtn = document.getElementById('whatsapp-order');
    const clearBtn = document.getElementById('clear-order');
    const capturedImgEl = document.getElementById('captured-img');
    const orderCapturedEl = document.getElementById('order-captured');

    const navHome = document.getElementById('nav-home');
    const navCamera = document.getElementById('nav-camera');
    const navMaps = document.getElementById('nav-maps');
    const navOrders = document.getElementById('nav-orders');

    const homeView = document.getElementById('home-view');
    const cameraView = document.getElementById('camera-view');
    const mapsView = document.getElementById('maps-view');
    const ordersView = document.getElementById('orders-view');

    const cameraStream = document.getElementById('camera-stream');
    const captureBtn = document.getElementById('capture-btn');
    const stopCameraBtn = document.getElementById('stop-camera-btn');
    const captureCanvas = document.getElementById('capture-canvas');
    const mapsOpenBtn = document.getElementById('maps-open-btn');
    const mapsAddress = document.getElementById('maps-address');

    const ordersListDiv = document.getElementById('orders-list');

    /**********************
     * Init
     **********************/
    function init(){
      renderCategoryBar();
      renderProducts();
      renderBasket();
      attachNav();
      loadOrdersToUI();
      // load saved captured image if present
      const savedCap = localStorage.getItem('capturedImage');
      if(savedCap){ capturedDataUrl = savedCap; showCapturedDataUrl(); }
    }
    init();

    /**********************
     * Categories & products
     **********************/
    function renderCategoryBar(){
      categoryBar.innerHTML = '';
      Object.keys(CATEGORIES).forEach(cat=>{
        const btn = document.createElement('button');
        btn.className = 'cat' + (cat === selectedCategory ? ' active' : '');
        btn.innerText = cat;
        btn.onclick = () => {
          selectedCategory = cat;
          document.querySelectorAll('.cat').forEach(c=>c.classList.remove('active'));
          btn.classList.add('active');
          selectedCatSpan.innerText = 'Selected category: ' + cat;
          renderProducts();
        };
        categoryBar.appendChild(btn);
      });
      selectedCatSpan.innerText = 'Selected category: ' + selectedCategory;
    }

    function renderProducts(){
      productGrid.innerHTML = '';
      const items = CATEGORIES[selectedCategory] || [];
      document.getElementById('category-title').innerText = selectedCategory;
      items.forEach(it=>{
        const el = document.createElement('div');
        el.className = 'card';
        el.innerHTML = `
          <img loading="lazy" src="${it.img}" alt="${it.name}" />
          <div class="name">${it.name}</div>
          <button>Add</button>
        `;
        el.querySelector('button').onclick = () => {
          basket.push({...it, category:selectedCategory});
          renderBasket();
        };
        productGrid.appendChild(el);
      });
      if(items.length === 0){
        productGrid.innerHTML = '<div class="small">No items found.</div>';
      }
    }

    /**********************
     * Basket / Order UI
     **********************/
    function renderBasket(){
      orderItemsDiv.innerHTML = '';
      if(basket.length === 0){
        orderItemsDiv.innerHTML = '<div class="small">No items selected yet.</div>';
        return;
      }
      basket.forEach((it, idx)=>{
        const row = document.createElement('div');
        row.className = 'order-item';
        row.innerHTML = `
          <img src="${it.img}" />
          <div style="flex:1;">
            <div style="font-weight:700">${it.name}</div>
            <div class="small">${it.category}</div>
          </div>
          <div>
            <button style="background:transparent;border:none;color:#c00;font-weight:700;cursor:pointer" data-idx="${idx}">Remove</button>
          </div>
        `;
        row.querySelector('button').onclick = (e) => {
          const i = Number(e.target.dataset.idx);
          basket.splice(i,1);
          renderBasket();
        };
        orderItemsDiv.appendChild(row);
      });
    }

    clearBtn.onclick = () => {
      basket = [];
      custName.value = '';
      custPhone.value = '';
      custAddress.value = '';
      capturedDataUrl = null;
      localStorage.removeItem('capturedImage');
      orderCapturedEl.style.display = 'none';
      renderBasket();
    };

    /**********************
     * WhatsApp order
     **********************/
    whatsappBtn.onclick = () => {
      const name = custName.value.trim();
      const phone = custPhone.value.trim();
      const address = custAddress.value.trim();

      if(!name || !phone || !address){
        alert('Please fill Name, Phone and Delivery Address before ordering.');
        return;
      }
      if(basket.length === 0){
        alert('Please add at least one item to the order.');
        return;
      }

      // build order text
      let txt = `New order from ${name} (%2B${phone})%0A`; // encode newlines as %0A
      txt += `Delivery address: ${address}%0A%0A`;
      txt += `Items:%0A`;
      basket.forEach((it, idx)=>{
        txt += `${idx+1}. ${it.name} (${it.category})%0A`;
      });
      if(capturedDataUrl){
        txt += `%0ANote: customer attached a photo (captured in app).`;
      }
      // save to local orders
      const orderObj = {
        id: 'ord-' + Date.now(),
        name, phone, address,
        items: basket.slice(),
        captured: capturedDataUrl,
        timestamp: new Date().toISOString()
      };
      saveOrder(orderObj);

      // open whatsapp
      const url = `https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(decodeURIComponent(txt))}`;
      // Use window.open to open in new tab / invoke native app on mobile
      window.open(url, '_blank');

      // optional: clear basket after ordering
      basket = [];
      renderBasket();
    };

    /**********************
     * Orders storage: save to localStorage
     **********************/
    function saveOrder(order){
      const arr = JSON.parse(localStorage.getItem('savedOrders') || '[]');
      arr.unshift(order); // newest first
      localStorage.setItem('savedOrders', JSON.stringify(arr));
      loadOrdersToUI();
    }

    function loadOrdersToUI(){
      ordersListDiv.innerHTML = '';
      const arr = JSON.parse(localStorage.getItem('savedOrders') || '[]');
      if(arr.length === 0){
        ordersListDiv.innerHTML = '<div class="small">No orders saved yet.</div>';
        return;
      }
      arr.forEach(ord=>{
        const card = document.createElement('div');
        card.className = 'order-card';
        card.innerHTML = `
          <div style="display:flex; justify-content:space-between; align-items:center;">
            <div>
              <div style="font-weight:800">${ord.name} — ${ord.phone}</div>
              <div class="small">${new Date(ord.timestamp).toLocaleString()}</div>
            </div>
            <div style="text-align:right;">
              <button class="btn btn-ghost btn-resend" data-id="${ord.id}">Resend via WhatsApp</button>
            </div>
          </div>
          <div style="margin-top:8px;" class="small">Address: ${escapeHtml(ord.address)}</div>
          <div style="margin-top:8px;">
            ${ord.items.map(i => `<div class="small">• ${escapeHtml(i.name)} <em class="small">(${escapeHtml(i.category)})</em></div>`).join('')}
          </div>
          ${ord.captured ? `<div style="margin-top:8px"><img src="${ord.captured}" style="width:100%; border-radius:8px; object-fit:cover;" /></div>` : ''}
          <div style="margin-top:8px; display:flex; gap:8px; justify-content:flex-end;">
            <button class="btn" data-id="${ord.id}" style="background:#ffefc2;" onclick="deleteOrder('${ord.id}')">Delete</button>
          </div>
        `;
        ordersListDiv.appendChild(card);
      });

      // attach handlers for resend buttons
      document.querySelectorAll('.btn-resend').forEach(b=>{
        b.onclick = (e) => {
          const id = e.target.dataset.id;
          const arr = JSON.parse(localStorage.getItem('savedOrders') || '[]');
          const ord = arr.find(x=>x.id===id);
          if(!ord) return alert('Order not found');
          let txt = `Order from ${ord.name} (%2B${ord.phone})%0ADelivery address: ${ord.address}%0A%0AItems:%0A`;
          ord.items.forEach((it, idx)=> txt += `${idx+1}. ${it.name} (${it.category})%0A`);
          if(ord.captured) txt += `%0APhoto attached by customer.`;
          const url = `https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(txt)}`;
          window.open(url, '_blank');
        };
      });
    }

    // delete order (global function used in inline onclick)
    window.deleteOrder = function(id){
      let arr = JSON.parse(localStorage.getItem('savedOrders') || '[]');
      arr = arr.filter(a=>a.id !== id);
      localStorage.setItem('savedOrders', JSON.stringify(arr));
      loadOrdersToUI();
    };

    function escapeHtml(s){
      return String(s).replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;');
    }

    /**********************
     * Camera functionality (getUserMedia)
     **********************/
    let streamRef = null;
    navCamera.onclick = startCameraRoute;

    function startCameraRoute(){
      // show camera view
      showView('camera');
      startCamera();
    }

    async function startCamera(){
      stopCamera();
      try{
        const constraints = { video: { facingMode: "environment" }, audio:false };
        const stream = await navigator.mediaDevices.getUserMedia(constraints);
        streamRef = stream;
        cameraStream.srcObject = stream;
        cameraStream.play();
      }catch(err){
        alert('Camera access failed or blocked. ' + (err && err.message ? err.message : ''));
      }
    }

    captureBtn.onclick = () => {
      if(!streamRef) return alert('Camera not started.');
      const video = cameraStream;
      captureCanvas.width = video.videoWidth;
      captureCanvas.height = video.videoHeight;
      const ctx = captureCanvas.getContext('2d');
      ctx.drawImage(video, 0, 0, captureCanvas.width, captureCanvas.height);
      const dataUrl = captureCanvas.toDataURL('image/jpeg', 0.85);
      capturedImgEl.src = dataUrl;
      capturedImgEl.style.display = 'block';
      // save to order captured
      capturedDataUrl = dataUrl;
      localStorage.setItem('capturedImage', dataUrl);
      showCapturedDataUrl();
      alert('Image captured and attached to order panel.');
    };

    stopCameraBtn.onclick = () => {
      stopCamera();
      cameraStream.srcObject = null;
    };

    function stopCamera(){
      if(streamRef){
        streamRef.getTracks().forEach(t => t.stop());
        streamRef = null;
      }
    }

    function showCapturedDataUrl(){
      if(capturedDataUrl){
        orderCapturedEl.src = capturedDataUrl;
        orderCapturedEl.style.display = 'block';
      }else{
        orderCapturedEl.style.display = 'none';
      }
    }

    /**********************
     * Maps
     **********************/
    navMaps.onclick = () => showView('maps');
    mapsOpenBtn.onclick = () => {
      const address = mapsAddress.value.trim();
      let url;
      if(address){
        url = `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(address)}`;
      }else{
        url = 'https://www.google.com/maps';
      }
      window.open(url, '_blank');
    };

    /**********************
     * Nav / SPA switching
     **********************/
    navHome.onclick = ()=> showView('home');
    navOrders.onclick = ()=> showView('orders');

    function showView(name){
      homeView.style.display = (name==='home') ? '' : 'none';
      cameraView.style.display = (name==='camera') ? '' : 'none';
      mapsView.style.display = (name==='maps') ? '' : 'none';
      ordersView.style.display = (name==='orders') ? '' : 'none';
      // stop camera when leaving
      if(name !== 'camera') stopCamera();
      // update active states
      [navHome, navCamera, navMaps, navOrders].forEach(btn => btn.style.opacity = '1');
      if(name==='home') navHome.style.opacity = '0.9';
      if(name==='camera') navCamera.style.opacity = '0.9';
      if(name==='maps') navMaps.style.opacity = '0.9';
      if(name==='orders') navOrders.style.opacity = '0.9';
    }

    function attachNav(){
      // default to home
      showView('home');
    }

    /**********************
     * Load orders on init
     **********************/
    // already done in init() via loadOrdersToUI

    /**********************
     * Helpful note:
     * This is a front-end only demo. In production, attachments (captured images) should be uploaded to a server and a store DB used for orders.
     **********************/
  </script>
</body>
</html>
