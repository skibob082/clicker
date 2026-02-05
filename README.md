# 🎮 Кликер Игра

## Статистика игры

<div align="center">
    <h2>💰 Баллы: <span id="points">0</span></h2>
    <button id="clickButton" style="padding: 20px 40px; font-size: 24px; margin: 20px; background: linear-gradient(45deg, #4CAF50, #45a049); color: white; border: none; border-radius: 10px; cursor: pointer; transition: all 0.2s;">
        🖱️ Нажми меня!
    </button>
    
    <div style="margin: 20px;">
        <h3>⚡ Баллов за клик: <span id="pointsPerClick">1</span></h3>
        <h3>🔄 Автокликов в секунду: <span id="autoClickers">0</span></h3>
    </div>
</div>

---

## 🛒 Магазин улучшений

### Улучшение клика
<button id="upgradeClick" style="padding: 10px 20px; margin: 5px; background: #2196F3; color: white; border: none; border-radius: 5px; cursor: pointer;">
    Улучшить клик (+1 за клик)<br>
    <small>Стоимость: <span id="upgradeClickCost">10</span> баллов</small>
</button>

### Автокликеры
<button id="buyAutoClicker" style="padding: 10px 20px; margin: 5px; background: #FF9800; color: white; border: none; border-radius: 5px; cursor: pointer;">
    Купить автокликер (+1/сек)<br>
    <small>Стоимость: <span id="autoClickerCost">50</span> баллов</small>
</button>

<button id="upgradeAutoClicker" style="padding: 10px 20px; margin: 5px; background: #9C27B0; color: white; border: none; border-radius: 5px; cursor: pointer;">
    Улучшить автокликеры (x2 эффективность)<br>
    <small>Стоимость: <span id="upgradeAutoClickerCost">200</span> баллов</small>
</button>

---

## 🎯 Достижения
<div id="achievements" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 10px; margin-top: 20px;">
    <div style="background: #f0f0f0; padding: 10px; border-radius: 5px; text-align: center;">🏆 Первые 100 баллов</div>
    <div style="background: #f0f0f0; padding: 10px; border-radius: 5px; text-align: center;">🏆 10 автокликеров</div>
    <div style="background: #f0f0f0; padding: 10px; border-radius: 5px; text-align: center;">🏆 1000 баллов</div>
</div>

---

## 📊 Сохранение и сброс
<button id="saveGame" style="padding: 10px 20px; margin: 10px; background: #4CAF50; color: white; border: none; border-radius: 5px; cursor: pointer;">
    💾 Сохранить игру
</button>
<button id="resetGame" style="padding: 10px 20px; margin: 10px; background: #f44336; color: white; border: none; border-radius: 5px; cursor: pointer;">
    🔄 Сбросить игру
</button>
<div style="margin-top: 10px; color: #666; font-size: 12px;">
    Игра автоматически сохраняется каждые 30 секунд
</div>

<script>
// Игровые переменные
let points = 0;
let pointsPerClick = 1;
let autoClickers = 0;
let autoClickerPower = 1;
let upgradeClickCost = 10;
let autoClickerCost = 50;
let upgradeAutoClickerCost = 200;
let lastSaveTime = Date.now();

// Элементы DOM
const pointsElement = document.getElementById('points');
const pointsPerClickElement = document.getElementById('pointsPerClick');
const autoClickersElement = document.getElementById('autoClickers');
const clickButton = document.getElementById('clickButton');
const upgradeClickButton = document.getElementById('upgradeClick');
const buyAutoClickerButton = document.getElementById('buyAutoClicker');
const upgradeAutoClickerButton = document.getElementById('upgradeAutoClicker');

// Загрузка сохраненной игры
function loadGame() {
    const saved = localStorage.getItem('clickerGame');
    if (saved) {
        const gameData = JSON.parse(saved);
        points = gameData.points || 0;
        pointsPerClick = gameData.pointsPerClick || 1;
        autoClickers = gameData.autoClickers || 0;
        autoClickerPower = gameData.autoClickerPower || 1;
        upgradeClickCost = gameData.upgradeClickCost || 10;
        autoClickerCost = gameData.autoClickerCost || 50;
        upgradeAutoClickerCost = gameData.upgradeAutoClickerCost || 200;
        
        updateUI();
        console.log('Игра загружена!');
    }
}

// Сохранение игры
function saveGame() {
    const gameData = {
        points,
        pointsPerClick,
        autoClickers,
        autoClickerPower,
        upgradeClickCost,
        autoClickerCost,
        upgradeAutoClickerCost,
        saveTime: Date.now()
    };
    localStorage.setItem('clickerGame', JSON.stringify(gameData));
    console.log('Игра сохранена!');
}

