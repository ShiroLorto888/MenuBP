import React, { useState } from "react";

// BuraphaMenuApp.jsx
// Single-file React component (TailwindCSS assumed) that previews a mobile-friendly menu app.
// - English primary, Thai secondary
// - Theme closely follows the PDF (clean, centered headings, serif-like feel simulated)
// - Images: optional per-item. If not provided, a placeholder is shown and user can upload per-item.

export default function BuraphaMenuApp() {
  const urlParams = new URLSearchParams(window.location.search);
  const [adminMode, setAdminMode] = useState(urlParams.get('admin') === '1');
  const [pw, setPw] = useState('');
  const correctPw = '25402612';
  const initialData = [
    {
      id: 1,
      category: "Breakfast",
      title_en: "American Breakfast",
      title_th: "อาหารเช้าแบบอเมริกัน",
      desc_en: "Two eggs any style with ham, bacon, sausage, toast with jam & butter, juice, coffee or tea and fresh fruit",
      desc_th: "ไข่ 2 ฟอง แฮม เบคอน ไส้กรอก ขนมปัง แยม เนย น้ำผลไม้ กาแฟหรือชา ผลไม้สด",
      price: "240฿",
      image: null,
    },
    {
      id: 2,
      category: "Appetizers",
      title_en: "Deep Fried Bread with Pork",
      title_th: "ขนมปังหน้าหมู",
      desc_en: "Crispy deep fried bread topped with seasoned pork",
      desc_th: "ขนมปังทอดกรอบหน้าหมูปรุงรส",
      price: "150฿",
      image: null,
    },
    {
      id: 3,
      category: "Seafood Mains",
      title_en: "Stir-Fried Squid with Salted Egg Yolk",
      title_th: "ปลาหมึกผัดไข่เค็ม",
      desc_en: "Classic stir-fry of squid with rich salted egg yolk sauce",
      desc_th: "ปลาหมึกผัดซอสไข่เค็ม",
      price: "250฿",
      image: null,
    },
    {
      id: 4,
      category: "Soups & Curries",
      title_en: "Tom Yam Koong",
      title_th: "ต้มยำกุ้ง",
      desc_en: "Spicy shrimp soup with lemongrass",
      desc_th: "ต้มยำกุ้ง ตะไคร้และสมุนไพร",
      price: "250฿",
      image: null,
    },
  ];

  const [items, setItems] = useState(initialData);
  const [query, setQuery] = useState("");
  const [selectedCategory, setSelectedCategory] = useState("All");

  const categories = ["All", ...Array.from(new Set(items.map((i) => i.category)))];

  function handleImageUpload(e, id) {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (ev) => {
      setItems((prev) => prev.map((it) => (it.id === id ? { ...it, image: ev.target.result } : it)));
    };
    reader.readAsDataURL(file);
  }

  function filteredItems() {
    return items.filter((it) => {
      const matchesCategory = selectedCategory === "All" || it.category === selectedCategory;
      const text = (it.title_en + " " + it.title_th + " " + it.desc_en + " " + it.desc_th + " " + it.price).toLowerCase();
      const matchesQuery = query.trim() === "" || text.includes(query.toLowerCase());
      return matchesCategory && matchesQuery;
    });
  }

  // Allow bulk import of menu JSON (paste format from PDF extraction)
  function handleBulkImport(jsonText) {
    try {
      const parsed = JSON.parse(jsonText);
      // Expect array of items with fields: category, title_en, title_th, desc_en, desc_th, price
      const withIds = parsed.map((it, idx) => ({ id: Date.now() + idx, image: null, ...it }));
      setItems((prev) => [...prev, ...withIds]);
      alert("Imported " + withIds.length + " items");
    } catch (err) {
      alert("Invalid JSON: " + err.message);
    }
  }

  return (
    <div className="min-h-screen bg-white text-gray-900 p-4 sm:p-6 font-sans">
      <header className="max-w-xl mx-auto mb-4 flex items-center justify-between">
        <div className="text-center flex-1">
          <h1 className="text-3xl font-bold tracking-wider">Burapha</h1>
          <p className="text-sm mt-1">Food Menu — English primary / ไทยรอง</p>
        </div>
        <div className="ml-2">
          {!adminMode && (
            <button onClick={()=>{ const pwInput = prompt('Admin password'); if(pwInput===correctPw){ setAdminMode(true);} else { alert('Wrong password'); } }} className="text-xs border px-2 py-1 rounded">
              Login
            </button>
          )}
          {adminMode && (
            <div className="flex flex-col items-end">
              <span className="text-xs text-green-600">Admin</span>
              <button onClick={()=>setAdminMode(false)} className="text-xs border px-2 py-1 rounded mt-1">
                Logout
              </button>
              <button onClick={()=>alert('Register new admins must be done manually inside code or future backend.')} className="text-xs border px-2 py-1 rounded mt-1">
                Register
              </button>
            </div>
          )}
        </div>
      </header>

      {!adminMode && urlParams.get('admin') === '1' && (
        <div className="max-w-xl mx-auto mb-4 p-3 border rounded bg-gray-50">
          <p className="text-sm mb-2">Enter admin password:</p>
          <input type="password" value={pw} onChange={(e)=>setPw(e.target.value)} className="p-2 border rounded w-full mb-2" />
          <button onClick={()=>{ if(pw===correctPw){setAdminMode(true);} else {alert('Wrong password');}}} className="px-3 py-1 border rounded w-full">Unlock</button>
        </div>
      )}

      <main className="max-w-xl mx-auto">
        <div className="flex gap-2 mb-3">
          <input
            value={query}
            onChange={(e) => setQuery(e.target.value)}
            placeholder="Search menu..."
            className="flex-1 p-2 border rounded"
          />
          <select value={selectedCategory} onChange={(e) => setSelectedCategory(e.target.value)} className="p-2 border rounded">
            {categories.map((c) => (
              <option key={c} value={c}>
                {c}
              </option>
            ))}
          </select>
        </div>

        <section className="space-y-6">
          {filteredItems().map((item) => (
            <article key={item.id} className="border rounded-lg p-3 flex gap-3 items-center">
              <div className="w-24 h-24 flex-shrink-0 rounded overflow-hidden bg-gray-100 flex items-center justify-center">
                {item.image ? (
                  // eslint-disable-next-line jsx-a11y/img-redundant-alt
                  <img src={item.image} alt={item.title_en} className="w-full h-full object-cover" />
                ) : (
                  <div className="text-xs text-gray-500 text-center p-2">No image<br/>Tap to add</div>
                )}
              </div>

              <div className="flex-1">
                <div className="flex justify-between items-start">
                  <div>
                    <h3 className="text-lg font-semibold">{item.title_en}</h3>
                    <div className="text-sm text-gray-600">{item.title_th}</div>
                  </div>
                  <div className="text-right">
                    <div className="font-bold">{item.price}</div>
                    <div className="text-xs text-gray-500">{item.category}</div>
                  </div>
                </div>
                <p className="mt-2 text-sm text-gray-700">{item.desc_en}</p>
                <p className="mt-1 text-xs text-gray-500">{item.desc_th}</p>

                {adminMode && (
                <div className="mt-2 flex items-center gap-2">
                  <label className="text-xs cursor-pointer inline-block px-2 py-1 border rounded text-gray-600">
                    Upload image
                    <input type="file" accept="image/*" className="hidden" onChange={(e) => handleImageUpload(e, item.id)} />
                  </label>
                </div>
              )}
              </div>
            </article>
          ))}

          {filteredItems().length === 0 && <div className="text-center text-gray-500 p-6">No items found</div>}
        </section>

        <footer className="mt-6 text-center text-sm text-gray-500">
          <div>Theme follows the original PDF look — simple, centered headings.</div>
          <div className="mt-2">You can bulk-import menu items as JSON via browser console or paste into the importer below.</div>

          {adminMode && (
            <div className="mt-3">
              <BulkImport onImport={handleBulkImport} />
            </div>
          )}
        </footer>
      </main>
    </div>
  );
}

function BulkImport({ onImport }) {
  const [text, setText] = useState("");
  return (
    <div className="mt-3">
      <textarea value={text} onChange={(e) => setText(e.target.value)} placeholder='Paste JSON array of items here (category,title_en,title_th,desc_en,desc_th,price)'
        className="w-full p-2 border rounded h-28" />
      <div className="flex gap-2 mt-2">
        <button className="px-3 py-1 border rounded" onClick={() => onImport(text)}>Import JSON</button>
        <button className="px-3 py-1 border rounded" onClick={() => { setText(''); }}>Clear</button>
      </div>
    </div>
  );
}
