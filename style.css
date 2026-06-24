// エラーの原因になるグローバル汚染を防ぐため、安全なスコープで実行
(function() {
    // ==========================================
    // 🎬 ロードアニメーションの安全なフェードアウト
    // ==========================================
    window.addEventListener('load', () => {
        const loader = document.getElementById('loading-screen');
        // アニメーション全体の完了（約2.3秒）を見計らって非表示クラスを付与
        setTimeout(() => {
            if(loader) loader.classList.add('fade-out');
        }, 2400); 
    });

    // ==========================================
    // ⚡ ターボモード & FPS制御 (トグル重複バグ防止)
    // ==========================================
    let isTurbo = false;
    const turboBtn = document.getElementById('turbo-btn');
    if (turboBtn) {
        turboBtn.addEventListener('click', () => {
            isTurbo = !isTurbo;
            turboBtn.classList.toggle('active', isTurbo);
        });
    }

    const fpsSlider = document.getElementById('fps-slider');
    const fpsDisplay = document.getElementById('fps-display');
    if (fpsSlider && fpsDisplay) {
        fpsSlider.addEventListener('input', (e) => {
            fpsDisplay.textContent = e.target.value + 'FPS';
        });
    }

    // ==========================================
    // ✖ タブ（パレット）の開閉バグ防止
    // ==========================================
    const closePaletteBtn = document.getElementById('close-palette-btn');
    const blockPalette = document.getElementById('block-palette');
    if (closePaletteBtn && blockPalette) {
        closePaletteBtn.addEventListener('click', () => {
            if (blockPalette.style.visibility === 'hidden') {
                blockPalette.style.visibility = 'visible';
                blockPalette.style.opacity = '1';
                closePaletteBtn.textContent = '✖';
            } else {
                blockPalette.style.visibility = 'hidden';
                blockPalette.style.opacity = '0';
                closePaletteBtn.textContent = '👁'; // 閉じている時は目印に変える
            }
        });
    }

    // ==========================================
    // 🧱 ブロックのタップ生成と個別削除
    // ==========================================
    const workspace = document.getElementById('workspace');
    const sourceBlocks = document.querySelectorAll('#block-palette .streech-block');

    sourceBlocks.forEach(sourceBlock => {
        sourceBlock.addEventListener('click', () => {
            if (!workspace) return;
            
            // ブロックを完全にクローン
            const newBlock = sourceBlock.cloneNode(true);
            
            // クローンされたブロックをクリックした時だけ削除するイベントを紐付け（誤作動防止）
            newBlock.addEventListener('click', (e) => {
                e.stopPropagation(); // 親要素へのイベント伝播を防ぎ、安全に削除
                newBlock.remove();
            });
            
            workspace.appendChild(newBlock);
        });
    });

    // ==========================================
    // 💾 プロジェクトデータの保存 (連動バグ修正済)
    // ==========================================
    const saveBtn = document.getElementById('save-btn');
    if (saveBtn) {
        saveBtn.addEventListener('click', () => {
            const username = document.getElementById('username-input')?.value || 'streech';
            const currentFps = fpsSlider?.value || 60;
            
            // ワークスペース内にある現在のブロックの種類を正しくスキャン
            const currentBlocks = [];
            workspace.querySelectorAll('.streech-block').forEach(b => {
                currentBlocks.push(b.getAttribute('data-type'));
            });

            const streechData = {
                appName: "streech",
                author: username,
                turboMode: isTurbo,
                fps: currentFps,
                blocks: currentBlocks
            };

            const jsonString = JSON.stringify(streechData, null, 2);
            const blob = new Blob([jsonString], { type: 'application/json' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = `${username}_project.streech`;
            link.click();
            URL.revokeObjectURL(link.href);
        });
    }

    // ==========================================
    // 📁 プロジェクトデータの読み込み
    // ==========================================
    const fileLoad = document.getElementById('file-load');
    if (fileLoad) {
        fileLoad.addEventListener('change', (event) => {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = (e) => {
                try {
                    const loadedData = JSON.parse(e.target.result);
                    if (loadedData.appName === "streech") {
                        // ユーザー名、FPS、ターボモードの復元
                        if(document.getElementById('username-input')) document.getElementById('username-input').value = loadedData.author;
                        isTurbo = loadedData.turboMode;
                        if(turboBtn) turboBtn.classList.toggle('active', isTurbo);
                        if(fpsSlider) fpsSlider.value = loadedData.fps;
                        if(fpsDisplay) fpsDisplay.textContent = loadedData.fps + 'FPS';

                        // ワークスペースを一度綺麗にしてから、保存されていたブロックを再生成
                        if(workspace) {
                            workspace.innerHTML = '';
                            loadedData.blocks.forEach(type => {
                                const original = document.querySelector(`#block-palette .streech-block[data-type="${type}"]`);
                                if(original) {
                                    const restoredBlock = original.cloneNode(true);
                                    restoredBlock.addEventListener('click', (ev) => {
                                        ev.stopPropagation();
                                        restoredBlock.remove();
                                    });
                                    workspace.appendChild(restoredBlock);
                                }
                            });
                        }
                        alert('プロジェクトを正常に読み込みました！');
                    } else {
                        alert('streech専用のファイルではありません。');
                    }
                } catch (err) {
                    alert('ファイルの読み込み中に予期せぬエラーが発生しました。');
                }
            };
            reader.readAsText(file);
        });
    }
})();