// Обновление интерфейса
function updateUI() {
    pointsElement.textContent = points.toFixed(0);
    pointsPerClickElement.textContent = pointsPerClick;
    autoClickersElement.textContent = autoClickers;
    
    document.getElementById('upgradeClickCost').textContent = upgradeClickCost;
    document.getElementById('autoClickerCost').textContent = autoClickerCost;
    document.getElementById('upgradeAutoClickerCost').textContent = upgradeAutoClickerCost;
    
    // Обновляем состояние кнопок
    upgradeClickButton.disabled = points < upgradeClickCost;
    buyAutoClickerButton.disabled = points < autoClickerCost;
    upgradeAutoClickerButton.disabled = points < upgradeAutoClickerCost || autoClickers === 0;
}

// Клик по основной кнопке
clickButton.addEventListener('click', function() {
    points += pointsPerClick;
    updateUI();
    
    // Анимация клика
    this.style.transform = 'scale(0.95)';
    setTimeout(() => {
        this.style.transform = 'scale(1)';
    }, 100);
});

// Покупка улучшения клика
upgradeClickButton.addEventListener('click', function() {
    if (points >= upgradeClickCost) {
        points -= upgradeClickCost;
        pointsPerClick += 1;
        upgradeClickCost = Math.floor(upgradeClickCost * 1.5);
        updateUI();
        saveGame();
    }
});

// Покупка автокликера
buyAutoClickerButton.addEventListener('click', function() {
    if (points >= autoClickerCost) {
        points -= autoClickerCost;
        autoClickers += 1;
        autoClickerCost = Math.floor(autoClickerCost * 1.3);
        updateUI();
        saveGame();
    }
});

// Улучшение автокликеров
upgradeAutoClickerButton.addEventListener('click', function() {
    if (points >= upgradeAutoClickerCost && autoClickers > 0) {
        points -= upgradeAutoClickerCost;
        autoClickerPower *= 2;
        upgradeAutoClickerCost = Math.floor(upgradeAutoClickerCost * 2);
        updateUI();
        saveGame();
    }
});

// Автокликер система
setInterval(function() {
    if (autoClickers > 0) {
        points += autoClickers * autoClickerPower;
        updateUI();
    }
}, 1000);

// Автосохранение каждые 30 секунд
setInterval(function() {
    saveGame();
    console.log('Автосохранение...');
}, 30000);

// Сброс игры
document.getElementById('resetGame').addEventListener('click', function() {
    if (confirm('Вы уверены, что хотите сбросить игру? Все данные будут потеряны!')) {
        localStorage.removeItem('clickerGame');
        location.reload();
    }
});

// Ручное сохранение
document.getElementById('saveGame').addEventListener('click', function() {
    saveGame();
    alert('Игра сохранена!');
});

// Инициализация игры
loadGame();

// Автосохранение при закрытии страницы
window.addEventListener('beforeunload', function() {
    saveGame();
});

// Анимация для кнопки клика
clickButton.addEventListener('mousedown', function() {
    this.style.boxShadow = 'inset 0 5px 10px rgba(0,0,0,0.2)';
});

clickButton.addEventListener('mouseup', function() {
    this.style.boxShadow = '0 5px 15px rgba(0,0,0,0.2)';
});

clickButton.addEventListener('mouseleave', function() {
    this.style.boxShadow = '0 5px 15px rgba(0,0,0,0.2)';
});

// Добавляем эффект частиц при клике
clickButton.addEventListener('click', function(e) {
    for(let i = 0; i < 5; i++) {
        createParticle(e.clientX, e.clientY);
    }
});

function createParticle(x, y) {
    const particle = document.createElement('div');
    particle.style.position = 'fixed';
    particle.style.left = x + 'px';
    particle.style.top = y + 'px';
    particle.style.width = '10px';
    particle.style.height = '10px';
    particle.style.backgroundColor = '#4CAF50';
    particle.style.borderRadius = '50%';
    particle.style.pointerEvents = 'none';
    particle.style.zIndex = '9999';
    
    document.body.appendChild(particle);
    
    // Анимация
    const animation = particle.animate([
        { transform: 'translate(0, 0) scale(1)', opacity: 1 },
        { transform: `translate(${Math.random() * 100 - 50}px, ${Math.random() * 100 - 50}px) scale(0)`, opacity: 0 }
    ], {
        duration: 500,
        easing: 'ease-out'
    });
    
    animation.onfinish = () => particle.remove();
}
</script>

<style>
button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

button:not(:disabled):hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(0,0,0,0.2);
    transition: all 0.3s ease;
}

#clickButton {
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}
</style>

## 🎮 Как играть:
1. **Кликайте по зеленой кнопке** для получения баллов
2. **Покупайте улучшения** в магазине:
   - Улучшение клика: увеличивает баллы за каждый клик
   - Автокликер: автоматически генерирует баллы каждую секунду
   - Улучшение автокликера: удваивает эффективность всех автокликеров
3. **Игра автоматически сохраняется** каждые 30 секунд
4. **Используйте кнопку сохранения** для принудительного сохранения
5. **Нажмите сброс** если хотите начать заново

**Совет:** Сначала купите несколько улучшений клика, затем инвестируйте в автокликеры!
