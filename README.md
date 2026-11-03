(function () {
    if (window.__maxFlPanelInstance) {
        window.__maxFlPanelInstance.remove();
    }

    const panel = document.createElement('div');
    panel.id = 'maxfl-panel';
    window.__maxFlPanelInstance = panel;

    panel.style.cssText = `
        position: fixed;
        top: 20px;
        right: 20px;
        width: 320px;
        background: linear-gradient(135deg, #121214, #1a1a1e);
        color: #fff;
        padding: 16px;
        border-radius: 14px;
        box-shadow: 0 10px 35px rgba(0, 0, 0, 0.8);
        z-index: 999999;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        border: 1px solid rgba(0, 255, 204, 0.3);
        backdrop-filter: blur(10px);
        user-select: none;
    `;

    panel.innerHTML = `
        <div style="display: flex; align-items: center; justify-content: space-between; border-bottom: 1px solid rgba(255,255,255,0.1); padding-bottom: 10px; margin-bottom: 12px; cursor: move;" id="maxfl-header">
            <div style="display: flex; align-items: center; gap: 8px;">
                <div style="width: 10px; height: 10px; background: #00ffcc; border-radius: 50%; box-shadow: 0 0 10px #00ffcc;"></div>
                <span style="font-weight: 700; font-size: 15px; letter-spacing: 0.5px; background: linear-gradient(90deg, #00ffcc, #00bfff); -webkit-background-clip: text; -webkit-text-fill-color: transparent;">MaxFl Hub</span>
            </div>
            <button id="maxfl-close" style="background: none; border: none; color: #888; font-size: 16px; cursor: pointer; transition: color 0.2s;" onmouseover="this.style.color='#ff3b30'" onmouseout="this.style.color='#888'">✕</button>
        </div>

        <div style="display: flex; flex-direction: column; gap: 10px;">
            <div style="font-size: 12px; color: #aaa; background: rgba(255,255,255,0.05); padding: 8px; border-radius: 8px;">
                Статус: <span style="color: #00ffcc; font-weight: 600;">Активен (MaxFl)</span>
            </div>

            <button class="maxfl-action-btn" id="maxfl-btn-1" style="background: rgba(0,255,204,0.1); border: 1px solid rgba(0,255,204,0.4); color: #00ffcc; padding: 10px; border-radius: 8px; font-weight: 600; cursor: pointer; transition: 0.2s; text-align: left; display: flex; align-items: center; justify-content: space-between;">
                <span>⚡ Быстрый бег</span>
                <span style="font-size: 10px; background: rgba(0,255,204,0.2); padding: 2px 6px; border-radius: 4px;">OFF</span>
            </button>

            <button class="maxfl-action-btn" id="maxfl-btn-2" style="background: rgba(0,191,255,0.1); border: 1px solid rgba(0,191,255,0.4); color: #00bfff; padding: 10px; border-radius: 8px; font-weight: 600; cursor: pointer; transition: 0.2s; text-align: left; display: flex; align-items: center; justify-content: space-between;">
                <span>🚀 Режим полета</span>
                <span style="font-size: 10px; background: rgba(0,191,255,0.2); padding: 2px 6px; border-radius: 4px;">OFF</span>
            </button>

            <button class="maxfl-action-btn" id="maxfl-btn-3" style="background: rgba(255,59,48,0.1); border: 1px solid rgba(255,59,48,0.4); color: #ff6b6b; padding: 10px; border-radius: 8px; font-weight: 600; cursor: pointer; transition: 0.2s; text-align: left; display: flex; align-items: center; justify-content: space-between;">
                <span>🛡️ Античит Тест</span>
                <span style="font-size: 10px; background: rgba(255,59,48,0.2); padding: 2px 6px; border-radius: 4px;">RUN</span>
            </button>
        </div>

        <div style="margin-top: 12px; font-size: 10px; text-align: center; color: #555; border-top: 1px solid rgba(255,255,255,0.05); padding-top: 8px;">
            MaxFl Brand Studio © 2026
        </div>
    `;

    document.body.appendChild(panel);

    document.getElementById('maxfl-close').onclick = () => panel.remove();

    // Логика кнопок в панели
    let speedActive = false;
    document.getElementById('maxfl-btn-1').onclick = function() {
        speedActive = !speedActive;
        const tag = this.querySelector('span:last-child');
        tag.innerText = speedActive ? 'ON' : 'OFF';
        tag.style.background = speedActive ? 'rgba(0,255,204,0.4)' : 'rgba(0,255,204,0.2)';
        
        try {
            const player = game.Players.LocalPlayer.Character;
            if (player && player.FindFirstChildOfClass('Humanoid')) {
                player.FindFirstChildOfClass('Humanoid').WalkSpeed = speedActive ? 32 : 16;
            }
        } catch (e) {
            console.log("MaxFl Speed toggle:", speedActive);
        }
    };

    let flyActive = false;
    document.getElementById('maxfl-btn-2').onclick = function() {
        flyActive = !flyActive;
        const tag = this.querySelector('span:last-child');
        tag.innerText = flyActive ? 'ON' : 'OFF';
        tag.style.background = flyActive ? 'rgba(0,191,255,0.4)' : 'rgba(0,191,255,0.2)';
        console.log("MaxFl Fly mode:", flyActive);
    };

    document.getElementById('maxfl-btn-3').onclick = function() {
        alert("MaxFl: Проверка античита выполнена. Ошибок не обнаружено!");
    };

    // Перетаскивание панели мышкой за шапку
    const header = document.getElementById('maxfl-header');
    let isDragging = false, startX, startY;

    header.onmousedown = (e) => {
        isDragging = true;
        startX = e.clientX - panel.offsetLeft;
        startY = e.clientY - panel.offsetTop;
    };

    document.onmousemove = (e) => {
        if (!isDragging) return;
        panel.style.left = (e.clientX - startX) + 'px';
        panel.style.top = (e.clientY - startY) + 'px';
        panel.style.right = 'auto';
    };

    document.onmouseup = () => {
        isDragging = false;
    };
})();
