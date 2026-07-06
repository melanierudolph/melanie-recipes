<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Melanie's Recipes</title>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Fraunces:ital,wght@0,600;0,700;1,500&family=Inter:wght@400;500;600&family=IBM+Plex+Mono:wght@500&display=swap">
<style>
  * { box-sizing: border-box; }
  body {
    margin: 0;
    font-family: 'Inter', sans-serif;
    background: #FFFFFF;
  }
  #app {
    display: flex;
    min-height: 100vh;
  }
  @media (max-width: 800px) {
    #app { flex-direction: column; }
  }

  /* SIDEBAR */
  #sidebar {
    width: 300px;
    flex-shrink: 0;
    background: #FFFFFF;
    border-right: 1px solid #E5E5E5;
    display: flex;
    flex-direction: column;
  }
  @media (max-width: 800px) {
    #sidebar { width: 100%; }
  }
  .sidebar-header {
    padding: 24px 20px 16px;
    border-bottom: 1px solid rgba(42,29,21,0.7);
  }
  .brand {
    display: flex;
    align-items: center;
    gap: 8px;
    color: #000000;
  }
  .brand svg { flex-shrink: 0; }
  .brand span {
    font-family: 'Fraunces', serif;
    font-weight: 700;
    font-size: 21px;
  }
  .sidebar-controls { padding: 16px 16px 0; }
  .search-wrap { position: relative; }
  .search-wrap svg {
    position: absolute; left: 11px; top: 50%; transform: translateY(-50%);
    color: #666666;
  }
  #search {
    width: 100%;
    background: #FFFFFF;
    border: 1px solid #DDDDDD;
    color: #000000;
    font-size: 14px;
    border-radius: 2px;
    padding: 8px 12px 8px 34px;
    outline: none;
    font-family: 'Inter', sans-serif;
  }
  #search::placeholder { color: #666666; }
  #search:focus { border-color: #000000; }
  #new-btn {
    margin-top: 12px;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    background: #000000;
    color: #FFFFFF;
    border: none;
    font-size: 14px;
    font-weight: 500;
    border-radius: 2px;
    padding: 9px 0;
    cursor: pointer;
    font-family: 'Inter', sans-serif;
    transition: background 0.15s;
  }
  #new-btn:hover { background: #333333; }

  #recipe-list {
    flex: 1;
    overflow-y: auto;
    padding: 16px;
  }
  .empty-note {
    color: #666666;
    font-size: 14px;
    line-height: 1.5;
  }
  .letter-group { margin-bottom: 16px; }
  .letter-label {
    color: #000000;
    font-size: 12px;
    letter-spacing: 0.1em;
    margin-bottom: 6px;
    padding-bottom: 4px;
    border-bottom: 1px solid #DDDDDD;
    font-family: 'IBM Plex Mono', monospace;
  }
  .recipe-item {
    width: 100%;
    text-align: left;
    background: none;
    border: none;
    color: #000000;
    font-size: 14px;
    padding: 6px 8px;
    border-radius: 2px;
    margin-bottom: 2px;
    cursor: pointer;
    font-family: 'Inter', sans-serif;
    display: block;
  }
  .recipe-item:hover { background: rgba(74,54,36,0.5); }
  .recipe-item.active { background: #DDDDDD; color: #FFFFFF; }

  /* MAIN */
  #main {
    flex: 1;
    background: #FFFFFF;
    padding: 40px 24px;
    display: flex;
    justify-content: center;
    overflow-y: auto;
  }
  .empty-state {
    text-align: center;
    margin-top: 64px;
    max-width: 380px;
  }
  .empty-state svg { color: #000000; margin-bottom: 16px; }
  .empty-state h2 {
    color: #000000;
    font-family: 'Fraunces', serif;
    font-weight: 700;
    font-size: 21px;
    margin: 0 0 8px;
  }
  .empty-state p {
    color: #666666;
    font-size: 14px;
    line-height: 1.6;
    margin: 0;
  }

  .card-column { width: 100%; max-width: 640px; }
  .toolbar {
    display: flex;
    justify-content: flex-end;
    gap: 8px;
    margin-bottom: 12px;
  }
  .toolbar.split { justify-content: space-between; }
  .tbtn {
    display: flex;
    align-items: center;
    gap: 6px;
    background: none;
    font-size: 14px;
    padding: 6px 12px;
    border-radius: 2px;
    border: 1px solid #DDDDDD;
    color: #000000;
    cursor: pointer;
    font-family: 'Inter', sans-serif;
  }
  .tbtn:hover { color: #FFFFFF; border-color: #000000; }
  .tbtn.danger:hover { color: #000000; border-color: #000000; }
  .tbtn.back { border: none; padding: 6px 4px; }
  .tbtn.save {
    background: #000000;
    color: #FFFFFF;
    border: none;
    font-weight: 500;
  }
  .tbtn.save:hover { background: #333333; }
  .confirm-row { display: flex; align-items: center; gap: 10px; font-size: 14px; }
  .confirm-row span { color: #000000; }
  .confirm-row button { background: none; border: none; cursor: pointer; font-size: 14px; font-family: 'Inter', sans-serif; }
  .confirm-yes { color: #000000; font-weight: 500; }
  .confirm-no { color: #666666; }

  .card {
    position: relative;
    background: #FFFFFF;
    border-radius: 2px;
    border: 1px solid #E0E0E0;
    box-shadow: 0 4px 16px rgba(0,0,0,0.06);
    padding: 32px 32px 32px 64px;
  }
  .holes {
    position: absolute;
    left: 24px; top: 32px; bottom: 32px;
    display: flex; flex-direction: column; justify-content: space-between;
  }
  .hole {
    width: 14px; height: 14px; border-radius: 50%;
    background: rgba(0,0,0,0.7);
    box-shadow: inset 0 1px 2px rgba(0,0,0,0.5);
  }
  .margin-line {
    position: absolute; left: 56px; top: 0; bottom: 0; width: 1px;
    background: #CCCCCC;
  }
  .card h1 {
    color: #000000; font-family: 'Fraunces', serif; font-weight: 700;
    font-size: 30px; margin: 0 0 4px;
  }
  .card .desc {
    color: #333333; font-family: 'Fraunces', serif; font-style: italic;
    font-size: 14px; margin: 0 0 12px;
  }
  .rule { height: 2px; background: #000000; margin-bottom: 16px; }
  .meta-row { display: flex; flex-wrap: wrap; gap: 16px; margin-bottom: 20px; color: #333333; font-size: 14px; }
  .meta-row span { display: flex; align-items: center; gap: 6px; }
  .tags { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 24px; }
  .tag {
    font-size: 12px; padding: 2px 9px; border-radius: 999px;
    background: rgba(0,0,0,0.05); color: #000000;
    border: 1px solid rgba(0,0,0,0.2);
  }
  .section-label {
    color: #000000; font-size: 12px; letter-spacing: 0.1em;
    margin-bottom: 8px; font-family: 'IBM Plex Mono', monospace;
  }
  .ingredients-list { margin: 0 0 24px; padding: 0; list-style: none;
    background-image: repeating-linear-gradient(to bottom, transparent, transparent 27px, rgba(0,0,0,0.08) 28px);
  }
  .ingredients-list li { display: flex; gap: 12px; padding: 4px 0; font-size: 14px; line-height: 28px; }
  .ing-amount { color: #000000; width: 96px; flex-shrink: 0; text-align: right; font-family: 'IBM Plex Mono', monospace; }
  .ing-name { color: #000000; }
  .steps-list { margin: 0; padding: 0; list-style: none; }
  .steps-list li { display: flex; gap: 12px; font-size: 14px; color: #000000; line-height: 1.6; margin-bottom: 12px; }
  .step-num { color: #000000; font-weight: 600; flex-shrink: 0; font-family: 'IBM Plex Mono', monospace; }

  /* FORM */
  .field-label {
    color: #000000; font-size: 11px; letter-spacing: 0.03em; display: block; margin-bottom: 2px;
  }
  .field-input {
    width: 100%; background: transparent; border: none; border-bottom: 1px solid #CCCCCC;
    outline: none; color: #000000; font-size: 14px; padding: 6px 0;
    font-family: 'Inter', sans-serif;
  }
  .field-input:focus { border-color: #000000; }
  .field-input::placeholder { color: #999999; }
  .title-input {
    width: 100%; background: transparent; border: none; outline: none;
    color: #000000; font-size: 30px; margin-bottom: 8px;
    font-family: 'Fraunces', serif; font-weight: 700;
  }
  .title-input::placeholder { color: #999999; }
  .desc-input {
    width: 100%; background: transparent; border: none; outline: none;
    color: #333333; font-size: 14px; font-style: italic; margin-bottom: 12px;
    font-family: 'Fraunces', serif;
  }
  .desc-input::placeholder { color: #999999; }
  .grid3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 12px; margin-bottom: 20px; }
  .form-block { margin-bottom: 24px; }
  .ing-row, .step-row { display: flex; gap: 12px; align-items: center; margin-bottom: 8px; }
  .step-row { align-items: flex-start; }
  .ing-amount-input {
    width: 96px; flex-shrink: 0; text-align: right; background: transparent;
    border: none; border-bottom: 1px solid #CCCCCC; outline: none;
    color: #000000; font-size: 14px; padding: 4px 0;
    font-family: 'IBM Plex Mono', monospace;
  }
  .ing-amount-input:focus { border-color: #000000; }
  .ing-name-input {
    flex: 1; background: transparent; border: none; border-bottom: 1px solid #CCCCCC;
    outline: none; color: #000000; font-size: 14px; padding: 4px 0;
    font-family: 'Inter', sans-serif;
  }
  .ing-name-input:focus { border-color: #000000; }
  .step-text {
    flex: 1; background: transparent; border: none; border-bottom: 1px solid #CCCCCC;
    outline: none; color: #000000; font-size: 14px; padding: 4px 0; resize: none;
    font-family: 'Inter', sans-serif;
  }
  .step-text:focus { border-color: #000000; }
  .step-num-form {
    color: #000000; font-weight: 600; padding-top: 6px; flex-shrink: 0;
    font-family: 'IBM Plex Mono', monospace;
  }
  .row-remove {
    background: none; border: none; color: #999999; cursor: pointer; padding: 0;
    display: flex; align-items: center;
  }
  .row-remove:hover { color: #000000; }
  .add-link {
    display: flex; align-items: center; gap: 6px; background: none; border: none;
    color: #000000; font-size: 14px; cursor: pointer; padding: 4px 0;
    font-family: 'Inter', sans-serif;
  }
  .add-link:hover { color: #5f7357; }
</style>
</head>
<body>
<div id="app">
  <div id="sidebar">
    <div class="sidebar-header">
      <div class="brand">
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.75" stroke-linecap="round" stroke-linejoin="round"><path d="M17.5 21a2 2 0 0 0 2-2v-8.5a2.5 2.5 0 0 0-2.5-2.5h-2.007a3 3 0 0 0-2.986-3.5A3 3 0 0 0 8.996 8H7a2.5 2.5 0 0 0-2.5 2.5V19a2 2 0 0 0 2 2h11z"/><path d="M6 21v-7a6 6 0 0 1 6-6v0a6 6 0 0 1 6 6v7"/></svg>
        <span>Melanie's Recipes</span>
      </div>
    </div>
    <div class="sidebar-controls">
      <div class="search-wrap">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/></svg>
        <input id="search" placeholder="Search recipes or tags" />
      </div>
      <button id="new-btn">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M5 12h14M12 5v14"/></svg>
        New recipe
      </button>
    </div>
    <div id="recipe-list"></div>
  </div>
  <div id="main"></div>
</div>

<script>
const uid = () => Math.random().toString(36).slice(2, 10);
const STORAGE_KEY = "recipe-box-recipes-v1";

const emptyForm = () => ({
  id: null,
  title: "",
  description: "",
  servings: "",
  prepTime: "",
  cookTime: "",
  tags: "",
  ingredients: [{ id: uid(), amount: "", name: "" }],
  steps: [{ id: uid(), text: "" }],
});

let recipes = [];
let selectedId = null;
let mode = "empty"; // empty | view | form
let form = emptyForm();
let search = "";
let confirmingDelete = null;

const defaultRecipes = [
  {
    id: uid(),
    title: "Teriyaki Chicken Meatballs",
    description: "",
    servings: "16-18 meatballs",
    prepTime: "",
    cookTime: "15-20 min",
    tags: [],
    ingredients: [
      { id: uid(), amount: "1 lb", name: "ground chicken" },
      { id: uid(), amount: "1", name: "egg" },
      { id: uid(), amount: "1/2 cup", name: "almond flour or bread crumbs" },
      { id: uid(), amount: "2 cloves", name: "garlic, minced" },
      { id: uid(), amount: "1 tbsp", name: "freshly grated ginger" },
      { id: uid(), amount: "3 tbsp", name: "green onions, chopped" },
      { id: uid(), amount: "1 tbsp", name: "soy sauce" },
      { id: uid(), amount: "2 tsp", name: "sriracha, optional" },
      { id: uid(), amount: "1/2 tsp", name: "kosher salt" },
      { id: uid(), amount: "1/2 tsp", name: "black pepper" },
      { id: uid(), amount: "1/2 cup", name: "teriyaki sauce, to serve" },
      { id: uid(), amount: "", name: "sesame seeds and scallions, to serve" }
    ],
    steps: [
      { id: uid(), text: "Preheat oven to 400 degrees and line a baking sheet with parchment paper, foil, or coat with oil. Combine all ingredients except the teriyaki sauce using your hands or a rubber spatula." },
      { id: uid(), text: "Form the mixture into small balls (should make about 16-18) and place on the prepared baking sheet. Bake for about 15 minutes until fully cooked through. Larger meatballs (12 in quantity) will take closer to 20 minutes to bake." },
      { id: uid(), text: "Toss in teriyaki sauce and serve over rice. Top with sesame seeds and scallions. Sauteed zucchini or roasted broccoli make a great veggie side." }
    ],
    createdAt: Date.now()
  },
  {
    id: uid(),
    title: "Beef & Broccoli",
    description: "",
    servings: "",
    prepTime: "",
    cookTime: "",
    tags: [],
    ingredients: [
      { id: uid(), amount: "8 oz", name: "broccoli florets" },
      { id: uid(), amount: "1 tbsp", name: "avocado oil" },
      { id: uid(), amount: "1 lb", name: "shaved beef or steak, cut into bite sized pieces" },
      { id: uid(), amount: "", name: "salt and pepper" },
      { id: uid(), amount: "1 tbsp", name: "sesame oil" },
      { id: uid(), amount: "", name: "sesame seeds, to serve, optional" },
      { id: uid(), amount: "", name: "rice, to serve" },
      { id: uid(), amount: "1/2 cup", name: "Sauce: Trader Joe's reduced sodium soy sauce" },
      { id: uid(), amount: "1/2 cup", name: "Sauce: water" },
      { id: uid(), amount: "1 tbsp", name: "Sauce: fresh ginger, peeled and grated" },
      { id: uid(), amount: "3 cloves", name: "Sauce: garlic, minced" },
      { id: uid(), amount: "1 tbsp", name: "Sauce: honey" },
      { id: uid(), amount: "1 tsp", name: "Sauce: chili onion crunch or sriracha" },
      { id: uid(), amount: "1 tbsp", name: "Sauce: tapioca flour or corn starch (or 2 tbsp all purpose flour)" }
    ],
    steps: [
      { id: uid(), text: "First make the sauce by whisking together all sauce ingredients until the flour has fully dissolved." },
      { id: uid(), text: "Heat avocado oil in a large, deep skillet over medium heat. Add the broccoli and 2 tablespoons water. Saute for about 5 minutes then set aside." },
      { id: uid(), text: "Rinse the pan and then add the sesame oil over medium high heat. Season the beef with salt and pepper. Add the beef and brown on each side for about 2 minutes (add 2 more minutes each side if using chicken)." },
      { id: uid(), text: "Turn heat to medium then add the sauce. Simmer until beef is cooked through then stir in the broccoli." },
      { id: uid(), text: "Serve over rice with sesame seeds to top." }
    ],
    createdAt: Date.now()
  },
  {
    id: uid(),
    title: "Fall Veggie Sheet Pan Dinner with Maple Mustard Sauce",
    description: "",
    servings: "",
    prepTime: "",
    cookTime: "30 min",
    tags: [],
    ingredients: [
      { id: uid(), amount: "12 oz", name: "italian chicken sausage or sausage of choice, cut into 1/4 inch slices" },
      { id: uid(), amount: "2 cups", name: "Brussels sprouts, roughly chopped" },
      { id: uid(), amount: "1 medium", name: "sweet potato, diced" },
      { id: uid(), amount: "1/2", name: "red onion, sliced" },
      { id: uid(), amount: "1/3 cup", name: "walnuts" },
      { id: uid(), amount: "2 tbsp", name: "olive oil" },
      { id: uid(), amount: "1/2 tsp", name: "kosher salt, plus more to taste" },
      { id: uid(), amount: "1/2 tsp", name: "garlic powder" },
      { id: uid(), amount: "1/4 tsp", name: "black pepper, plus more to taste" },
      { id: uid(), amount: "", name: "parmesan cheese, to top, optional" },
      { id: uid(), amount: "", name: "generous drizzle of Maple Mustard Sauce" },
      { id: uid(), amount: "", name: "steamed quinoa, to serve" },
      { id: uid(), amount: "2 tbsp", name: "Maple Mustard Sauce: olive oil" },
      { id: uid(), amount: "2 tbsp", name: "Maple Mustard Sauce: Dijon mustard" },
      { id: uid(), amount: "2 tbsp", name: "Maple Mustard Sauce: pure maple syrup" },
      { id: uid(), amount: "1 clove", name: "Maple Mustard Sauce: garlic, minced" },
      { id: uid(), amount: "1/4 tsp", name: "Maple Mustard Sauce: red chili flakes" },
      { id: uid(), amount: "1/4 tsp", name: "Maple Mustard Sauce: kosher salt" },
      { id: uid(), amount: "1/4 tsp", name: "Maple Mustard Sauce: black pepper" },
      { id: uid(), amount: "1-2 tbsp", name: "Maple Mustard Sauce: water, optional" }
    ],
    steps: [
      { id: uid(), text: "Preheat oven to 425 degrees. On a large sheet pan, toss the chicken sausage, Brussels sprouts, sweet potato, red onion, and walnuts in the olive oil, salt, garlic powder, and pepper until evenly coated. To ensure everything gets crispy, spread the ingredients out as evenly as possible on the sheet pan. Bake for about 30 minutes or until the sweet potato is tender when pierced with a fork." },
      { id: uid(), text: "While it bakes, make the Maple Mustard Sauce. Whisk together all sauce ingredients and add 1-2 tablespoons water to thin out the sauce if needed." },
      { id: uid(), text: "Serve on a bed of quinoa with a generous drizzle of sauce and some parmesan cheese to top." }
    ],
    createdAt: Date.now()
  }
];

function loadRecipes() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    const saved = raw ? JSON.parse(raw) : [];
    const savedTitles = new Set(saved.map((r) => r.title.trim().toLowerCase()));
    const missingDefaults = defaultRecipes.filter((r) => !savedTitles.has(r.title.trim().toLowerCase()));
    recipes = [...saved, ...missingDefaults];
    if (missingDefaults.length > 0 || !raw) saveRecipes();
  } catch (e) {
    recipes = defaultRecipes;
  }
}

function saveRecipes() {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(recipes));
  } catch (e) {
    console.error("Could not save recipes", e);
  }
}

function escapeHtml(str) {
  return (str || "").replace(/[&<>"']/g, (c) => ({
    "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;",
  }[c]));
}

function getFiltered() {
  const q = search.trim().toLowerCase();
  let list = recipes;
  if (q) {
    list = list.filter((r) =>
      r.title.toLowerCase().includes(q) ||
      (r.tags || []).some((t) => t.toLowerCase().includes(q))
    );
  }
  return [...list].sort((a, b) => a.title.localeCompare(b.title));
}

function getGrouped() {
  const g = {};
  getFiltered().forEach((r) => {
    const letter = (r.title[0] || "#").toUpperCase();
    if (!g[letter]) g[letter] = [];
    g[letter].push(r);
  });
  return g;
}

function renderSidebar() {
  const listEl = document.getElementById("recipe-list");
  const grouped = getGrouped();
  const letters = Object.keys(grouped).sort();

  if (letters.length === 0) {
    listEl.innerHTML = `<p class="empty-note">Empty box. Add your first recipe and it'll show up here, filed by letter.</p>`;
    return;
  }

  listEl.innerHTML = letters.map((letter) => `
    <div class="letter-group">
      <div class="letter-label">${letter}</div>
      <ul style="list-style:none;margin:0;padding:0;">
        ${grouped[letter].map((r) => `
          <li>
            <button class="recipe-item ${selectedId === r.id && mode !== "form" ? "active" : ""}" data-id="${r.id}">
              ${escapeHtml(r.title)}
            </button>
          </li>
        `).join("")}
      </ul>
    </div>
  `).join("");

  listEl.querySelectorAll(".recipe-item").forEach((btn) => {
    btn.addEventListener("click", () => {
      const r = recipes.find((x) => x.id === btn.dataset.id);
      if (r) {
        selectedId = r.id;
        mode = "view";
        confirmingDelete = null;
        renderAll();
      }
    });
  });
}

function renderEmptyState() {
  return `
    <div class="empty-state">
      <svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
      <h2>Pick a card, or write a new one</h2>
      <p>Every recipe lives here as one clean card. No ads, no autoplay video, no story about someone's grandmother before you get to the ingredients.</p>
    </div>
  `;
}

function renderCard(recipe) {
  const ingredientsHtml = recipe.ingredients.length ? `
    <div class="section-label">INGREDIENTS</div>
    <ul class="ingredients-list">
      ${recipe.ingredients.map((ing) => `
        <li><span class="ing-amount">${escapeHtml(ing.amount)}</span><span class="ing-name">${escapeHtml(ing.name)}</span></li>
      `).join("")}
    </ul>
  ` : "";

  const stepsHtml = recipe.steps.length ? `
    <div class="section-label">METHOD</div>
    <ol class="steps-list">
      ${recipe.steps.map((s, idx) => `
        <li><span class="step-num">${String(idx + 1).padStart(2, "0")}</span><span>${escapeHtml(s.text)}</span></li>
      `).join("")}
    </ol>
  ` : "";

  const tagsHtml = (recipe.tags && recipe.tags.length) ? `
    <div class="tags">${recipe.tags.map((t) => `<span class="tag">${escapeHtml(t)}</span>`).join("")}</div>
  ` : "";

  const metaParts = [];
  if (recipe.servings) metaParts.push(`<span>${svgUsers()} ${escapeHtml(recipe.servings)} servings</span>`);
  if (recipe.prepTime) metaParts.push(`<span>${svgClock()} prep ${escapeHtml(recipe.prepTime)}</span>`);
  if (recipe.cookTime) metaParts.push(`<span>${svgClock()} cook ${escapeHtml(recipe.cookTime)}</span>`);

  return `
    <div class="card-column">
      <div class="toolbar">
        <button class="tbtn" id="edit-btn">${svgPencil()} Edit</button>
        ${confirmingDelete === recipe.id ? `
          <div class="confirm-row">
            <span>Delete this card?</span>
            <button class="confirm-yes" id="confirm-yes">Yes</button>
            <button class="confirm-no" id="confirm-no">No</button>
          </div>
        ` : `<button class="tbtn danger" id="delete-btn">${svgTrash()} Delete</button>`}
      </div>
      <div class="card">
        <div class="holes"><span class="hole"></span><span class="hole"></span><span class="hole"></span></div>
        <div class="margin-line"></div>
        <h1>${escapeHtml(recipe.title)}</h1>
        ${recipe.description ? `<p class="desc">${escapeHtml(recipe.description)}</p>` : ""}
        <div class="rule"></div>
        ${metaParts.length ? `<div class="meta-row">${metaParts.join("")}</div>` : ""}
        ${tagsHtml}
        ${ingredientsHtml}
        ${stepsHtml}
      </div>
    </div>
  `;
}

function renderForm() {
  const isNew = !form.id;
  return `
    <div class="card-column">
      <div class="toolbar split">
        <button class="tbtn back" id="cancel-btn">${svgArrowLeft()} Back</button>
        <button class="tbtn save" id="save-btn">${svgSave()} Save card</button>
      </div>
      <div class="card">
        <div class="holes"><span class="hole"></span><span class="hole"></span><span class="hole"></span></div>
        <div class="margin-line"></div>
        <input class="title-input" id="f-title" placeholder="${isNew ? "Untitled recipe" : "Recipe title"}" value="${escapeHtml(form.title)}" />
        <input class="desc-input" id="f-description" placeholder="A one-line note about this dish" value="${escapeHtml(form.description)}" />
        <div class="rule"></div>
        <div class="grid3">
          <div><label class="field-label">SERVINGS</label><input class="field-input" id="f-servings" placeholder="4" value="${escapeHtml(form.servings)}" /></div>
          <div><label class="field-label">PREP TIME</label><input class="field-input" id="f-prepTime" placeholder="15 min" value="${escapeHtml(form.prepTime)}" /></div>
          <div><label class="field-label">COOK TIME</label><input class="field-input" id="f-cookTime" placeholder="30 min" value="${escapeHtml(form.cookTime)}" /></div>
        </div>
        <div class="form-block">
          <label class="field-label">TAGS (comma separated)</label>
          <input class="field-input" id="f-tags" placeholder="weeknight, vegetarian, soup" value="${escapeHtml(form.tags)}" />
        </div>
        <div class="form-block">
          <div class="section-label">INGREDIENTS</div>
          <div id="ingredients-rows">
            ${form.ingredients.map((ing) => `
              <div class="ing-row" data-id="${ing.id}">
                <input class="ing-amount-input" data-role="amount" placeholder="2 cups" value="${escapeHtml(ing.amount)}" />
                <input class="ing-name-input" data-role="name" placeholder="all-purpose flour" value="${escapeHtml(ing.name)}" />
                <button class="row-remove" data-remove-ing="${ing.id}">${svgX()}</button>
              </div>
            `).join("")}
          </div>
          <button class="add-link" id="add-ingredient">${svgPlus()} Add ingredient</button>
        </div>
        <div class="form-block">
          <div class="section-label">METHOD</div>
          <div id="steps-rows">
            ${form.steps.map((s, idx) => `
              <div class="step-row" data-id="${s.id}">
                <span class="step-num-form">${String(idx + 1).padStart(2, "0")}</span>
                <textarea class="step-text" rows="2" placeholder="Describe this step">${escapeHtml(s.text)}</textarea>
                <button class="row-remove" data-remove-step="${s.id}" style="padding-top:6px;">${svgX()}</button>
              </div>
            `).join("")}
          </div>
          <button class="add-link" id="add-step">${svgPlus()} Add step</button>
        </div>
      </div>
    </div>
  `;
}

function renderMain() {
  const mainEl = document.getElementById("main");
  if (mode === "empty") {
    mainEl.innerHTML = renderEmptyState();
  } else if (mode === "view") {
    const recipe = recipes.find((r) => r.id === selectedId);
    if (!recipe) { mode = "empty"; mainEl.innerHTML = renderEmptyState(); return; }
    mainEl.innerHTML = renderCard(recipe);
    attachViewHandlers(recipe);
  } else if (mode === "form") {
    mainEl.innerHTML = renderForm();
    attachFormHandlers();
  }
}

function attachViewHandlers(recipe) {
  const editBtn = document.getElementById("edit-btn");
  if (editBtn) editBtn.addEventListener("click", () => openEdit(recipe));
  const deleteBtn = document.getElementById("delete-btn");
  if (deleteBtn) deleteBtn.addEventListener("click", () => { confirmingDelete = recipe.id; renderAll(); });
  const yesBtn = document.getElementById("confirm-yes");
  if (yesBtn) yesBtn.addEventListener("click", () => deleteRecipe(recipe.id));
  const noBtn = document.getElementById("confirm-no");
  if (noBtn) noBtn.addEventListener("click", () => { confirmingDelete = null; renderAll(); });
}

function attachFormHandlers() {
  document.getElementById("f-title").addEventListener("input", (e) => form.title = e.target.value);
  document.getElementById("f-description").addEventListener("input", (e) => form.description = e.target.value);
  document.getElementById("f-servings").addEventListener("input", (e) => form.servings = e.target.value);
  document.getElementById("f-prepTime").addEventListener("input", (e) => form.prepTime = e.target.value);
  document.getElementById("f-cookTime").addEventListener("input", (e) => form.cookTime = e.target.value);
  document.getElementById("f-tags").addEventListener("input", (e) => form.tags = e.target.value);

  document.querySelectorAll("#ingredients-rows .ing-row").forEach((row) => {
    const id = row.dataset.id;
    row.querySelector('[data-role="amount"]').addEventListener("input", (e) => {
      const ing = form.ingredients.find((i) => i.id === id);
      if (ing) ing.amount = e.target.value;
    });
    row.querySelector('[data-role="name"]').addEventListener("input", (e) => {
      const ing = form.ingredients.find((i) => i.id === id);
      if (ing) ing.name = e.target.value;
    });
  });
  document.querySelectorAll("[data-remove-ing]").forEach((btn) => {
    btn.addEventListener("click", () => {
      form.ingredients = form.ingredients.filter((i) => i.id !== btn.dataset.removeIng);
      renderMain();
    });
  });
  document.getElementById("add-ingredient").addEventListener("click", () => {
    form.ingredients.push({ id: uid(), amount: "", name: "" });
    renderMain();
  });

  document.querySelectorAll("#steps-rows .step-row").forEach((row) => {
    const id = row.dataset.id;
    row.querySelector(".step-text").addEventListener("input", (e) => {
      const s = form.steps.find((x) => x.id === id);
      if (s) s.text = e.target.value;
    });
  });
  document.querySelectorAll("[data-remove-step]").forEach((btn) => {
    btn.addEventListener("click", () => {
      form.steps = form.steps.filter((s) => s.id !== btn.dataset.removeStep);
      renderMain();
    });
  });
  document.getElementById("add-step").addEventListener("click", () => {
    form.steps.push({ id: uid(), text: "" });
    renderMain();
  });

  document.getElementById("save-btn").addEventListener("click", saveRecipe);
  document.getElementById("cancel-btn").addEventListener("click", () => {
    mode = selectedId ? "view" : "empty";
    renderAll();
  });
}

function openNew() {
  form = emptyForm();
  mode = "form";
  selectedId = null;
  renderAll();
}

function openEdit(recipe) {
  form = {
    ...recipe,
    tags: (recipe.tags || []).join(", "),
    ingredients: recipe.ingredients.length ? recipe.ingredients.map(i => ({...i})) : [{ id: uid(), amount: "", name: "" }],
    steps: recipe.steps.length ? recipe.steps.map(s => ({...s})) : [{ id: uid(), text: "" }],
  };
  mode = "form";
  renderAll();
}

function saveRecipe() {
  if (!form.title.trim()) return;
  const cleanTags = form.tags.split(",").map((t) => t.trim()).filter(Boolean);
  const cleanIngredients = form.ingredients.filter((i) => i.name.trim());
  const cleanSteps = form.steps.filter((s) => s.text.trim());

  if (form.id) {
    recipes = recipes.map((r) => r.id === form.id
      ? { ...form, tags: cleanTags, ingredients: cleanIngredients, steps: cleanSteps }
      : r
    );
    selectedId = form.id;
  } else {
    const newRecipe = {
      ...form, id: uid(), tags: cleanTags, ingredients: cleanIngredients, steps: cleanSteps, createdAt: Date.now(),
    };
    recipes.push(newRecipe);
    selectedId = newRecipe.id;
  }
  saveRecipes();
  mode = "view";
  renderAll();
}

function deleteRecipe(id) {
  recipes = recipes.filter((r) => r.id !== id);
  saveRecipes();
  confirmingDelete = null;
  selectedId = null;
  mode = "empty";
  renderAll();
}

function renderAll() {
  renderSidebar();
  renderMain();
}

// icons
function svgUsers() { return `<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="vertical-align:-2px"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 0 0-3-3.87"/><path d="M16 3.13a4 4 0 0 1 0 7.75"/></svg>`; }
function svgClock() { return `<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="vertical-align:-2px"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>`; }
function svgPencil() { return `<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="vertical-align:-2px"><path d="M17 3a2.85 2.83 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5Z"/></svg>`; }
function svgTrash() { return `<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="vertical-align:-2px"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>`; }
function svgArrowLeft() { return `<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="vertical-align:-2px"><path d="M19 12H5M12 19l-7-7 7-7"/></svg>`; }
function svgSave() { return `<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="vertical-align:-2px"><path d="M19 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11l5 5v11a2 2 0 0 1-2 2Z"/><polyline points="17 21 17 13 7 13 7 21"/><polyline points="7 3 7 8 15 8"/></svg>`; }
function svgX() { return `<svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 6 6 18M6 6l12 12"/></svg>`; }
function svgPlus() { return `<svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" style="vertical-align:-2px"><path d="M5 12h14M12 5v14"/></svg>`; }

document.getElementById("new-btn").addEventListener("click", openNew);
document.getElementById("search").addEventListener("input", (e) => {
  search = e.target.value;
  renderSidebar();
});

loadRecipes();
renderAll();
</script>
</body>
</html>
