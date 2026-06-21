const prizes = [
  { name: "Kuning", icon: "🟡" },
  { name: "Biru", icon: "🔵" },
  { name: "Biru Tua", icon: "🔷" }
];

let coins = 100;
let adminActive = false;

const reel1 = document.getElementById("reel1");
const reel2 = document.getElementById("reel2");
const reel3 = document.getElementById("reel3");
const result = document.getElementById("result");
const spinBtn = document.getElementById("spinBtn");
const coinCount = document.getElementById("coinCount");
const adminBtn = document.getElementById("adminBtn");
const adminPanel = document.getElementById("adminPanel");
const adminStatus = document.getElementById("adminStatus");
const adminOnBtn = document.getElementById("adminOnBtn");
const adminOffBtn = document.getElementById("adminOffBtn");
const closeAdminBtn = document.getElementById("closeAdminBtn");
const setCoinBtn = document.getElementById("setCoinBtn");
const coinInput = document.getElementById("coinInput");
const vipItem = document.getElementById("vipItem");

function updateCoins() {
  coinCount.textContent = coins;
}

function updateAdminUI() {
  adminStatus.textContent = adminActive ? "Aktif" : "Nonaktif";
  vipItem.style.display = adminActive ? "block" : "none";
}

adminBtn.addEventListener("click", () => {
  adminPanel.style.display = "flex";
  updateAdminUI();
  coinInput.value = coins;
});

closeAdminBtn.addEventListener("click", () => {
  adminPanel.style.display = "none";
});

adminOnBtn.addEventListener("click", () => {
  adminActive = true;
  updateAdminUI();
  result.textContent = "Admin aktif. VIP Admin tersedia.";
});

adminOffBtn.addEventListener("click", () => {
  adminActive = false;
  updateAdminUI();
  result.textContent = "Admin nonaktif.";
});

setCoinBtn.addEventListener("click", () => {
  coins = parseInt(coinInput.value) || 0;
  updateCoins();
  result.textContent = `Koin diatur menjadi ${coins}.`;
});

spinBtn.addEventListener("click", () => {
  const spinCost = 10;

  if (coins < spinCost) {
    result.textContent = "Koin tidak cukup untuk spin.";
    return;
  }

  coins -= spinCost;
  updateCoins();
  spinBtn.disabled = true;
  result.textContent = "Memutar...";

  const currentPrizes = adminActive
    ? [...prizes, { name: "VIP", icon: "👑" }]
    : prizes;

  let count = 0;
  const spinAnimation = setInterval(() => {
    const r1 = currentPrizes[Math.floor(Math.random() * currentPrizes.length)];
    const r2 = currentPrizes[Math.floor(Math.random() * currentPrizes.length)];
    const r3 = currentPrizes[Math.floor(Math.random() * currentPrizes.length)];

    reel1.textContent = r1.icon;
    reel2.textContent = r2.icon;
    reel3.textContent = r3.icon;
    count++;

    if (count > 15) {
      clearInterval(spinAnimation);

      const f1 = currentPrizes[Math.floor(Math.random() * currentPrizes.length)];
      const f2 = currentPrizes[Math.floor(Math.random() * currentPrizes.length)];
      const f3 = currentPrizes[Math.floor(Math.random() * currentPrizes.length)];

      reel1.textContent = f1.icon;
      reel2.textContent = f2.icon;
      reel3.textContent = f3.icon;

      if (f1.name === f2.name && f2.name === f3.name) {
        result.textContent = `Menang! 3 gambar kembar: ${f1.name}`;
      } else {
        result.textContent = "Belum menang. Harus 3 gambar kembar.";
      }

      spinBtn.disabled = false;
    }
  }, 120);
});

updateCoins();
updateAdminUI();
