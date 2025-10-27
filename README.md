<script>
function waitForElements(selectors, callback) {
  const interval = setInterval(() => {
    const ready = selectors.every(sel => document.querySelector(sel));
    if (ready) {
      clearInterval(interval);
      callback();
    }
  }, 500);
}

document.addEventListener("DOMContentLoaded", function() {
  waitForElements(
    [
      "[data-field-class^='current-']",
      "[data-field-class^='desired-']",
      "[data-field-class='current-rank-img']",
      "[data-field-class='desired-rank-img']"
    ],
    initRankScript
  );
});

function initRankScript() {
  console.log("🎯 Элементы найдены, скрипт активирован!");

  const ranks = {
    iron: { name: "Железо", img: "ironpng" },
    bronze: { name: "Бронза", img: "bronzepng" },
    silver: { name: "Серебро", img: "silverpng" },
    gold: { name: "Золото", img: "goldpng" },
    platinum: { name: "Платина", img: "platinumpng" },
    emerald: { name: "Эмеральд", img: "emeraldpng" },
    diamond: { name: "Даймонд", img: "diamondpng" },
    master: { name: "Мастер", img: "masterpng" }
  };

  const currentMainImg = document.querySelector("[data-field-class='current-rank-img']");
  const desiredMainImg = document.querySelector("[data-field-class='desired-rank-img']");
  const orderCurrentImg = document.querySelector("[data-field-class='order-current-img']");
  const orderDesiredImg = document.querySelector("[data-field-class='order-desired-img']");
  const orderCurrentText = document.querySelector("[data-field-class='order-current-rank']");
  const orderDesiredText = document.querySelector("[data-field-class='order-desired-rank']");

  const currentIcons = document.querySelectorAll("[data-field-class^='current-']");
  const desiredIcons = document.querySelectorAll("[data-field-class^='desired-']");

  console.log("Найдено current:", currentIcons.length, "desired:", desiredIcons.length);

  let selectedCurrent = null;
  let selectedDesired = null;

  function updateOrder() {
    if (selectedCurrent && ranks[selectedCurrent]) {
      orderCurrentImg.src = ranks[selectedCurrent].img;
      orderCurrentText.textContent = ranks[selectedCurrent].name;
    }
    if (selectedDesired && ranks[selectedDesired]) {
      orderDesiredImg.src = ranks[selectedDesired].img;
      orderDesiredText.textContent = ranks[selectedDesired].name;
    }
  }

  currentIcons.forEach(icon => {
    const rank = icon.getAttribute("data-field-class").replace("current-", "");
    icon.style.cursor = "pointer";
    icon.addEventListener("click", () => {
      console.log("Клик по текущему:", rank);
      if (ranks[rank]) {
        selectedCurrent = rank;
        currentMainImg.src = ranks[rank].img;
        updateOrder();
      }
    });
  });

  desiredIcons.forEach(icon => {
    const rank = icon.getAttribute("data-field-class").replace("desired-", "");
    icon.style.cursor = "pointer";
    icon.addEventListener("click", () => {
      console.log("Клик по желаемому:", rank);
      if (ranks[rank]) {
        selectedDesired = rank;
        desiredMainImg.src = ranks[rank].img;
        updateOrder();
      }
    });
  });
}
</script>
