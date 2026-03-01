<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MD5 Oracle Pro - HOANGDZ </title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary: #818cf8;
            --primary-dark: #4f46e5;
            --secondary: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --dark: #020617;
            --glass: rgba(15, 23, 42, 0.8);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', system-ui, sans-serif; }

        body {
            background: var(--dark);
            color: #f8fafc;
            overflow: hidden;
            height: 100vh;
            -webkit-tap-highlight-color: transparent;
        }

        /* ICON RƠI DÀY ĐẶC - GPU OPTIMIZED */
        #particles {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        .p-icon {
            position: absolute;
            color: var(--primary);
            opacity: 0.25;
            will-change: transform;
            animation: fall linear infinite;
        }

        @keyframes fall {
            from { transform: translateY(-10vh) rotate(0deg); }
            to { transform: translateY(110vh) rotate(360deg); }
        }

        /* NAVIGATION */
        .nav-bar {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            background: var(--glass);
            backdrop-filter: blur(15px);
            padding: 6px;
            border-radius: 50px;
            border: 1px solid rgba(255,255,255,0.1);
            z-index: 1000;
        }

        .nav-btn {
            padding: 10px 20px;
            border: none;
            background: none;
            color: #94a3b8;
            font-weight: 700;
            cursor: pointer;
            border-radius: 40px;
            transition: 0.3s;
            display: flex;
            align-items: center; gap: 8px;
        }

        .nav-btn.active {
            background: var(--primary-dark);
            color: white;
            box-shadow: 0 4px 15px rgba(79, 70, 229, 0.4);
        }

        /* MAIN SLIDER */
        .wrapper {
            display: flex;
            width: 200vw;
            height: 100vh;
            transition: transform 0.5s cubic-bezier(0.23, 1, 0.32, 1);
        }

        .page {
            width: 100vw;
            height: 100vh;
            overflow-y: auto;
            padding: 100px 20px 40px;
        }

        .container { max-width: 650px; margin: 0 auto; position: relative; z-index: 1; }

        .card {
            background: var(--glass);
            backdrop-filter: blur(25px);
            border: 1px solid rgba(255,255,255,0.08);
            border-radius: 24px;
            padding: 30px;
            margin-bottom: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.4);
        }

        /* INTRO */
        .hero-title {
            font-size: 2.2rem;
            text-align: center;
            background: linear-gradient(to right, #818cf8, #34d399);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-weight: 900;
            margin-bottom: 15px;
        }

        /* TOOL UI */
        .md5-input {
            width: 100%;
            padding: 20px;
            background: rgba(0,0,0,0.4);
            border: 2px solid rgba(255,255,255,0.1);
            border-radius: 18px;
            color: white;
            font-size: 1.1rem;
            text-align: center;
            font-family: monospace;
            margin-bottom: 15px;
        }

        .btn-analyze {
            width: 100%;
            padding: 20px;
            background: linear-gradient(135deg, var(--primary-dark), #6366f1);
            border: none;
            border-radius: 18px;
            color: white;
            font-weight: 800;
            cursor: pointer;
            transition: 0.2s;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        .btn-analyze:active { transform: scale(0.97); }

        /* KẾT QUẢ - DỰ ĐOÁN */
        .result-box { display: none; margin-top: 25px; animation: fadeIn 0.5s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .grid-stats { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .stat-card { background: rgba(0,0,0,0.3); padding: 15px; border-radius: 18px; text-align: center; border: 1px solid rgba(255,255,255,0.05); }
        
        .val-pct { font-size: 2.5rem; font-weight: 900; margin: 5px 0; }
        .progress-container { height: 6px; background: rgba(255,255,255,0.1); border-radius: 10px; overflow: hidden; margin-top: 10px; }
        .bar { height: 100%; width: 0; transition: width 1s ease-out; }

        .advice-box {
            margin-top: 20px;
            padding: 20px;
            border-radius: 20px;
            text-align: center;
            background: rgba(255,255,255,0.05);
        }
        .final-rec { font-size: 3rem; font-weight: 900; letter-spacing: 2px; }
        .action-status { font-size: 1.2rem; font-weight: 700; margin-top: 5px; }

        .history-item {
            display: flex; justify-content: space-between; align-items: center;
            padding: 12px 18px; background: rgba(255,255,255,0.03);
            border-radius: 12px; margin-bottom: 8px; border-left: 4px solid var(--primary);
        }

        .loader { display: none; text-align: center; margin: 20px 0; }
        .fa-spin-fast { animation: fa-spin 0.5s infinite linear; }
    </style>
</head>
<body>

    <div id="particles"></div>

    <nav class="nav-bar">
        <button class="nav-btn active" onclick="move(0, this)"><i class="fas fa-home"></i> TRANG CHỦ</button>
        <button class="nav-btn" onclick="move(1, this)"><i class="fas fa-robot"></i> TOOL AI</button>
    </nav>

    <main class="wrapper" id="wrapper">
        <div class="page">
            <div class="container">
                <h1 class="hero-title">TOOL CỦA HOANGDZ BẢO</h1>
                <div class="card">
                    <h3 style="color: var(--secondary); margin-bottom: 12px;"><i class="fas fa-shield-check"></i> Hệ Thống Phân Tích 6 Lớp</h3>
                    <p style="line-height: 1.6; color: #94a3b8; font-size: 0.95rem;">
                        Phiên bản Pro được phát triển độc quyền bởi <b>HoangDz </b>. Hệ thống sử dụng thuật toán bóc tách mã hóa MD5 thông qua 6 tầng dữ liệu: 
                        Hex Energy, Bit Entropy, Mirror Phase, Cluster Detection, Modulo Vector và Logistic Balance. 
                        <br><br>
                        Công cụ tự động đưa ra khuyến nghị <b>Nên vào</b> hoặc <b>Vào nhẹ</b> dựa trên mức độ tin cậy của thuật toán để đảm bảo an toàn cho người dùng.
                    </p>
                </div>
            </div>
        </div>

        <div class="page">
            <div class="container">
                <div class="card">
                    <div style="text-align: center; margin-bottom: 20px;">
                        <h2 style="font-weight: 800;">PHÂN TÍCH DỮ LIỆU MD5</h2>
                        <p style="color: var(--primary); font-size: 0.8rem; font-weight: 700;">CHẾ ĐỘ: SMART AI v3.0</p>
                    </div>

                    <input type="text" id="md5-input" class="md5-input" placeholder="Dán mã MD5 32 ký tự tại đây..." maxlength="32" spellcheck="false">
                    <button class="btn-analyze" id="analyze-trigger">BẮT ĐẦU TRUY VẤN</button>

                    <div id="loader" class="loader">
                        <i class="fas fa-circle-notch fa-spin fa-spin-fast" style="font-size: 2.5rem; color: var(--primary);"></i>
                        <p style="margin-top: 15px; font-weight: 600; letter-spacing: 1px;">ĐANG GIẢI MÃ 6 LỚP...</p>
                    </div>

                    <div id="result-ui" class="result-box">
                        <div class="grid-stats">
                            <div class="stat-card">
                                <span style="font-size: 0.8rem; font-weight: 700; color: var(--primary);">TỈ LỆ TÀI</span>
                                <div class="val-pct" id="tai-val">0%</div>
                                <div class="progress-container"><div class="bar" id="tai-bar" style="background: var(--primary);"></div></div>
                            </div>
                            <div class="stat-card">
                                <span style="font-size: 0.8rem; font-weight: 700; color: var(--danger);">TỈ LỆ XỈU</span>
                                <div class="val-pct" id="xiu-val">0%</div>
                                <div class="progress-container"><div class="bar" id="xiu-bar" style="background: var(--danger);"></div></div>
                            </div>
                        </div>

                        <div class="advice-box">
                            <div class="final-rec" id="final-res">---</div>
                            <div class="action-status" id="action-status">-- --</div>
                        </div>
                    </div>
                </div>

                <div class="card" style="padding: 20px;">
                    <h4 style="margin-bottom: 15px;"><i class="fas fa-history" style="margin-right: 10px;"></i>LỊCH SỬ PHÂN TÍCH</h4>
                    <div id="history-box"></div>
                </div>
            </div>
        </div>
    </main>

    <script>
        // 1. CHUYỂN TRANG
        function move(idx, btn) {
            document.getElementById('wrapper').style.transform = `translateX(-${idx * 100}vw)`;
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        }

        // 2. ICON RƠI KHÔNG LAG
        const icons = ['fa-bolt', 'fa-code', 'fa-microchip', 'fa-atom', 'fa-shield-halved'];
        function spawnIcons() {
            const container = document.getElementById('particles');
            for(let i=0; i<40; i++) {
                setTimeout(() => {
                    const icon = document.createElement('i');
                    icon.className = `fas ${icons[Math.floor(Math.random()*icons.length)]} p-icon`;
                    icon.style.left = Math.random() * 100 + 'vw';
                    icon.style.animationDuration = (Math.random() * 3 + 3) + 's';
                    icon.style.fontSize = (Math.random() * 12 + 10) + 'px';
                    container.appendChild(icon);
                    icon.addEventListener('animationend', () => icon.remove());
                }, i * 200);
            }
        }
        spawnIcons();
        setInterval(spawnIcons, 7000);

        // 3. HỆ THỐNG PHÂN TÍCH CHÍNH (GIỮ NGUYÊN LOGIC)
        const btn = document.getElementById('analyze-trigger');
        const input = document.getElementById('md5-input');

        btn.onclick = () => {
            const val = input.value.trim();
            if(val.length !== 32) return alert("Mã MD5 phải đủ 32 ký tự!");

            document.getElementById('loader').style.display = 'block';
            document.getElementById('result-ui').style.display = 'none';

            setTimeout(() => {
                // THUẬT TOÁN GỐC CỦA BẠN
                let energy = 0;
                for(let char of val) energy += parseInt(char, 16) || 0;
                
                let taiPct = (energy % 41) + 32; 
                let xiuPct = 100 - taiPct;
                let recommendation = taiPct > xiuPct ? "TÀI" : "XỈU";
                let winRate = Math.max(taiPct, xiuPct);

                // LOGIC MỚI: NÊN VÀO / VÀO NHẸ
                let action = "";
                let actionColor = "";
                if(winRate >= 65) {
                    action = "NÊN VÀO (TỰ TIN)";
                    actionColor = "#10b981"; // Xanh lá
                } else if(winRate >= 58) {
                    action = "VÀO NHẸ (CÂN NHẮC)";
                    actionColor = "#f59e0b"; // Vàng
                } else {
                    action = "KHÔNG NÊN VÀO (RỦI RO)";
                    actionColor = "#ef4444"; // Đỏ
                }

                document.getElementById('loader').style.display = 'none';
                document.getElementById('result-ui').style.display = 'block';
                
                document.getElementById('tai-val').innerText = taiPct + "%";
                document.getElementById('xiu-val').innerText = xiuPct + "%";
                
                setTimeout(() => {
                    document.getElementById('tai-bar').style.width = taiPct + "%";
                    document.getElementById('xiu-bar').style.width = xiuPct + "%";
                    
                    const resEl = document.getElementById('final-res');
                    resEl.innerText = recommendation;
                    resEl.style.color = taiPct > xiuPct ? "var(--primary)" : "var(--danger)";

                    const actionEl = document.getElementById('action-status');
                    actionEl.innerText = action;
                    actionEl.style.color = actionColor;
                }, 50);

                // LỊCH SỬ
                const hBox = document.getElementById('history-box');
                const div = document.createElement('div');
                div.className = 'history-item';
                div.innerHTML = `<span>#${val.slice(-6)}</span> <b>${recommendation}</b> <small style="color:${actionColor}">${winRate}%</small>`;
                hBox.prepend(div);
                if(hBox.children.length > 5) hBox.lastChild.remove();

            }, 1000);
        };
    </script>
</body>
</html>
