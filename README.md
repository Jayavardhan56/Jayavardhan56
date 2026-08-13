<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jayavardhan@MacBook-Pro ~ zsh</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: #0d1117;
            color: #c9d1d9;
            font-family: 'JetBrains Mono', monospace;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .terminal-window {
            width: 100%;
            max-width: 900px;
            height: 85vh;
            background-color: #161b22;
            border-radius: 10px;
            border: 1px solid #30363d;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.6);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        .terminal-header {
            background-color: #0d1117;
            padding: 12px 16px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid #30363d;
            user-select: none;
        }

        .window-controls {
            display: flex;
            gap: 8px;
        }

        .btn {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            display: inline-block;
        }

        .btn-close { background-color: #ff5f56; }
        .btn-minimize { background-color: #ffbd2e; }
        .btn-maximize { background-color: #27c93f; }

        .window-title {
            color: #8b949e;
            font-size: 0.85rem;
            font-weight: 500;
        }

        .window-actions {
            color: #8b949e;
            font-size: 0.85rem;
        }

        .terminal-body {
            padding: 20px;
            flex-grow: 1;
            overflow-y: auto;
            font-size: 0.95rem;
            line-height: 1.5;
        }

        /* Custom Scrollbar */
        .terminal-body::-webkit-scrollbar {
            width: 8px;
        }
        .terminal-body::-webkit-scrollbar-track {
            background: #161b22;
        }
        .terminal-body::-webkit-scrollbar-thumb {
            background: #30363d;
            border-radius: 4px;
        }

        .command-line {
            display: flex;
            align-items: center;
            flex-wrap: wrap;
            margin-top: 10px;
            margin-bottom: 5px;
        }

        .prompt {
            color: #58a6ff;
            font-weight: bold;
            margin-right: 10px;
            white-space: nowrap;
        }

        .prompt .user { color: #79c0ff; }
        .prompt .host { color: #d2a8ff; }
        .prompt .path { color: #a5d6ff; }
        .prompt .symbol { color: #79c0ff; }

        .input-container {
            display: flex;
            flex-grow: 1;
            align-items: center;
        }

        .cmd-input {
            background: transparent;
            border: none;
            color: #f0f6fc;
            font-family: 'JetBrains Mono', monospace;
            font-size: 0.95rem;
            width: 100%;
            outline: none;
            caret-color: #58a6ff;
        }

        .output {
            margin-bottom: 12px;
            white-space: pre-wrap;
            word-break: break-word;
        }

        .accent { color: #58a6ff; }
        .success { color: #3fb950; }
        .warning { color: #d29922; }
        .danger { color: #f85149; }
        .muted { color: #8b949e; }
        .highlight { color: #d2a8ff; }

        .ascii-art {
            color: #a5d6ff;
            font-weight: bold;
            line-height: 1.2;
        }

        .neofetch-container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin: 10px 0;
        }

        .info-table {
            border-collapse: collapse;
            width: 100%;
            margin: 10px 0;
        }

        .info-table td {
            padding: 4px 8px;
            vertical-align: top;
        }

        .box {
            border: 1px solid #30363d;
            border-radius: 6px;
            padding: 12px;
            background-color: #0d1117;
            margin: 8px 0;
        }

        a {
            color: #58a6ff;
            text-decoration: none;
        }

        a:hover {
            text-decoration: underline;
        }

        .progress-bar {
            color: #3fb950;
        }

        .help-grid {
            display: grid;
            grid-template-columns: auto 1fr;
            gap: 8px 20px;
            margin: 10px 0;
        }
    </style>
</head>
<body>

<div class="terminal-window">
    <div class="terminal-header">
        <div class="window-controls">
            <span class="btn btn-close"></span>
            <span class="btn btn-minimize"></span>
            <span class="btn btn-maximize"></span>
        </div>
        <div class="window-title">jayavardhan@MacBook-Pro — zsh — 120×32</div>
        <div class="window-actions">⌘</div>
    </div>
    
    <div class="terminal-body" id="terminal-body">
        <div id="output-container"></div>
        
        <div class="command-line" id="active-input-line">
            <span class="prompt">
                <span class="user">jayavardhan</span>@<span class="host">MacBook-Pro</span> <span class="path">~</span> <span class="symbol">%</span>
            </span>
            <div class="input-container">
                <input type="text" id="cmd-input" class="cmd-input" autofocus autocomplete="off" spellcheck="false">
            </div>
        </div>
    </div>
</div>

<script>
    const outputContainer = document.getElementById('output-container');
    const cmdInput = document.getElementById('cmd-input');
    const terminalBody = document.getElementById('terminal-body');

    const profileData = {
        name: "K Jayavardhan",
        role: "DevOps Aspirant",
        degree: "B.Tech CSE",
        year: "2026",
        location: "India",
        status: "Available / OPEN TO WORK",
        mission: "Build → Automate → Deploy → Monitor → Improve",
        github: "https://github.com/Jayavardhan56",
        linkedin: "https://www.linkedin.com/in/konathala-jayavardhan-130907261",
        telegram: "https://t.me/Jayavardhan56"
    };

    const commandHistory = [];
    let historyIndex = -1;

    const commands = {
        'help': () => `
<span class="accent">AVAILABLE COMMANDS:</span>

  <span class="highlight">neofetch</span>        Display system summary & skill ratings
  <span class="highlight">whoami</span>          Display current user identity
  <span class="highlight">about</span>           About K Jayavardhan
  <span class="highlight">stack</span>           List technical skills & ecosystem
  <span class="highlight">pipeline</span>        View DevOps workflow
  <span class="highlight">learning</span>        Display current learning status & logs
  <span class="highlight">connect</span>         Social links & communication handles
  <span class="highlight">mission</span>         Show career echo mission statement
  <span class="highlight">clear</span>           Clear terminal screen
`,

        'neofetch': () => `
<div class="neofetch-container">
<pre class="ascii-art">
              .:' 
          _ :' _  
       .-' \`   \` '-.
     .'             '.
    /   .-"""-.       \\
   ;   /       \\       ;
   |  |  <span class="accent">◉</span>   <span class="accent">◉</span>  |      |
   ;   \\   ^   /       ;
    \\   '.___.'       / 
     '.             .'  
       '-._______.-'    
</pre>
<div>
<span class="accent">jayavardhan</span>@<span class="highlight">MacBook-Pro</span>
─────────────────────────────
<span class="muted">OS</span>     : macOS / Linux
<span class="muted">Shell</span>  : zsh / bash
<span class="muted">Role</span>   : ${profileData.role}
<span class="muted">Focus</span>  : Cloud + Automation
<span class="muted">Status</span> : <span class="success">${profileData.status}</span>
─────────────────────────────
<span class="muted">AWS</span>    : <span class="progress-bar">█████████░</span> learning
<span class="muted">Docker</span> : <span class="progress-bar">█████████░</span> hands-on
<span class="muted">Linux</span>  : <span class="progress-bar">█████████░</span> hands-on
<span class="muted">Git</span>    : <span class="progress-bar">██████████</span> strong
<span class="muted">Jenkins</span>: <span class="progress-bar">███████░░░</span> learning
<span class="muted">CI/CD</span>  : <span class="progress-bar">███████░░░</span> learning
</div>
</div>
`,

        'whoami': () => `<span class="highlight">${profileData.name}</span>`,

        'about': () => `
<div class="box">
<span class="accent">ABOUT ME</span>
───────
I'm <span class="highlight">K Jayavardhan</span>, a B.Tech Computer Science graduate focused on starting my career in DevOps and Cloud Engineering.

I enjoy working close to the infrastructure layer — Linux systems, containers, CI/CD pipelines, cloud services, and deployment automation.

I learn by building, breaking, debugging, and rebuilding systems until I understand how they work.
</div>
`,

        'stack': () => `
<span class="accent">SYSTEM STACK & TOOLS</span>
────────────────────

☁️  <span class="highlight">Cloud:</span>          AWS (EC2, S3, IAM, Lambda), GCP Basics
🐧 <span class="highlight">Infrastructure:</span> Linux, Docker, Jenkins
💻 <span class="highlight">Programming:</span>    Java, Python, C, C++, JavaScript
🌐 <span class="highlight">Development:</span>    React, Django, Spring Boot, REST APIs
🗄️ <span class="highlight">Databases:</span>      PostgreSQL, MySQL, SQL
🔧 <span class="highlight">Tools:</span>          Git, GitHub, VS Code, Postman
`,

        'pipeline': () => `
<span class="accent">DEVOPS DEPLOYMENT PIPELINE</span>
──────────────────────────
[CODE] ➔ [GIT] ➔ [BUILD] ➔ [TEST] ➔ [DOCKER] ➔ [CI/CD] ➔ [CLOUD] ➔ [MONITOR]
`,

        'learning': () => `
<span class="accent">CURRENTLY LEARNING & TOP TRACKS</span>
───────────────────────────────
🐳  <span class="highlight">Docker</span>      : Containerization, Images, Deployment
☁️  <span class="highlight">AWS</span>         : Cloud Infrastructure, Core Services
🔄 <span class="highlight">CI/CD</span>       : Jenkins, Automation, Pipelines
☸️  <span class="highlight">Kubernetes</span> : Orchestration & Scaling Fundamentals

<span class="muted">Recent Activity Log:</span>
✓ Linux fundamentals
✓ Git & GitHub
✓ Docker
✓ AWS fundamentals
➔ Jenkins / CI-CD
➔ Kubernetes / Infrastructure Automation
`,

        'connect': () => `
<span class="accent">CONNECT / LINKS</span>
───────────────
<span class="muted">GitHub:</span>   <a href="${profileData.github}" target="_blank">${profileData.github}</a>
<span class="muted">LinkedIn:</span> <a href="${profileData.linkedin}" target="_blank">linkedin.com/in/konathala-jayavardhan</a>
<span class="muted">Telegram:</span> <a href="${profileData.telegram}" target="_blank">t.me/Jayavardhan56</a>
`,

        'mission': () => `<span class="success">${profileData.mission}</span>`
    };

    // Auto Run standard introductory setup on start
    window.addEventListener('DOMContentLoaded', () => {
        executeCommand('neofetch');
    });

    cmdInput.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') {
            const rawInput = cmdInput.value.trim();
            const cmd = rawInput.toLowerCase();

            // Print Command Line History
            appendPromptOutput(rawInput);

            if (rawInput !== '') {
                commandHistory.push(rawInput);
                historyIndex = commandHistory.length;

                if (cmd === 'clear') {
                    outputContainer.innerHTML = '';
                } else if (commands[cmd]) {
                    appendOutput(commands[cmd]());
                } else {
                    appendOutput(`<span class="danger">zsh: command not found: ${escapeHtml(rawInput)}</span>. Type <span class="highlight">'help'</span> for available commands.`);
                }
            }

            cmdInput.value = '';
            scrollToBottom();
        } else if (e.key === 'ArrowUp') {
            e.preventDefault();
            if (historyIndex > 0) {
                historyIndex--;
                cmdInput.value = commandHistory[historyIndex];
            }
        } else if (e.key === 'ArrowDown') {
            e.preventDefault();
            if (historyIndex < commandHistory.length - 1) {
                historyIndex++;
                cmdInput.value = commandHistory[historyIndex];
            } else {
                historyIndex = commandHistory.length;
                cmdInput.value = '';
            }
        } else if (e.key === 'Tab') {
            e.preventDefault();
            const inputVal = cmdInput.value.trim().toLowerCase();
            if (inputVal) {
                const matches = Object.keys(commands).filter(c => c.startsWith(inputVal));
                if (matches.length === 1) {
                    cmdInput.value = matches[0];
                }
            }
        }
    });

    function executeCommand(cmdName) {
        appendPromptOutput(cmdName);
        if (commands[cmdName]) {
            appendOutput(commands[cmdName]());
        }
        scrollToBottom();
    }

    function appendPromptOutput(cmdText) {
        const line = document.createElement('div');
        line.className = 'command-line';
        line.innerHTML = `
            <span class="prompt">
                <span class="user">jayavardhan</span>@<span class="host">MacBook-Pro</span> <span class="path">~</span> <span class="symbol">%</span>
            </span>
            <span>${escapeHtml(cmdText)}</span>
        `;
        outputContainer.appendChild(line);
    }

    function appendOutput(htmlContent) {
        const out = document.createElement('div');
        out.className = 'output';
        out.innerHTML = htmlContent;
        outputContainer.appendChild(out);
    }

    function scrollToBottom() {
        terminalBody.scrollTop = terminalBody.scrollHeight;
    }

    function escapeHtml(text) {
        return text
            .replace(/&/g, "&amp;")
            .replace(/</g, "&lt;")
            .replace(/>/g, "&gt;")
            .replace(/"/g, "&quot;")
            .replace(/'/g, "&#039;");
    }

    // Always keep focus on terminal input
    document.addEventListener('click', () => {
        cmdInput.focus();
    });
</script>

</body>
</html>
