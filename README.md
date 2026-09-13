```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PixelForge | Pixel Art Studio</title>

    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        :root {
            --bg: #090b12;
            --panel: #111522;
            --panel-light: #181d2c;
            --border: #293044;
            --text: #f4f7ff;
            --muted: #8c96ad;
            --accent: #7c5cff;
            --accent-hover: #9278ff;
            --danger: #ff5577;
        }

        body {
            min-height: 100vh;
            font-family: Arial, Helvetica, sans-serif;
            background:
                radial-gradient(circle at top left, #17112e 0, transparent 35%),
                radial-gradient(circle at bottom right, #101d32 0, transparent 35%),
                var(--bg);
            color: var(--text);
        }

        button,
        input {
            font: inherit;
        }

        button {
            cursor: pointer;
        }

        .app {
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        header {
            height: 72px;
            padding: 0 28px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(9, 11, 18, 0.88);
            border-bottom: 1px solid var(--border);
            backdrop-filter: blur(15px);
            position: sticky;
            top: 0;
            z-index: 20;
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .brand-mark {
            width: 38px;
            height: 38px;
            border-radius: 10px;
            background:
                linear-gradient(90deg, #7c5cff 50%, #20d4ff 50%);
            box-shadow: 0 0 25px rgba(124, 92, 255, 0.35);
        }

        .brand h1 {
            font-size: 21px;
            letter-spacing: 0.5px;
        }

        .brand p {
            color: var(--muted);
            font-size: 11px;
            margin-top: 2px;
        }

        .header-actions {
            display: flex;
            gap: 8px;
        }

        .button {
            border: 1px solid var(--border);
            background: var(--panel);
            color: var(--text);
            padding: 9px 13px;
            border-radius: 9px;
            transition: 0.2s ease;
        }

        .button:hover {
            border-color: var(--accent);
            background: var(--panel-light);
            transform: translateY(-1px);
        }

        .button.primary {
            background: var(--accent);
            border-color: var(--accent);
        }

        .button.primary:hover {
            background: var(--accent-hover);
        }

        .workspace {
            flex: 1;
            display: grid;
            grid-template-columns: 260px minmax(0, 1fr) 270px;
            min-height: 0;
        }

        .sidebar {
            padding: 20px;
            background: rgba(13, 16, 27, 0.82);
            border-right: 1px solid var(--border);
        }

        .right-sidebar {
            border-right: none;
            border-left: 1px solid var(--border);
        }

        .section {
            margin-bottom: 24px;
        }

        .section-title {
            color: var(--muted);
            text-transform: uppercase;
            letter-spacing: 1.5px;
            font-size: 10px;
            font-weight: bold;
            margin-bottom: 12px;
        }

        .tools {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 8px;
        }

        .tool {
            min-height: 44px;
            background: var(--panel);
            color: var(--text);
            border: 1px solid var(--border);
            border-radius: 9px;
            transition: 0.2s;
        }

        .tool:hover {
            border-color: #4c5875;
        }

        .tool.active {
            background: rgba(124, 92, 255, 0.18);
            border-color: var(--accent);
            color: #cfc5ff;
        }

        .control-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 10px;
            margin-bottom: 11px;
        }

        .control-row label {
            color: #c4cad8;
            font-size: 13px;
        }

        input[type="number"] {
            width: 75px;
            padding: 7px;
            background: var(--panel);
            border: 1px solid var(--border);
            border-radius: 7px;
            color: white;
            outline: none;
        }

        input[type="number"]:focus {
            border-color: var(--accent);
        }

        input[type="range"] {
            width: 100%;
            accent-color: var(--accent);
        }

        .range-value {
            color: var(--accent-hover);
            font-size: 12px;
            min-width: 35px;
            text-align: right;
        }

        .size-row {
            display: flex;
            gap: 8px;
        }

        .size-row .button {
            flex: 1;
        }

        .color-picker {
            width: 100%;
            height: 46px;
            padding: 3px;
            border: 1px solid var(--border);
            border-radius: 9px;
            background: var(--panel);
            cursor: pointer;
        }

        .palette {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 7px;
            margin-top: 12px;
        }

        .swatch {
            width: 100%;
            aspect-ratio: 1;
            border-radius: 6px;
            border: 2px solid transparent;
            cursor: pointer;
            transition: 0.15s;
        }

        .swatch:hover {
            transform: scale(1.08);
        }

        .swatch.selected {
            border-color: white;
            box-shadow: 0 0 0 2px var(--accent);
        }

        .canvas-area {
            min-width: 0;
            min-height: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 28px;
            position: relative;
        }

        .canvas-info {
            width: min(850px, 100%);
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .canvas-info h2 {
            font-size: 16px;
        }

        .canvas-info span {
            color: var(--muted);
            font-size: 12px;
        }

        .canvas-wrapper {
            max-width: min(850px, 100%);
            max-height: calc(100vh - 190px);
            overflow: auto;
            padding: 18px;
            border: 1px solid var(--border);
            border-radius: 15px;
            background:
                linear-gradient(45deg, #151925 25%, transparent 25%),
                linear-gradient(-45deg, #151925 25%, transparent 25%),
                linear-gradient(45deg, transparent 75%, #151925 75%),
                linear-gradient(-45deg, transparent 75%, #151925 75%);
            background-size: 24px 24px;
            background-position: 0 0, 0 12px, 12px -12px, -12px 0;
            box-shadow: 0 25px 70px rgba(0, 0, 0, 0.35);
        }

        #pixelCanvas {
            display: block;
            background: white;
            image-rendering: pixelated;
            image-rendering: crisp-edges;
            cursor: crosshair;
            box-shadow: 0 0 0 1px #000;
        }

        .save-list {
            display: flex;
            flex-direction: column;
            gap: 8px;
            max-height: 250px;
            overflow-y: auto;
        }

        .saved-item {
            padding: 10px;
            background: var(--panel);
            border: 1px solid var(--border);
            border-radius: 9px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 8px;
        }

        .saved-name {
            min-width: 0;
        }

        .saved-name strong {
            display: block;
            font-size: 12px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .saved-name small {
            color: var(--muted);
            font-size: 10px;
        }

        .saved-buttons {
            display: flex;
            gap: 4px;
        }

        .small-button {
            padding: 6px 7px;
            border: 1px solid var(--border);
            background: var(--panel-light);
            color: white;
            border-radius: 6px;
            font-size: 10px;
        }

        .small-button:hover {
            border-color: var(--accent);
        }

        .empty {
            color: var(--muted);
            font-size: 12px;
            text-align: center;
            padding: 20px 5px;
        }

        .status {
            position: fixed;
            left: 50%;
            bottom: 25px;
            transform: translate(-50%, 80px);
            background: #171c2b;
            border: 1px solid var(--border);
            padding: 10px 17px;
            border-radius: 10px;
            font-size: 12px;
            opacity: 0;
            transition: 0.3s;
            z-index: 100;
        }

        .status.show {
            transform: translate(-50%, 0);
            opacity: 1;
        }

        .checkbox-row {
            display: flex;
            align-items: center;
            gap: 9px;
            font-size: 13px;
            color: #c4cad8;
            margin-bottom: 12px;
        }

        input[type="checkbox"] {
            accent-color: var(--accent);
            width: 15px;
            height: 15px;
        }

        .danger {
            color: #ff829a;
        }

        @media (max-width: 1000px) {
            .workspace {
                grid-template-columns: 210px minmax(0, 1fr);
            }

            .right-sidebar {
                display: none;
            }
        }

        @media (max-width: 700px) {
            header {
                padding: 0 14px;
            }

            .header-actions .button:nth-child(1),
            .header-actions .button:nth-child(2) {
                display: none;
            }

            .workspace {
                display: flex;
                flex-direction: column;
            }

            .sidebar {
                border-right: none;
                border-bottom: 1px solid var(--border);
            }

            .tools {
                grid-template-columns: repeat(4, 1fr);
            }

            .section {
                margin-bottom: 14px;
            }

            .canvas-area {
                min-height: 500px;
                padding: 15px;
            }

            .canvas-wrapper {
                max-width: 100%;
            }
        }
    </style>
</head>

<body>

<div class="app">

    <header>
        <div class="brand">
            <div class="brand-mark"></div>
            <div>
                <h1>PixelForge</h1>
                <p>Pixel Art Studio</p>
            </div>
        </div>

        <div class="header-actions">
            <button class="button" id="undoButton">Undo</button>
            <button class="button" id="redoButton">Redo</button>
            <button class="button primary" id="downloadButton">Download PNG</button>
        </div>
    </header>

    <main class="workspace">

        <!-- LEFT SIDEBAR -->
        <aside class="sidebar">

            <div class="section">
                <div class="section-title">Tools</div>

                <div class="tools">
                    <button class="tool active" data-tool="pencil">Pencil</button>
                    <button class="tool" data-tool="eraser">Eraser</button>
                    <button class="tool" data-tool="fill">Fill</button>
                    <button class="tool" data-tool="picker">Picker</button>
                </div>
            </div>

            <div class="section">
                <div class="section-title">Canvas</div>

                <div class="control-row">
                    <label>Width</label>
                    <input type="number" id="canvasWidth" value="32" min="4" max="128">
                </div>

                <div class="control-row">
                    <label>Height</label>
                    <input type="number" id="canvasHeight" value="32" min="4" max="128">
                </div>

                <div class="size-row">
                    <button class="button" id="newCanvasButton">New</button>
                    <button class="button" id="clearButton">Clear</button>
                </div>
            </div>

            <div class="section">
                <div class="section-title">Brush Size</div>

                <div class="control-row">
                    <input
                        type="range"
                        id="brushSize"
                        min="1"
                        max="8"
                        value="1"
                    >
                    <span class="range-value" id="brushValue">1 px</span>
                </div>
            </div>

            <div class="section">
                <div class="section-title">Color</div>

                <input
                    type="color"
                    id="colorPicker"
                    class="color-picker"
                    value="#7c5cff"
                >

                <div class="palette" id="palette"></div>
            </div>

            <div class="section">
                <div class="section-title">Display</div>

                <label class="checkbox-row">
                    <input type="checkbox" id="gridToggle" checked>
                    Show pixel grid
                </label>
            </div>

        </aside>


        <!-- CANVAS -->
        <section class="canvas-area">

            <div class="canvas-info">
                <h2 id="artworkTitle">Untitled Artwork</h2>
                <span id="canvasDimensions">32 × 32</span>
            </div>

            <div class="canvas-wrapper">
                <canvas id="pixelCanvas"></canvas>
            </div>

        </section>


        <!-- RIGHT SIDEBAR -->
        <aside class="sidebar right-sidebar">

            <div class="section">
                <div class="section-title">Artwork</div>

                <button class="button" id="renameButton" style="width:100%;">
                    Rename Artwork
                </button>

                <button
                    class="button primary"
                    id="saveButton"
                    style="width:100%; margin-top:8px;"
                >
                    Save Artwork
                </button>
            </div>

            <div class="section">
                <div class="section-title">Saved Artworks</div>

                <div class="save-list" id="saveList"></div>
            </div>

            <div class="section">
                <div class="section-title">Keyboard</div>

                <div style="font-size:12px; line-height:1.9; color:#9aa4ba;">
                    <div><strong>Ctrl + Z</strong> — Undo</div>
                    <div><strong>Ctrl + Y</strong> — Redo</div>
                    <div><strong>P</strong> — Pencil</div>
                    <div><strong>E</strong> — Eraser</div>
                    <div><strong>F</strong> — Fill</div>
                    <div><strong>I</strong> — Picker</div>
                </div>
            </div>

            <div class="section">
                <div class="section-title">Project Info</div>

                <div style="font-size:12px; color:#8c96ad; line-height:1.7;">
                    PixelForge stores your saved artworks directly in your browser.
                    No account or server is required.
                </div>
            </div>

        </aside>

    </main>
</div>

<div class="status" id="statusMessage"></div>


<script>
    const canvas = document.getElementById("pixelCanvas");
    const ctx = canvas.getContext("2d");

    const canvasWidthInput = document.getElementById("canvasWidth");
    const canvasHeightInput = document.getElementById("canvasHeight");
    const canvasDimensions = document.getElementById("canvasDimensions");
    const artworkTitle = document.getElementById("artworkTitle");

    const colorPicker = document.getElementById("colorPicker");
    const brushSizeInput = document.getElementById("brushSize");
    const brushValue = document.getElementById("brushValue");

    const gridToggle = document.getElementById("gridToggle");
    const palette = document.getElementById("palette");
    const saveList = document.getElementById("saveList");

    let currentTool = "pencil";
    let currentColor = "#7c5cff";
    let isDrawing = false;

    let canvasWidth = 32;
    let canvasHeight = 32;

    let history = [];
    let historyIndex = -1;

    let artworkName = "Untitled Artwork";


    /* ---------------------------
       Palette
    --------------------------- */

    const colors = [
        "#000000",
        "#ffffff",
        "#ff3b30",
        "#ff9500",
        "#ffcc00",
        "#34c759",
        "#00c7be",
        "#32ade6",
        "#007aff",
        "#5856d6",
        "#af52de",
        "#ff2d55",
        "#8e8e93",
        "#636366",
        "#48484a",
        "#1c1c1e",
        "#794b25",
        "#c97c5d",
        "#f7b267",
        "#70c1b3",
        "#247ba0",
        "#50514f",
        "#f25f5c",
        "#ffe066"
    ];

    colors.forEach(color => {
        const swatch = document.createElement("button");
        swatch.className = "swatch";
        swatch.style.background = color;
        swatch.title = color;

        swatch.addEventListener("click", () => {
            currentColor = color;
            colorPicker.value = color;

            document.querySelectorAll(".swatch").forEach(item => {
                item.classList.remove("selected");
            });

            swatch.classList.add("selected");
        });

        palette.appendChild(swatch);
    });


    /* ---------------------------
       Canvas Setup
    --------------------------- */

    function setupCanvas(width, height) {
        canvasWidth = width;
        canvasHeight = height;

        const maxSize = Math.min(
            window.innerWidth > 900 ? 800 : window.innerWidth - 80,
            window.innerHeight - 210
        );

        const pixelSize = Math.max(
            8,
            Math.floor(maxSize / Math.max(width, height))
        );

        canvas.width = width * pixelSize;
        canvas.height = height * pixelSize;

        canvas.style.width = `${canvas.width}px`;
        canvas.style.height = `${canvas.height}px`;

        ctx.imageSmoothingEnabled = false;

        clearCanvas(false);

        canvasDimensions.textContent =
            `${width} × ${height}`;

        canvasWidthInput.value = width;
        canvasHeightInput.value = height;

        history = [];
        historyIndex = -1;

        saveHistory();
        drawGrid();
    }


    function clearCanvas(addHistory = true) {
        ctx.fillStyle = "#ffffff";
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        if (addHistory) {
            saveHistory();
        }

        drawGrid();
    }


    /* ---------------------------
       Pixel Coordinates
    --------------------------- */

    function getPixelPosition(event) {
        const rect = canvas.getBoundingClientRect();

        const x = Math.floor(
            ((event.clientX - rect.left) / rect.width) * canvasWidth
        );

        const y = Math.floor(
            ((event.clientY - rect.top) / rect.height) * canvasHeight
        );

        return {
            x: Math.max(0, Math.min(canvasWidth - 1, x)),
            y: Math.max(0, Math.min(canvasHeight - 1, y))
        };
    }


    function getPixelSize() {
        return canvas.width / canvasWidth;
    }


    /* ---------------------------
       Drawing
    --------------------------- */

    function drawPixel(x, y) {
        const size = getPixelSize();
        const brushSize = Number(brushSizeInput.value);

        if (currentTool === "pencil") {
            ctx.fillStyle = currentColor;
        }

        if (currentTool === "eraser") {
            ctx.fillStyle = "#ffffff";
        }

        if (
            currentTool === "pencil" ||
            currentTool === "eraser"
        ) {
            for (let offsetX = 0; offsetX < brushSize; offsetX++) {
                for (let offsetY = 0; offsetY < brushSize; offsetY++) {

                    const pixelX = x + offsetX;
                    const pixelY = y + offsetY;

                    if (
                        pixelX < canvasWidth &&
                        pixelY < canvasHeight
                    ) {
                        ctx.fillRect(
                            pixelX * size,
                            pixelY * size,
                            size,
                            size
                        );
                    }
                }
            }
        }

        if (currentTool === "picker") {
            const imageData = ctx.getImageData(
                Math.floor(x * size + size / 2),
                Math.floor(y * size + size / 2),
                1,
                1
            ).data;

            const pickedColor =
                "#" +
                [imageData[0], imageData[1], imageData[2]]
                    .map(value => value.toString(16).padStart(2, "0"))
                    .join("");

            currentColor = pickedColor;
            colorPicker.value = pickedColor;

            setTool("pencil");
        }

        if (currentTool === "fill") {
            floodFill(x, y);
            saveHistory();
        }

        drawGrid();
    }


    function floodFill(startX, startY) {
        const size = getPixelSize();

        const imageData = ctx.getImageData(
            0,
            0,
            canvas.width,
            canvas.height
        );

        const targetX = Math.floor(startX * size + size / 2);
        const targetY = Math.floor(startY * size + size / 2);

        const startIndex =
            (targetY * canvas.width + targetX) * 4;

        const targetColor = [
            imageData.data[startIndex],
            imageData.data[startIndex + 1],
            imageData.data[startIndex + 2],
            imageData.data[startIndex + 3]
        ];

        const fillColor = hexToRgba(currentColor);

        if (
            targetColor[0] === fillColor[0] &&
            targetColor[1] === fillColor[1] &&
            targetColor[2] === fillColor[2]
        ) {
            return;
        }

        const pixelQueue = [[startX, startY]];
        const visited = new Set();

        while (pixelQueue.length) {
            const [x, y] = pixelQueue.shift();
            const key = `${x},${y}`;

            if (visited.has(key)) {
                continue;
            }

            visited.add(key);

            if (
                x < 0 ||
                y < 0 ||
                x >= canvasWidth ||
                y >= canvasHeight
            ) {
                continue;
            }

            const pixelX = Math.floor(x * size + size / 2);
            const pixelY = Math.floor(y * size + size / 2);

            const index =
                (pixelY * canvas.width + pixelX) * 4;

            if (
                imageData.data[index] !== targetColor[0] ||
                imageData.data[index + 1] !== targetColor[1] ||
                imageData.data[index + 2] !== targetColor[2]
            ) {
                continue;
            }

            ctx.fillStyle = currentColor;
            ctx.fillRect(
                x * size,
                y * size,
                size,
                size
            );

            pixelQueue.push([x + 1, y]);
            pixelQueue.push([x - 1, y]);
            pixelQueue.push([x, y + 1]);
            pixelQueue.push([x, y - 1]);
        }
    }


    function hexToRgba(hex) {
        const value = hex.replace("#", "");

        return [
            parseInt(value.substring(0, 2), 16),
            parseInt(value.substring(2, 4), 16),
            parseInt(value.substring(4, 6), 16),
            255
        ];
    }


    /* ---------------------------
       Grid
    --------------------------- */

    function drawGrid() {
        if (!gridToggle.checked) {
            return;
        }

        const size = getPixelSize();

        ctx.save();

        ctx.strokeStyle = "rgba(0,0,0,0.16)";
        ctx.lineWidth = 1;

        for (let x = 0; x <= canvasWidth; x++) {
            ctx.beginPath();
            ctx.moveTo(x * size + 0.5, 0);
            ctx.lineTo(x * size + 0.5, canvas.height);
            ctx.stroke();
        }

        for (let y = 0; y <= canvasHeight; y++) {
            ctx.beginPath();
            ctx.moveTo(0, y * size + 0.5);
            ctx.lineTo(canvas.width, y * size + 0.5);
            ctx.stroke();
        }

        ctx.restore();
    }


    /* ---------------------------
       Mouse Events
    --------------------------- */

    canvas.addEventListener("mousedown", event => {
        isDrawing = true;

        const position = getPixelPosition(event);
        drawPixel(position.x, position.y);

        if (
            currentTool === "pencil" ||
            currentTool === "eraser"
        ) {
            saveHistory();
        }
    });


    canvas.addEventListener("mousemove", event => {
        if (!isDrawing) {
            return;
        }

        const position = getPixelPosition(event);

        if (
            currentTool === "pencil" ||
            currentTool === "eraser"
        ) {
            drawPixel(position.x, position.y);
        }
    });


    window.addEventListener("mouseup", () => {
        isDrawing = false;
    });


    /* ---------------------------
       Touch Support
    --------------------------- */

    canvas.addEventListener(
        "touchstart",
        event => {
            event.preventDefault();

            const touch = event.touches[0];

            const fakeMouseEvent = {
                clientX: touch.clientX,
                clientY: touch.clientY
            };

            isDrawing = true;

            const position =
                getPixelPosition(fakeMouseEvent);

            drawPixel(position.x, position.y);

            saveHistory();
        },
        { passive: false }
    );


    canvas.addEventListener(
        "touchmove",
        event => {
            event.preventDefault();

            if (!isDrawing) {
                return;
            }

            const touch = event.touches[0];

            const position = getPixelPosition({
                clientX: touch.clientX,
                clientY: touch.clientY
            });

            drawPixel(position.x, position.y);
        },
        { passive: false }
    );


    window.addEventListener("touchend", () => {
        isDrawing = false;
    });


    /* ---------------------------
       Tools
    --------------------------- */

    function setTool(tool) {
        currentTool = tool;

        document.querySelectorAll(".tool").forEach(button => {
            button.classList.toggle(
                "active",
                button.dataset.tool === tool
            );
        });
    }


    document.querySelectorAll(".tool").forEach(button => {
        button.addEventListener("click", () => {
            setTool(button.dataset.tool);
        });
    });


    /* ---------------------------
       Color
    --------------------------- */

    colorPicker.addEventListener("input", event => {
        currentColor = event.target.value;
    });


    /* ---------------------------
       Brush Size
    --------------------------- */

    brushSizeInput.addEventListener("input", () => {
        brushValue.textContent =
            `${brushSizeInput.value} px`;
    });


    /* ---------------------------
       History
    --------------------------- */

    function saveHistory() {
        const image = canvas.toDataURL();

        history =
            history.slice(0, historyIndex + 1);

        history.push(image);

        historyIndex = history.length - 1;

        if (history.length > 30) {
            history.shift();
            historyIndex--;
        }
    }


    function restoreHistory(index) {
        if (
            index < 0 ||
            index >= history.length
        ) {
            return;
        }

        const image = new Image();

        image.onload = () => {
            ctx.clearRect(
                0,
                0,
                canvas.width,
                canvas.height
            );

            ctx.drawImage(
                image,
                0,
                0,
                canvas.width,
                canvas.height
            );

            drawGrid();
        };

        image.src = history[index];
    }


    function undo() {
        if (historyIndex <= 0) {
            return;
        }

        historyIndex--;
        restoreHistory(historyIndex);
    }


    function redo() {
        if (historyIndex >= history.length - 1) {
            return;
        }

        historyIndex++;
        restoreHistory(historyIndex);
    }


    document.getElementById("undoButton")
        .addEventListener("click", undo);

    document.getElementById("redoButton")
        .addEventListener("click", redo);


    /* ---------------------------
       New Canvas
    --------------------------- */

    document.getElementById("newCanvasButton")
        .addEventListener("click", () => {

            const width =
                Number(canvasWidthInput.value);

            const height =
                Number(canvasHeightInput.value);

            if (
                width < 4 ||
                height < 4 ||
                width > 128 ||
                height > 128
            ) {
                showStatus(
                    "Canvas size must be between 4 and 128."
                );
                return;
            }

            artworkName = "Untitled Artwork";
            artworkTitle.textContent = artworkName;

            setupCanvas(width, height);

            showStatus("New canvas created.");
        });


    /* ---------------------------
       Clear
    --------------------------- */

    document.getElementById("clearButton")
        .addEventListener("click", () => {

            if (
                confirm("Clear the entire artwork?")
            ) {
                clearCanvas(true);
                showStatus("Canvas cleared.");
            }
        });


    /* ---------------------------
       Grid
    --------------------------- */

    gridToggle.addEventListener("change", () => {
        clearCanvas(false);

        const currentImage =
            history[historyIndex];

        if (currentImage) {
            restoreHistory(historyIndex);
        }

        setTimeout(drawGrid, 20);
    });


    /* ---------------------------
       Rename
    --------------------------- */

    document.getElementById("renameButton")
        .addEventListener("click", () => {

            const newName =
                prompt(
                    "Enter artwork name:",
                    artworkName
                );

            if (
                newName &&
                newName.trim()
            ) {
                artworkName =
                    newName.trim();

                artworkTitle.textContent =
                    artworkName;

                showStatus("Artwork renamed.");
            }
        });


    /* ---------------------------
       Save
    --------------------------- */

    document.getElementById("saveButton")
        .addEventListener("click", saveArtwork);


    function saveArtwork() {
        const savedArtworks =
            JSON.parse(
                localStorage.getItem("pixelForgeArtworks") || "[]"
            );

        const artwork = {
            id: Date.now(),
            name: artworkName,
            width: canvasWidth,
            height: canvasHeight,
            image: canvas.toDataURL("image/png"),
            date: new Date().toLocaleString()
        };

        savedArtworks.unshift(artwork);

        localStorage.setItem(
            "pixelForgeArtworks",
            JSON.stringify(savedArtworks)
        );

        renderSavedArtworks();

        showStatus("Artwork saved.");
    }


    /* ---------------------------
       Saved Artwork List
    --------------------------- */

    function renderSavedArtworks() {
        const savedArtworks =
            JSON.parse(
                localStorage.getItem("pixelForgeArtworks") || "[]"
            );

        saveList.innerHTML = "";

        if (savedArtworks.length === 0) {
            saveList.innerHTML =
                `<div class="empty">
                    No saved artworks yet.
                </div>`;

            return;
        }

        savedArtworks.forEach(artwork => {

            const item =
                document.createElement("div");

            item.className = "saved-item";

            item.innerHTML = `
                <div class="saved-name">
                    <strong>${escapeHtml(artwork.name)}</strong>
                    <small>
                        ${artwork.width} × ${artwork.height}
                    </small>
                </div>

                <div class="saved-buttons">
                    <button class="small-button load-btn">
                        Load
                    </button>

                    <button class="small-button delete-btn">
                        Delete
                    </button>
                </div>
            `;

            item.querySelector(".load-btn")
                .addEventListener("click", () => {
                    loadArtwork(artwork);
                });

            item.querySelector(".delete-btn")
                .addEventListener("click", () => {
                    deleteArtwork(artwork.id);
                });

            saveList.appendChild(item);
        });
    }


    function loadArtwork(artwork) {

        canvasWidth = artwork.width;
        canvasHeight = artwork.height;

        canvasWidthInput.value = artwork.width;
        canvasHeightInput.value = artwork.height;

        artworkName = artwork.name;
        artworkTitle.textContent = artworkName;

        setupCanvas(
            artwork.width,
            artwork.height
        );

        const image = new Image();

        image.onload = () => {

            ctx.clearRect(
                0,
                0,
                canvas.width,
                canvas.height
            );

            ctx.drawImage(
                image,
                0,
                0,
                canvas.width,
                canvas.height
            );

            saveHistory();
            drawGrid();

            showStatus(
                "Artwork loaded."
            );
        };

        image.src = artwork.image;
    }


    function deleteArtwork(id) {

        if (!confirm("Delete this saved artwork?")) {
            return;
        }

        let savedArtworks =
            JSON.parse(
                localStorage.getItem("pixelForgeArtworks") || "[]"
            );

        savedArtworks =
            savedArtworks.filter(
                artwork => artwork.id !== id
            );

        localStorage.setItem(
            "pixelForgeArtworks",
            JSON.stringify(savedArtworks)
        );

        renderSavedArtworks();

        showStatus("Artwork deleted.");
    }


    /* ---------------------------
       Download
    --------------------------- */

    document.getElementById("downloadButton")
        .addEventListener("click", () => {

            const exportCanvas =
                document.createElement("canvas");

            const scale = 10;

            exportCanvas.width =
                canvasWidth * scale;

            exportCanvas.height =
                canvasHeight * scale;

            const exportContext =
                exportCanvas.getContext("2d");

            exportContext.imageSmoothingEnabled =
                false;

            exportContext.drawImage(
                canvas,
                0,
                0,
                exportCanvas.width,
                exportCanvas.height
            );

            const link =
                document.createElement("a");

            link.download =
                `${artworkName.replace(/[^a-z0-9]/gi, "_")}.png`;

            link.href =
                exportCanvas.toDataURL("image/png");

            link.click();

            showStatus("PNG downloaded.");
        });


    /* ---------------------------
       Keyboard Shortcuts
       --------------------------- */

    document.addEventListener("keydown", event => {

        if (
            event.ctrlKey &&
            event.key.toLowerCase() === "z"
        ) {
            event.preventDefault();
            undo();
            return;
        }

        if (
            event.ctrlKey &&
            event.key.toLowerCase() === "y"
        ) {
            event.preventDefault();
            redo();
            return;
        }

        if (
            event.target.tagName === "INPUT"
        ) {
            return;
        }

        const key =
            event.key.toLowerCase();

        if (key === "p") {
            setTool("pencil");
        }

        if (key === "e") {
            setTool("eraser");
        }

        if (key === "f") {
            setTool("fill");
        }

        if (key === "i") {
            setTool("picker");
        }
    });


    /* ---------------------------
       Status Message
    --------------------------- */

    let statusTimer;

    function showStatus(message) {

        const status =
            document.getElementById("statusMessage");

        status.textContent = message;
        status.classList.add("show");

        clearTimeout(statusTimer);

        statusTimer =
            setTimeout(() => {
                status.classList.remove("show");
            }, 2200);
    }


    /* ---------------------------
       HTML Safety
    --------------------------- */

    function escapeHtml(text) {
        const div =
            document.createElement("div");

        div.textContent = text;

        return div.innerHTML;
    }


    /* ---------------------------
       Start App
    --------------------------- */

    setupCanvas(32, 32);
    renderSavedArtworks();

</script>

</body>
</html>
```
