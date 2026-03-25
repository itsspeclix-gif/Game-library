// gameLibrary.js
(async function() {
    const vaultUrl = "https://cdn.jsdelivr.net/gh/itsspeclix-gif/Game-library@master/vault"; 
    const masterKey = "2026"; 
    const sessionLimit = 30 * 60 * 1000; 

    // Load CryptoJS for AES decryption
    const s = document.createElement('script'); 
    s.src = "https://cdnjs.cloudflare.com/ajax/libs/crypto-js/4.1.1/crypto-js.min.js"; 
    document.head.appendChild(s); 

    s.onload = () => {
        const savedData = localStorage.getItem('gameLibSession'); 
        if (savedData) { 
            const session = JSON.parse(savedData); 
            if (Date.now() - session.time < sessionLimit) { 
                loadLibrary(session.games, session.user || 'User', session.time); 
                return; 
            } 
        } 

        // Login screen
        const bg = document.createElement('div'); 
        bg.style = "position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(10,10,10,0.98);z-index:999999;display:flex;justify-content:center;align-items:center;font-family:'Inter',sans-serif;backdrop-filter:blur(15px);"; 
        bg.innerHTML = `
            <style>@import url("https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap");</style>
            <div style="background:#161616;padding:40px;border-radius:24px;border:1px solid rgba(255,255,255,0.1);width:300px;text-align:center;">
                <h2 style="color:#fff;margin:0 0 8px 0;font-weight:700;font-size:1.5rem;">Library Access</h2>
                <p style="color:#666;margin:0 0 24px 0;font-size:0.9rem;">Secure Authentication</p>
                <input id="u" type="text" placeholder="Username" style="width:100%;padding:14px;margin-bottom:12px;background:#000;color:#fff;border:1px solid #333;border-radius:12px;outline:none;box-sizing:border-box;font-family:'Inter';">
                <input id="p" type="password" placeholder="Password" style="width:100%;padding:14px;margin-bottom:24px;background:#000;color:#fff;border:1px solid #333;border-radius:12px;outline:none;box-sizing:border-box;font-family:'Inter';">
                <button id="loginBtn" style="width:100%;padding:14px;background:#3b82f6;color:#fff;border:none;border-radius:12px;cursor:pointer;font-weight:600;font-family:'Inter';font-size:1rem;">Authorize</button>
                <p id="msg" style="color:#ff4444;margin-top:15px;font-size:13px;display:none;">Invalid Credentials</p>
            </div>
        `; 
        document.body.appendChild(bg); 
        document.getElementById('u').focus(); 

        document.getElementById('loginBtn').onclick = async function() { 
            const uInput = document.getElementById('u').value.trim(); 
            const pInput = document.getElementById('p').value.trim(); 
            const loginTime = Date.now();
            try { 
                const res = await fetch(vaultUrl); 
                const enc = await res.text(); 
                const bytes = CryptoJS.AES.decrypt(enc, masterKey); 
                const vault = JSON.parse(bytes.toString(CryptoJS.enc.Utf8)); 
                let auth = false; 
                for (let user in vault.config) { 
                    if (user.toLowerCase() === uInput.toLowerCase() && vault.config[user] === pInput) { 
                        auth = true; 
                        break; 
                    } 
                } 
                if (auth) { 
                    localStorage.setItem('gameLibSession', JSON.stringify({ time: loginTime, games: vault.games, user: uInput })); 
                    bg.remove(); 
                    loadLibrary(vault.games, uInput, loginTime); 
                } else { 
                    throw new Error(); 
                } 
            } catch (err) { 
                document.getElementById('msg').style.display = "block"; 
            } 
        }; 
    }; 

    function loadLibrary(games, userName, startTime) { 
        // Your existing loadLibrary code here
        console.log("Loaded library for", userName, games);
    }
})();
