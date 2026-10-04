<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fern Browser</title>

<style>
* {
    box-sizing: border-box;
}

html, body {
    margin: 0;
    width: 100%;
    height: 100%;
    font-family: Arial, sans-serif;
    background: #111827;
    color: white;
}

body {
    overflow: hidden;
}

.browser {
    display: flex;
    flex-direction: column;
    height: 100vh;
}

/* ================= TABS ================= */

.tabs {
    display: flex;
    align-items: center;
    gap: 5px;
    height: 43px;
    padding: 5px;
    background: #0b1220;
    border-bottom: 1px solid #263247;
}

.tab {
    display: flex;
    align-items: center;
    gap: 8px;
    min-width: 150px;
    max-width: 230px;
    height: 33px;
    padding: 0 10px;
    background: #172033;
    border-radius: 8px;
    cursor: pointer;
}

.tab.active {
    background: #263449;
}

.tab-title {
    flex: 1;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
}

.close-tab {
    color: #94a3b8;
    cursor: pointer;
}

.close-tab:hover {
    color: white;
}

.new-tab {
    width: 34px;
    height: 33px;
    border: none;
    border-radius: 7px;
    background: #1e293b;
    color: white;
    font-size: 20px;
    cursor: pointer;
}

.new-tab:hover {
    background: #334155;
}

/* ================= TOOLBAR ================= */

.toolbar {
    display: flex;
    align-items: center;
    gap: 7px;
    padding: 8px;
    background: #111827;
    border-bottom: 1px solid #263247;
}

.toolbar button {
    width: 37px;
    height: 35px;
    border: none;
    border-radius: 7px;
    background: #1e293b;
    color: white;
    cursor: pointer;
    font-size: 16px;
}

.toolbar button:hover {
    background: #334155;
}

.address {
    flex: 1;
    height: 35px;
    padding: 0 15px;
    border: 1px solid #334155;
    border-radius: 18px;
    outline: none;
    background: #0f172a;
    color: white;
    font-size: 14px;
}

.address:focus {
    border-color: #60a5fa;
}

.go {
    width: auto !important;
    padding: 0 16px;
    border-radius: 18px !important;
    background: #2563eb !important;
}

/* ================= HOME ================= */

.home {
    flex: 1;
    overflow: auto;
    text-align: center;
    padding: 70px 20px;
}

.logo {
    font-size: 58px;
    font-weight: bold;
    margin-bottom: 8px;
}

.subtitle {
    color: #94a3b8;
    margin-bottom: 30px;
}

.search {
    width: min(650px, 90%);
    height: 52px;
    padding: 0 20px;
    border: 1px solid #334155;
    border-radius: 26px;
    outline: none;
    background: #0f172a;
    color: white;
    font-size: 16px;
}

.search:focus {
    border-color: #60a5fa;
}

.quick-links {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 12px;
    margin-top: 30px;
}

.quick-link {
    padding: 12px 18px;
    background: #1e293b;
    border-radius: 10px;
    cursor: pointer;
}

.quick-link:hover {
    background: #334155;
}

.info {
    margin-top: 35px;
    color: #64748b;
    font-size: 13px;
}

.fullscreen {
    position: fixed;
    right: 15px;
    bottom: 15px;
    width: 43px;
    height: 43px;
    border: none;
    border-radius: 50%;
    background: #2563eb;
    color: white;
    cursor: pointer;
    font-size: 18px;
}
</style>
</head>

<body>

<div class="browser">

    <!-- TABS -->
    <div class="tabs" id="tabs"></div>

    <!-- TOOLBAR -->
    <div class="toolbar">

        <button onclick="goBack()" title="Back">←</button>

        <button onclick="goForward()" title="Forward">→</button>

        <button onclick="reloadPage()" title="Reload">↻</button>

        <button onclick="goHome()" title="Home">⌂</button>

        <input
            id="address"
            class="address"
            type="text"
            placeholder="Search DuckDuckGo or enter a website"
            autocomplete="off"
            spellcheck="false"
        >

        <button class="go" onclick="navigate()">
            Go
        </button>

    </div>

    <!-- HOME -->
    <main class="home">

        <div class="logo">Fern</div>

        <div class="subtitle">
            Search the web with DuckDuckGo
        </div>

        <input
            id="search"
            class="search"
            type="text"
            placeholder="Search DuckDuckGo or enter a website"
            autocomplete="off"
        >

        <div class="quick-links">

            <div
                class="quick-link"
                onclick="openWebsite('https://duckduckgo.com')">
                DuckDuckGo
            </div>

            <div
                class="quick-link"
                onclick="openWebsite('https://www.wikipedia.org')">
                Wikipedia
            </div>

            <div
                class="quick-link"
                onclick="openWebsite('https://github.com')">
                GitHub
            </div>

            <div
                class="quick-link"
                onclick="openWebsite('https://www.google.com')">
                Google
            </div>

        </div>

        <div class="info">
            Searches use DuckDuckGo.
        </div>

    </main>

</div>

<button
    class="fullscreen"
    onclick="toggleFullscreen()"
    title="Fullscreen">
    ⛶
</button>


<script>

/* =====================================
   FERN TABS
===================================== */

let tabs = [
    {
        id: 1,
        title: "Fern",
        url: window.location.href
    }
];

let activeTab = 1;


/* =====================================
   CREATE REAL BROWSER TAB
===================================== */

function createTab() {

    /*
       Opens a REAL browser tab.

       The new tab loads this Fern
       HTML page again.
    */

    const newTab = window.open(
        window.location.href,
        "_blank"
    );

    if (!newTab) {
        alert(
            "Your browser blocked the new tab. " +
            "Allow pop-ups for this page and try again."
        );
    }
}


/* =====================================
   CLOSE FERN TAB BUTTON
===================================== */

function closeTab(id, event) {

    if (event) {
        event.stopPropagation();
    }

    /*
       A normal HTML page cannot close
       arbitrary browser tabs.

       It can only request closing the
       current tab when the browser allows it.
    */

    if (tabs.length === 1) {
        return;
    }

    tabs = tabs.filter(tab => tab.id !== id);

    if (activeTab === id) {
        activeTab = tabs[0].id;
    }

    renderTabs();
}


/* =====================================
   SWITCH INTERNAL FERN TAB
===================================== */

function switchTab(id) {

    activeTab = id;

    const tab = tabs.find(
        tab => tab.id === id
    );

    if (!tab) return;

    if (tab.url) {
        window.open(tab.url, "_blank");
    }
}


/* =====================================
   RENDER TABS
===================================== */

function renderTabs() {

    const container =
        document.getElementById("tabs");

    container.innerHTML = "";

    tabs.forEach(tab => {

        const element =
            document.createElement("div");

        element.className =
            "tab" +
            (tab.id === activeTab
                ? " active"
                : "");

        element.onclick = () =>
            switchTab(tab.id);


        const title =
            document.createElement("span");

        title.className = "tab-title";

        title.textContent =
            tab.title || "New Tab";


        const close =
            document.createElement("span");

        close.className = "close-tab";

        close.textContent = "×";

        close.onclick = event =>
            closeTab(tab.id, event);


        element.appendChild(title);
        element.appendChild(close);

        container.appendChild(element);
    });


    /*
       REAL BROWSER TAB BUTTON
    */

    const newButton =
        document.createElement("button");

    newButton.className = "new-tab";

    newButton.textContent = "+";

    newButton.title =
        "Open Fern in a new browser tab";

    newButton.onclick = createTab;

    container.appendChild(newButton);
}


/* =====================================
   URL / DUCKDUCKGO SEARCH
===================================== */

function normalizeURL(value) {

    value = value.trim();

    if (!value) {
        return "";
    }

    /*
       Direct URL
    */

    if (
        value.startsWith("http://") ||
        value.startsWith("https://")
    ) {
        return value;
    }

    /*
       Website address
    */

    if (
        value.includes(".") &&
        !value.includes(" ")
    ) {
        return "https://" + value;
    }

    /*
       DuckDuckGo search
    */

    return "https://duckduckgo.com/?q=" +
        encodeURIComponent(value);
}


/* =====================================
   OPEN WEBSITE IN REAL BROWSER TAB
===================================== */

function openWebsite(value) {

    const url = normalizeURL(value);

    if (!url) return;

    /*
       Open the destination in a normal
       browser tab. No iframe is used.
    */

    const newTab = window.open(
        url,
        "_blank",
        "noopener,noreferrer"
    );

    /*
       Popup blocker detection
    */

    if (!newTab) {

        alert(
            "Your browser blocked the new tab. " +
            "Allow pop-ups for this page and try again."
        );

        return;
    }
}


/* =====================================
   ADDRESS BAR
===================================== */

function navigate() {

    const value =
        document.getElementById("address").value;

    openWebsite(value);
}


document
    .getElementById("address")
    .addEventListener(
        "keydown",
        function(event) {

            if (event.key === "Enter") {
                navigate();
            }

        }
    );


/* =====================================
   HOME SEARCH
===================================== */

document
    .getElementById("search")
    .addEventListener(
        "keydown",
        function(event) {

            if (event.key === "Enter") {

                openWebsite(this.value);

            }

        }
    );


/* =====================================
   HOME
===================================== */

function goHome() {

    window.location.href =
        window.location.pathname;
}


/* =====================================
   BACK
===================================== */

function goBack() {
    window.history.back();
}


/* =====================================
   FORWARD
===================================== */

function goForward() {
    window.history.forward();
}


/* =====================================
   RELOAD
===================================== */

function reloadPage() {
    window.location.reload();
}


/* =====================================
   KEYBOARD SHORTCUTS
===================================== */

document.addEventListener(
    "keydown",
    function(event) {

        /*
           Ctrl + L
        */

        if (
            event.ctrlKey &&
            event.key.toLowerCase() === "l"
        ) {

            event.preventDefault();

            const address =
                document.getElementById("address");

            address.focus();
            address.select();
        }


        /*
           Ctrl + T

           Opens an actual browser tab.
        */

        if (
            event.ctrlKey &&
            event.key.toLowerCase() === "t"
        ) {

            event.preventDefault();

            createTab();
        }


        /*
           Ctrl + R
        */

        if (
            event.ctrlKey &&
            event.key.toLowerCase() === "r"
        ) {

            event.preventDefault();

            reloadPage();
        }


        /*
           Alt + Left
        */

        if (
            event.altKey &&
            event.key === "ArrowLeft"
        ) {

            event.preventDefault();

            goBack();
        }


        /*
           Alt + Right
        */

        if (
            event.altKey &&
            event.key === "ArrowRight"
        ) {

            event.preventDefault();

            goForward();
        }


        /*
           F11
        */

        if (event.key === "F11") {

            event.preventDefault();

            toggleFullscreen();
        }

    }
);


/* =====================================
   FULLSCREEN
===================================== */

function toggleFullscreen() {

    if (!document.fullscreenElement) {

        document.documentElement
            .requestFullscreen()
            .catch(() => {});

    } else {

        document.exitFullscreen();
    }
}


/* =====================================
   START
===================================== */

renderTabs();

</script>

</body>
</html>

